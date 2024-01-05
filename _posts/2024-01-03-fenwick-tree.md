---
layout: post
title:  A Python implementation of Fenwick Tree (Binary Index Tree)
date:   2024-01-03 4:28:19
description:
tags: algorithm
categories: algorithm
---

There is a good tutorial about the Fenwick Tree (Binary Index Tree); here is the link <a href="https://en.oi-wiki.org/ds/fenwick/">https://en.oi-wiki.org/ds/fenwick/</a>.


```python
n = 1 << 20
fenwick = [0] * (n +1)

def lowbit(x):
    return x & -x

def update(index, d):
    while index <= n:
        fenwick[index] += d
        index += lowbit(index)

def getsum(index):
    res = 0
    while index > 0:
        res += fenwick[index]
        index -= lowbit(index)
    return res
```
