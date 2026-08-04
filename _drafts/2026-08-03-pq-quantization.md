---
layout: post
title: Product Quantization
categories: [vector, ann, quantization, storage]
---

## PQ quantization

Quantization is a lossy compression done on vectors. And PQ (Product Quantization) is a type of
compression that reduces the dimension of a vector, which effectively loses information.

So given a vector of dimension, lets define the PQ as `PQMxkBits`, where `M` is the number of
subspaces we want to split the original one in, and `kBits` is the number of bits we want to use to
represents the centroids in a given subspace calculated as 2^kBits.
If we were to use `PQ16x4`, where `M = 16` and, `kBits = 4` on vectors if `D = 128` we would split the vector of 128
dimension into 16 subspaces `kSub = 16`. Every subspace has `2^kBits` centroids and centroid is
represented by `M * float16` vectors.

## Refferences
 - [Product Quantization for Nearest Neighbor Search](https://inria.hal.science/inria-00514462v2/document)
