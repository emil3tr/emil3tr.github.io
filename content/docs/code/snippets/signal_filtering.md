---
title: Signal Filtering
sidebar:
    exclude: true
---

```python {filename="signal_filtering.py"}
import numpy as np
import matplotlib.pyplot as plt

def f(x):
    return np.abs(x-0.5) * np.sin(10*np.pi*x)


tt = np.linspace(0,1,1000)
n = 1000
errors = (np.random.rand(n) - 0.5)
eval_pts = np.linspace(0,1,n)

y = f(eval_pts) + errors

freqs = np.abs((np.fft.fft(y)**2) / n)
c = np.fft.fft(y)
filtered_c = np.fft.fft(y)
filtered_c[freqs < np.max(freqs) * 0.2] = 0

filtered_y = np.fft.ifft(filtered_c)

plt.plot(eval_pts, y, color="pink")
plt.plot(tt, f(tt), color="red")
plt.plot(eval_pts, filtered_y, color="blue")
plt.show()
```