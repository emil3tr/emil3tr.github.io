---
title: Huber vs. Squared Loss
sidebar:
    exclude: true
---

```python {filename="huber_loss.py"}
import matplotlib.pyplot as plt
import numpy as np

x = np.arange(-100, 101, 1, dtype=float) / 100.0
y = x.copy()
y[-1] = 100

delta = 1

def grad_squared_loss(x, w, y):
    return np.dot(-x, y - w*x) / len(x)

def grad_huber_loss(x, w, y):
    mask = np.abs(y - x*w) <= delta
    sum = 0.0
    sum += np.dot(-x[mask], y[mask] - w*x[mask])
    sum += np.dot(-x[mask^True], np.sign(x[mask^True])) * delta
    return sum / len(x)

def gdesc(start, gradient, eta):
    last = start
    for i in range(200):
        start -= gradient(x, start, y) * eta
        if(np.abs(last - start) < 1e-5):
            break
        last = start
    
    return start

res_huber = gdesc(0, grad_huber_loss, 0.5)
res_squared = gdesc(0, grad_squared_loss, 0.5)

evalp = np.linspace(-1, 1, 100)
plt.plot(evalp, evalp*res_huber, label="Huber Loss")
plt.plot(evalp, evalp*res_squared, label="Squared Loss")
plt.plot(x, y, label="Data")
plt.plot(evalp, evalp, label="x=y")
plt.legend()
plt.show()
```