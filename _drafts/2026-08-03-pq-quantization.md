---
layout: post
title: Product Quantization
categories: [vector, ann, quantization, storage]
---

Quantization is a lossy compression done on vectors. And PQ (Product Quantization) is a type of
compression that reduces the dimension of a vector, which effectively loses information.

Let's define PQ as $\text{PQ}\lbrace M \times k_{bits} \rbrace$, where $M$ is the number of subspaces we want to
split the original vector into, and $k_{bits}$ is the number of bits we want to use to represent the
centroids in a given subspace, calculated as $2^{k_{bits}}$.
If we were to use $\text{PQ16x8}$, where $M = 16$ and $k_{bits} = 8$, on vectors of dimension
$D = 128$, we would split it into $M = 16$ subspaces, each of dimension $k_{sub} = D / M = 8$. Every
subspace has $2^{k_{bits}} = 2^8 = 256$ centroids, and each centroid is represented by a
$k_{sub}$-dimensional vector of type `float32`.

## PQ Codebook
All of that information about centroids from every subspace is stored in the PQ codebook, and using
that information we can visualize what the codebook looks like and calculate how much space it takes.

So for $\text{PQ16x8}$ and vectors of dimension $D = 128$, we have $k_{sub} = D / M = 8$. So the
format of the codebook is that for every subspace (there are $M$ subspaces) we have $2^{k_{bits}}$
centroids, each a $k_{sub}$-dimensional vector. The total size is then:

$$
\begin{aligned}
\text{codebook bytes}
  &= M \cdot 2^{k_{bits}} \cdot k_{sub} \cdot \operatorname{sizeof}(\text{float32}) \\
  &= M \cdot 2^{k_{bits}} \cdot \frac{D}{M} \cdot \operatorname{sizeof}(\text{float32}) \\
  &= 2^{k_{bits}} \cdot D \cdot \operatorname{sizeof}(\text{float32}) \\
  &= 2^{8} \cdot 128 \cdot 4 \\
  &= 131072\ \text{B} = 128\ \text{KB}
\end{aligned}
$$

The whole codebook takes only 128 KB, and it depends only on the number of centroids and the vector
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
The $M$ and $k_{bits}$ are part of the PQ definition, but the centroids that are stored in the
codebook must be trained. If you need 256 centroids you need to have at least 256 vectors, which
makes the training trivial and useless but that is the bare minimum. FAISS proposes having 32 vectors
per centroid minimum and 256 maximum, meaning if you pick less than 32 you would get a warning and
picking more than 256 per centroid would just cut off the rest of the vectors.

Centroids are trained using the [k-means](https://en.wikipedia.org/wiki/K-means_clustering)
clustering algorithm, which partitions the given vectors into $k$ clusters by minimizing the total
squared Euclidean distance between the vectors and the centroid closest to them.

## Using PQ Codebook
So now that we have trained the PQ codebook and have all the information, how do we actually use the
codebook to convert the incoming vectors to the PQ form.

For the same setup as above we do the following: for each of the $M$ subspaces we calculate the
closest centroid in that subspace for a given vector. So in our case of a 128-dimensional vector
split into $M$ subspaces, we compare every $M$-th part of the vector against 256 centroids and pick
the closest one, and just remember the index of the closest centroid. So the transformation is
effectively like this:

$$[i_0, \dots, i_{128}] \longrightarrow [j_0, \dots, j_{16}]$$

where every $i_n$ is a `float32` which of size 4B and every $j_n$ is a number of size $k_{bits}$ of size 1B.
Therefore the reduction in size is from $128 \cdot \operatorname{sizeof}(\text{float32}) = 512\ \text{B}$
to $16 \cdot 1 = 16\ \text{B}$, which is exactly 32 times less.

## References
 - [Product Quantization for Nearest Neighbor Search](https://inria.hal.science/inria-00514462v2/document)
 - [FAISS](https://github.com/facebookresearch/faiss/wiki)
