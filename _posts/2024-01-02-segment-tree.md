---
layout: post
title:  A Python implementation of Segment Tree with range updates and lazy propagation
date:   2024-01-02 4:28:19
description:
tags: algorithm
categories: algorithm
---

There is a good tutorial about the SegTree on Codeforce; here is the link: <a href="https://codeforces.com/blog/entry/18051">https://codeforces.com/blog/entry/18051</a>. My code highly relies on it.


```python
class SegTree:
    def __init__(self, n, update_func, query_func):
        self.n = n
        self.h = 1
        while 1 << self.h < n:
            self.h += 1
        self.update_func = update_func
        self.query_func = query_func
        self.tree = [0] * (2 * self.n)
        self.lazy = [0] * (self.n)

    def _apply(self, p, val):
        self.tree[p] = self.update_func(self.tree[p], val)
        if p < self.n:
            self.lazy[p] = self.update_func(self.lazy[p], val)

    def _pull(self, p):
        while p > 1:
            p >>= 1
            self.tree[p] = self.query_func(self.tree[p << 1], self.tree[p << 1 | 1])
            self.tree[p] = self.update_func(self.tree[p], self.lazy[p])

    def _push(self, p):
        for s in range(self.h, 0, -1):
            i = p >> s
            if self.lazy[i]:
                self._apply(i << 1, self.lazy[i])
                self._apply(i << 1 | 1, self.lazy[i])
                self.lazy[i] = 0

    def update(self, l, r, val):
        l += self.n
        r += self.n
        l0, r0 = l, r
        while l < r:
            if l & 1:
                self._apply(l, val)
                l += 1
            if r & 1:
                r -= 1
                self._apply(r, val)
            l >>= 1
            r >>= 1
        self._pull(l0)
        self._pull(r0 - 1)

    def query_single(self, p):
        self._push(p)
        return self.tree[p]

    def query(self, l, r):
        l += self.n
        r += self.n
        self._push(l)
        self._push(r)
        res = 0
        while l < r:
            if l & 1:
                res = self.query_func(res, self.tree[l])
                l += 1
            if r & 1:
                r -= 1
                res = self.query_func(res, self.tree[r])
            l >>= 1
            r >>= 1
        return res
```