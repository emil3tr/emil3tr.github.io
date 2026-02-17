---
title: Kapitel 3
weight: 3
---

Wir wollen trigonometrische Polynome verwenden, um das Runge Phänomen zu vermeiden. Ein **trigonometrisches Polynom** kann man in der reellen
$$
    p_m(t) = \frac{a_0}{2} + \sum_{k=1}^m (a_k \cos(2 \pi k t) + b_k \sin(2 \pi k t))
$$

oder in der komplexen Form 
$$
    p_m(t) = \sum_{k=-m}^m c_k e^{2 \pi ikt}
$$

schreiben. Beide sind äquivalent, nur andere Darstellungen. Hier ist $t \in [0,1)$ und die Endpunkte $0,1$ äquivalent. Meistens nehmen wir **equally-spaced samples**
$$
    (t_j, f(t_j)), \quad t_j = \frac{j}{N}, \quad j = 0, \dots, N-1
$$

Mit N samples können wir aber nur circa halb so viele Frequenzen darstellen. Das **Nyquist-Limit** sagt, dass wir mit N samples höchstens ein polynom von Grad
$$
    \deg(p) = m = \left \lfloor \frac{N-1}{2} \right \rfloor
$$

Dieses Polynom geht genau durch die Messpunkte. Der Nyquist-Effekt passiert, weil wir die Frequenzen $k$ und $N-k$ nicht unterscheiden können. Die Frequenzen über $N/2$ geben dieselben Cosinus und negative Sinus Werte.

Für die Messpunkte bekommen wir die Gleichungen
$$
    y_j = \sum_k c_k e^{2 \pi ikt_j} 
$$

und dieses System können wir auf äquidistanten Punkten lösen.

## Orthogonalität

Das dot-product von zwei Funktionen misst, wie sehr sie überlappen.
$$
    \langle f,g \rangle = \int_0^1 f(t)g(t)dt
$$

Es gilt für $k,l \geq 1$ falls $k=l$
$$
    \int_0^1 \cos(2\pi kt) \cos(2\pi lt) dt = \int_0^1 \sin(2\pi kt) \sin(2\pi lt) dt = \frac{1}{2}
$$

und für $k \neq l$
$$
    \int_0^1 \cos(2\pi kt) \cos(2\pi lt) dt = \int_0^1 \sin(2\pi kt) \sin(2\pi lt) dt = 0
$$

und immer
$$
    \int_0^1 \cos(2\pi kt) \sin(2\pi lt) dt = 0
$$

Das bedeutet, dass die Frequenzen "unabhängig" sind und wir sie deshalb einzelnd messen können. Wir können das jetzt verwenden, um die Koeffizienten unseres Polynoms zu finden. Wenn wir $f \to [0,1]$ mit $f(0)=f(1)$ haben und das Polynom von oben
$$
    f(t) \sim \frac{a_0}{2} + \sum_{k=1}^\infty (a_k \cos(2 \pi k t) + b_k \sin(2 \pi k t))
$$

finden wollen, können wir verwenden, dass
$$
    a_0 = 2 \int_0^1 f(t)dt, \quad a_k = 2 \int_0^1 f(t) \cos(2\pi kt)dt, \quad b_k = 2 \int_0^1 f(t) \sin(2\pi kt)dt
$$

Dann gibt $a_0$ den Durschnittswert der Funktion an und die $a_k, b_k$ messen, wie stark die Funktion mit den einzelnen Frequenzen korreliert. (Also wie viel von jeder Frequenz in der Funktion "enthalten" ist.)

## Komplexe Form

Statt der reellen Form wie oben können wir auch
$$
    f(t) \sim \sum_{k=-\infty}^{\infty} c_k e^{2\pi ikt}
$$

schreiben. Dann finden wir
$$
    c_k = \int_0^1 f(t) e^{-2\pi ikt} dt
$$

und
$$
    c_0 = \frac{a_0}{2}, \quad c_k = \frac{a_k - ib_k}{2}, \quad c_{-k} = \overline{c_k}
$$

## Diskrete Fourier Transformation

Die DFT lässt uns die Frequenzen aus endlich vielen Messpunkten berechnen. Wir wollen aus $N$ Messpunkten das Polynom
$$
    p(t) = \sum_{-m}^m c_ke^{2\pi ikt}, \quad m = \left \lfloor \frac{N-1}{2} \right \rfloor, \quad p(t_j) = y_j
