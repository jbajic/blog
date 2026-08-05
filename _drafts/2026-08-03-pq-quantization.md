---
layout: post
title: Product Quantization
categories: [vector, ann, quantization, storage]
---

## PQ quantization

Quantization is a lossy compression done on vectors. And PQ (Product Quantization) is a type of
compression that reduces the dimension of a vector, which effectively loses information.

Lets define the PQ as `PQ{M x kBits}`, where `M` is the number of subspaces we want to split the original
vector one in, and `kBits` is the number of bits we want to use to represent the centroids in a given
subspace calculated as 2^kBits.
If we were to use `PQ16x8`, where `M = 16` and, `kBits = 8` on vectors of dimension `D = 128` we would
split the into 16 subspaces `kSub = D / M = 16`. Every subspace has `2^kBits = 2^8 = 256` centroids, and centroid is
represented by kSub dimensional vectors of type `float32`.

### PQ Codebook
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

### Creating PQ Codebook

## References
 - [Product Quantization for Nearest Neighbor Search](https://inria.hal.science/inria-00514462v2/document)
