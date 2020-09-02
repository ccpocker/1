---
layout: post
title:  GAN
categories: DL
tags: GAN
author: ccpocker
---


### 1.CapsuleNet
Q:what is capsule?

>A capsule is a group of neurons whose activity vector represents the instantiation parameters of a specific type of entity such as an object or object part. We use the length of the activity vector to represent the probability that the entity exists and its orientation to represent the instantiation paramters.

Q:how does capsule computer?
- input from low-level capsule:$u_i$
- affine transforms:$\hat{u}_{j|i}=W_{ij}u_i$
- weighting sum:$s_j=\sum_i c_{ij}\hat{u}_{j|i}$
- squashing:
  $$v_j=\frac{||s_j||^2}{1+||s_j||^2} \frac{s_j}{||s_j||}$$


