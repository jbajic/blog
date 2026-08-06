---
layout: post
title: Product Quantization
categories: [vector, ann, quantization, storage]
---

Quantization is a lossy compression done on vectors. And PQ (Product Quantization) is a type of
compression that reduces the dimension of a vector, which effectively loses information.

Lets define the PQ as `PQ{M x kBits}`, where `M` is the number of subspaces we want to split the original
vector one in, and `kBits` is the number of bits we want to use to represent the centroids in a given
subspace calculated as 2^kBits.
If we were to use `PQ16x8`, where `M = 16` and, `kBits = 8` on vectors of dimension `D = 128` we would
split the into 16 subspaces `kSub = D / M = 16`. Every subspace has `2^kBits = 2^8 = 256` centroids, and centroid is
represented by kSub dimensional vectors of type `float32`.

## PQ Codebook
All of that information about centroids from every subspace in stored in PQ codebook, and using that
information we can visualize how does the codebook looks like and calculate how much space it takes.

So for a PQ16x8, and vectors D = 128, we have kSub = D / M = 8. So the format of
the codebook is that for every subspace (kSub subspaces) we have 2^kBits centroids, i.e.:
```math
M: number of subspaces
2^kBits: number of centroids per subspace
kSub: dimension of each subspace
codebook_bytes = M * 2^kBits * kSub * size_of(float32)
= M * 2^kBits * D / M * size_of(float32)
= 2^kBits * D * size_of(float32)
= 2^8 * 128 * 32
= 1048576 B = 1 MB
```
The whole codebook takes only 1 MB, and it depends only on the number of centroids and the vector
dimension.

And from this we can derive how the codebook looks like, for every subspace it has 256 centroids
defined by vectors, so using C++ semantics it could be represented just like this:
```
{
    subspace_1: { centroid_1, ..., centroid_256},
    ...
    subspace_16: { centroid_1, ..., centroid_256}
}
```

## Creating PQ Codebook
The M and kBits are part of the PQ definition, but the centroids that are stored in the codebook
must be trained. If you need 256 centroids you need to have at least 256 vectors, which makes the
training trivial and useless but that is the bare minimum. FAISS proposes having 32 vectors per
centroid minimum and 256 maximum, meaning if you pick less then 32 you would get a warning and
picking more then 256 per centroid would just cut of the rest of the vectors.

Centroids are trained using [k-means](https://en.wikipedia.org/wiki/K-means_clustering) clustering
algorithm, which partitions the given vectors into k clusters by minimazing the total Euclidian
squared distance between the vectors and the centroid closest to them.

## Using PQ Codebook
So now that we have trained the PQ codebook and have all the information, how do we actually use the
codebook to convert the incoming vectors to the PQ form.

For the same setup as above we do the following, for every subspace M we calculate the closest
centroid in that subspace for a given vector. So in our case of 128 dimensional vector split into M
subspaces we compare every M-th part of vector against 256 centroids and pick the closes one, and
just remember the index of the closest centroid so the transformation is effectively like this:

`[i_0, ..., i_128]` where every i_n is `float32` is converted to this:
`[j_0, ..., j_16]` where every j_n is a number of size `kBits`, in this the size is 1B
Therefore the reduction in size is from `128 * size_of(float32) = 512 B` to `16 * 1 = 16 B` which is
exactly 32 times less.

## References
 - [Product Quantization for Nearest Neighbor Search](https://inria.hal.science/inria-00514462v2/document)
 - [FAISS](https://github.com/facebookresearch/faiss/wiki)
