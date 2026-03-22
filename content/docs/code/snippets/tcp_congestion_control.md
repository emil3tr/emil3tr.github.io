---
title: TCP Congestion Control
sidebar:
    exclude: true
---

```python
import matplotlib.pyplot as plt
import numpy as np

def simulate(losses):
    time = len(losses)
    cwnd = np.zeros(time, dtype=np.float64)
    cwnd[0] = 1
    sst = np.inf
    for i in range(1, time):
        if losses[i] == 1:
            cwnd[i] = cwnd[i-1] * 0.5
            sst = cwnd[i]
        elif losses[i] == 2:
            sst = cwnd[i-1] * 0.5
            cwnd[i] = 1
        elif cwnd[i-1] < sst:
            cwnd[i] = cwnd[i-1] * 2
        else:
            cwnd[i] = cwnd[i-1] + 1
    return cwnd

if __name__ == "__main__":
    losses = np.zeros(100)
    losses[10] = 1
    losses[20] = 2
    losses[30] = 1
    losses[40] = 1
    res = simulate(losses)
    plt.plot(res)
    plt.show()
```