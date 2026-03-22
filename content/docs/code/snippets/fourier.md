---
title: Fourier Approximation
sidebar:
    exclude: true
---

```python {filename="fourier.py"}
import matplotlib.pyplot as plt
import numpy as np

def f(x):
    mask = x < 0.3
    mask2 = np.logical_and(x >= 0.3 , x < 0.7)
    mask3 = x >= 0.7
    out = np.zeros_like(x)
    out[mask] = 0
    out[mask2] = 1
    out[mask3] = 2 - 5*np.exp(x[mask3] - 0.7)
    return out

def base(k, x):
    return np.exp(np.pi * 2 * 1.j * k * x)

def inter(c, x):
    m = (len(c) - 1)//2
    out = np.zeros_like(x, dtype=complex)
    for i in range(-m, m+1, 1):
        out += base(i, x) * c[i]
    return out

a = 0.0
b = 1.0
tt = np.linspace(a,b,1000)
n = 20

for j in range(4, n, 4):
    p = np.linspace(a,b,j)
    c = np.fft.fft(f(p)) / j
    plt.plot(tt, f(tt))
    plt.plot(tt, np.real(inter(c, tt)))

plt.show()
```