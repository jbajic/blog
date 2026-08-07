---
layout: post
title: Product Quantization
categories: [vector, ann, quantization, storage]
---

Quantization is a lossy compression done on vectors. And PQ (Product Quantization) is a type of
compression that reduces the vector into a multiple subspaces represented by centroids, which effectively loses information.

Let's define PQ as $\text{PQ}\lbrace M \times kBits \rbrace$, where $M$ is the number of subspaces we want to
split the original vector into, and $kBits$ defines the number of centroids in each subspace calculated as $2^{kBits}$.
If we were to use $\text{PQ16x8}$, where $M = 16$ and $kBits = 8$, on vectors of dimension
$D = 128$, we would split it into $M = 16$ subspaces, each of dimension $kSub = D / M = 8$. Every
subspace has $2^{kBits} = 2^8 = 256$ centroids, and each centroid is represented by a
$kSub$-dimensional vector of type `float32`.

## PQ Codebook

All of that information about centroids from every subspace is stored in the PQ codebook, and using
that information we can visualize what the codebook looks like and calculate how much space it takes.

So for $\text{PQ16x8}$ and vectors of dimension $D = 128$, we have $kSub = D / M = 8$. So the
format of the codebook is that for every subspace (there are $M$ subspaces) we have $2^{kBits}$
centroids, each a $kSub$-dimensional vector. The total size is then:

$$
\begin{aligned}
\text{codebook bytes}
  &= M \cdot 2^{kBits} \cdot kSub \cdot \operatorname{sizeof}(\text{float32}) \\
  &= M \cdot 2^{kBits} \cdot \frac{D}{M} \cdot \operatorname{sizeof}(\text{float32}) \\
  &= 2^{kBits} \cdot D \cdot \operatorname{sizeof}(\text{float32}) \\
  &= 2^{8} \cdot 128 \cdot 4 \\
  &= 131072\ \text{B} = 128\ \text{KiB}
\end{aligned}
$$

The whole codebook takes only 128 KiB, and it depends only on the number of centroids and the vector
dimension.

And from this we can derive what the codebook looks like: for every subspace it has 256 centroids
defined by vectors, so using C++ semantics it could be represented just like this:
```
{
    subspace_1:  { centroid_1, ..., centroid_256 },
    ...
    subspace_16: { centroid_1, ..., centroid_256 }
}
```

## Creating PQ Codebook

The $M$ and $kBits$ are part of the PQ definition, but the centroids that are stored in the
codebook must be trained. If you need 256 centroids you need to have at least 256 vectors, which
makes the training trivial and useless but that is the bare minimum. FAISS proposes having minimum of 39 training vectors
per centroid and 256 maximum, meaning if you pick less than 39 you would get a warning and
picking more than 256 per centroid would just cut off the rest of the vectors.

