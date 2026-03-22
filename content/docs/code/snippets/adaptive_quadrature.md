---
title: Adaptive Quadrature
sidebar:
    exclude: true
---

```python {filename="adaptive_quad.py"}
import numpy as np
import matplotlib.pyplot as plt
import scipy as sp

def f(x):
    if x < 1:
        return x
    elif x < 2:
        return 1
    elif x < 3:
        return np.sin(10 * x ) * x + 1.0 - np.sin(20) * 2
    else:
        return np.exp(x - 3.0) * 2 + np.sin(30) * 3 - np.sin(20) * 2 - 1

a = 0.0
b = 4.0

def adaptive(f, start, end, iter_left, tol, iter_tot):
    trapez = (end - start) * (f(start) + f(end)) / 2.0
    simpson = (end - start) * (f(start) + 4 * f((start + end)/2) + f(end)) / 6.0
    if iter_left <= 1:
        return simpson
    err = np.abs(trapez - simpson)
    if err < tol or tol < 1e-16:
        return simpson
    mid = (start + end) / 2.0
    plt.scatter(mid, (iter_tot - iter_left) * 0.2, color='red')
    return adaptive(f, start, mid, iter_left - 1, tol / 2.0, iter_tot) + adaptive(f, mid, end, iter_left - 1, tol / 2.0, iter_tot)

plt_pts = np.linspace(a,b,1000)
fplt = np.zeros(1000)
for i in range(1000):
    fplt[i] = f(plt_pts[i])
print(adaptive(f, 0, 4, 10, 1e-2, 8))
plt.plot(plt_pts, fplt)
plt.show()
```