$$

finden. Dafür verwenden wir die $N$ **Einheitswurzeln**
$$
\omega_N = e^{-2\pi i /N}
$$

welche gleichmässig auf dem Einheitskreis liegen. (Intuitiv gehen wir bei $\omega_N$ eine n-tel Umdrehung auf dem Einheitskreis.) Diese haben die Eigenschaft
$$
    \sum_{j=0}^{N-1} \omega_N^{jk} = N \; \text{if} \; k \equiv_N 0 \; \text{and else} \; 0
$$

und mit ihnen schreiben wir die **Fourier-Matrix**
$$
    F_N[i,j] = \omega_N^{ij}
$$

Wir erhalten die Fourier-Koeffizienten $c = (c_0, \dots, c_{N-1})$ aus den Messpunkten $y = (y_0, \dots, y_{N-1})$ mit
$$
    c = F_N * y
$$

und invers die Messpunkte mit
$$
    y = \frac{1}{N} F_N^H c
$$

Die DFT kostet $O(N^2)$, mit der FFT ist es in $O(N\log(N))$ möglich. Es gilt ausserdem
$$
    \sum_{j=0}^{N-1} |y_j|^2 = \frac{1}{N} \sum_{j=0}^{N-1} |c_k|^2
$$

## Schnelle Fourier Transformation

Wir wollen die Fourier-Koeffizienten in $O(N\log(N))$ berechnen. Dazu teilen wir die Messpunkte in gerade und ungerade auf und verwenden divide-and-conquer. Das berechnete Resultat ist aber dasselbe. Python verwendet in den implementierungen die FFT.

## In Python

Eine Anwendung der Fourier Transformation is das **Filtern**. Wir können ein Signal zuerst in die Frequenzen übersetzen und dann filtern.

```
y <- samples

coeffs = np.fft.fft(y)

Do some filtering.

filtered_y = np.fft.ifft(y)
```

Wenn wir *np.fft.fft* benutzen, bekommen wir die Frequenzwerte in einem Array [positive low - positive high - negative high - negative low]. Hier ist der Mittelpunkt bei $N/2$ und die negative Seite spiegelt die positive. Die funktion *np.fft.fftshift* ordnet sie stattdessen nicht-absteigen sortiert, also [negative high - negative low - positive low - positive high]

Falls wir eine gerade Anzahl an Messpunkten haben, können wir die Funktion wie folgt auf N Punkten auswerten. Falls wir die Ableitung wollen, müssen die drei eingerückten Zeilen auskommentiert werden.

```
y <- even number of samples
N <- number of equally spaced evaluation points

n = len(y)
m = n // 2
c = fft.fft(y) / n
    # mult = np.arange(n)
    # mult[m:] = np.arange(-m, 0, 1)
    # c *= mult * np.pi * 2 * 1.j
v = np.zeros(N, dtype=complex)
v[:m] = c[:m]
v[N-m:] = c[m:]
v = fft.ifft(v) * N 

result <- v
```

**Achtung:** Statt mit n und N zu dividierten und multiplizieren, können wir auch zuerst *ifft(y)* und dann *fft(v)* verwenden. Das ist schneller, aber dann müssen wir bei der Ableitung c mit -1 multiplizieren!

## Fehler

Der Fehler konvergiert **algebraisch**, falls $E(n) = O(1/n^{p})$ für $p > 0$ und **exponentiell**, falls $E(n) = O(q^n)$ für $0 \geq q < 1$.

Falls die Funtion $f$ 1-periodisch ist und die Summe der Fourier-Koeffizienten $\hat{f}(k)$ absolut konvergiert, können wir abschätzen
$$
| p_N(x) - f(x) | \leq 2 \sum_{|k| \geq N/2} |\hat{f}(k)|
$$

Falls $f$ 1-periodisch ist und die maximale Frequenz $m$ ist, dann können wir sie genau approximieren, falls $N > 2m$. Also wenn $\hat{f}(k) = 0$ für $|k| > m$ gilt dann $p_N(x) = f(x)$

## Mehr

+ Je glatter eine Funktion, desto schneller konvergiert sie zu ihrer Fourier-Reihe
+ Mit Lösen eines linearen Gleichungssystems können wir die Fourier-Koeffizienten **immer** in $O(n^3)$ berechnen. Für **äquidistante** Punkte geht es mit FFT in $O(n\log(n))$.