Centroids are trained using the [k-means](https://en.wikipedia.org/wiki/K-means_clustering)
clustering algorithm, which partitions the given vectors into $k$ clusters by minimizing the total
squared Euclidean distance between the vectors and the centroid closest to them. K-means is run for
every subspace independently, meaning it is run $M$ times.

## Converting vector to PQ form

So now that we have trained the PQ codebook and have all the information, how do we actually use the
codebook to convert the incoming vectors to the PQ form.

For the same setup as above we do the following: for each of the $M$ subspaces we calculate the
closest centroid in that subspace for a given vector. So in our case of a 128-dimensional vector
split into $M$ subspaces, and for each $M$ subspace of the vector compare against 256 centroids
and pick the closest one, and just remember the index of the chosen centroid. So the transformation is
effectively like this:

$$[i_1, \dots, i_{128}] \longrightarrow [j_1, \dots, j_{16}]$$

where every $i_n$ is a `float32` of size 4B and every $j_n$ is a number of size $kBits$ which in the case above is 1B.
Therefore the reduction in size is from $128 \cdot \operatorname{sizeof}(\text{float32}) = 512\ \text{B}$
to $16 \cdot 1 = 16\ \text{B}$, which is exactly 32 times less.

## Calculating distance in PQ form

To calculate the distance between two vectors in full form we need to sum their squared differences
across all dimensions. For vectors x,y dimension 128 the calculation looks like this:

$$d(x, y)^2 = (x_1 - y_1)^2 + \dots + (x_{128} - y_{128})^2$$

But when they are both in PQ form we sum the distance between the centroids of the vectors. So for
two vectors both in PQ form, the calculation of distance between them is called symmetrical distance
and it is calculated as a sum of distances between their respective centroids which are also vectors:

$$d(x, y)^2 = ||x_{c1} - y_{c1}||^2 + \dots + ||x_{c16} - y_{c16}||^2$$

where every $||x_{ci} - y_{ci}||^2$ is still $(x_{ci1} - y_{ci1})^2 + \dots + (x_{ci8} -
y_{ci8})^2$, which still does not reduce the amount of calculation that is needed.

But that would be naive approach, since we already know the centroids in advance and we can use that
to build the distance matrix between centroids in advance to reduce the computation needed, that matrix
is SDC(symmetrical distance) table, $M \cdot 2^{kBits} \cdot 2^{kBits}$. For every subspace we calculate the
distance between each centroid, which looks like this:
```
{
  subspace_1: {
    centroid_1: [distance(centroid_1, centroid_1), ..., distance(centroid_1, centroid_256)],
    ...
    centroid_256: [distance(centroid_256, centroid_1), ..., distance(centroid_256, centroid_256)]
  },
  ...
  subspace_16: {
    centroid_1: [distance(centroid_1, centroid_1), ..., distance(centroid_1, centroid_256)],
    ...
    centroid_256: [distance(centroid_256, centroid_1), ..., distance(centroid_256, centroid_256)]
  }
}
```

where every $distance(centroid_n, centroid_m)$ is
$d(c_n, c_m)^2 = (c_{n1} - c_{m1})^2 + \dots + (c_{n8} - c_{m8})^2 = ||c_n - c_m||^2$.
This is $16 \dots 256 \cdot 256$ entries of distances which is 4MiB in this case. Now the computation of
distances can be simplified to just summing of 16 distances:

$$d(x,y)^2 = \sum_{n=1}^{16} SDC[n][cx_n][cy_n]$$

where $cx_n$ and $cy_n$ are respective centroid ids from $x$ and $y$ vectors.

But if one vector $x$ is in PQ form and $y$ in full form we calculate asymmetric distance, we just
split the $y$ into $M$ subvectors and build the ADC(asymmetric distance) table for the $y$. In this
case the ADC table consists of only $M \cdot 2^{kBits}$ entries since we calculate only the
distances between the each of the vectors subvectors against the centroids, so the table looks like
this:
```
{
  subspace_1: [distance(y_1, centroid_1), ..., distance(y_1, centroid_256)],
  ...
  subspace_16: [distance(y_16, centroid_1), ..., distance(y_16, centroid_256)]
}
```

which is a much smaller matrix basically containing $16 \cdot 256 \cdot 4 = 16384 B = 16KiB$. This again reduces
the calculation to just summing of 16 distances.

$$d(x, y)^2 = \sum_{n=1}^{16} ADC[n][cx_n]$$

We see that the ADC table is much smaller but it is reconstructed per query vector, while SDC is true
for the whole dataset. Also in ADC only one vector is quantized which makes the distance error computation
between vectors smaller.

## Conclusion

So for a PQ16x8 and vectors dimension $D = 128$ we have the following structures that we will probably need:
- PQ codebook: $2^{kBits} \cdot D \cdot \operatorname{sizeof}(\text{float32}) = 128 KiB$
- SDC table: $M \cdot 2^{kBits} \cdot 2^{kBits} \cdot \operatorname{sizeof}(\text{float32})= 4MiB $
- ADC table: $M \cdot 2^{kBits} \cdot \operatorname{sizeof}(\text{float32}) = 16 KiB$

and for that we reduce the size of each vectors from $D \cdot \operatorname{sizeof}(\text{float32}) = 512 B$ to $M \cdot
\frac{kBits}{8} = 16 B$.

## References

 - [Product Quantization for Nearest Neighbor Search](https://inria.hal.science/inria-00514462v2/document)
 - [FAISS](https://github.com/facebookresearch/faiss/wiki)
