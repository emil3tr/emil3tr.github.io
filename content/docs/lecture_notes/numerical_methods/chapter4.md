---
title: Kaptitel 4
weight: 4
---

Wir wollen nun stückweise interpolieren. Dazu unterteilen wir das Interval in mehrere Teile und interpolieren auf jedem einzelnd.

## Lineare stückweise Interpolation

Wir haben $N+1$ Punkte $(x_j, y_j)$ gegeben und arbeiten auf $N$ Intervallen (jeweils zwischen den Punkten). Dann ist der Interpolant für jedes Intervall $[x_{j-1}, x_j]$
$$
    s_j(x) = y_{j-1} \frac{x_{j-1} - x}{h_j} + y_j \frac{x - x_j}{h_j}
$$

wobei $h_j = x_j - x_{j-1}$ die Länge des Intervalls ist. Ausserhalb des Intervalls ist er null.

**Eigenschaften:** *Überall stetig, aber an den Knackpunkten (möglicherweise) nicht differenzierbar.

## Kubische Hermite-Interpolation

Wir brauchen jetzt nicht mehr nur die Funktionswerte $y_j$ an den Messpunkten $x_j$, sondern auch die Werte der ersten Ableitung $c_j$. Haben wir diese gegeben, ergeben sich für jedes Intervall $[x_{j-1}, x_j]$ vier Bedingungen
$$
    s(x_{j-1}) = y_{j-1}, \; s(x_j) = y_j, \; s'(x_{j-1}) = c_{j-1}, \; s'(x_j) = c_j
$$

Das bedeutet, wir können einen Interpolanten von Grad drei bauen! Um die Voraussetzung an die Funktionswerte zu erfüllen, verwenden wir
$$
    \varphi(t) = t^2(3 - 2t), \quad \varphi'(t) = 6t(1-t)
$$

Wir haben $\varphi(0) = 0$, $\varphi(1) = 1$ und $\varphi'(0) = \varphi'(1) = 0$. Somit hat sie keinen Einfluss auf die Ableitungen an den Randpunkten. Unser erster Teil ist also
$$
    p_j(x) = y_{j-1} \varphi(\frac{x - x_{j-1}}{h_j}) + y_j \varphi(\frac{x_j - x}{h_j})
$$

wobei die Variablen gleich wie oben sind und ausserhalb von $[x_{j-1}, x_j]$ die Funktion verschwindet.

Für die Ableitungen verwenden wir
$$
    \psi(t) = t^2(t-1), \quad \psi'(t) = t(3t-2)
$$

Wir haben $\psi(0) = \psi(1) = 0$ und $\psi'(0)=0$, $\psi'(1)=1$. Wir müssen auf die Kettenregel aufpassen und bekommen dann
$$
    q_j(x) = c_{j-1} h_j \psi(\frac{x - x_{j-1}}{h_j}) + c_j h_j \psi(\frac{x_j - x}{h_j})
$$

Das Interpolationspolynom in dem Intervall ist dann
$$
    s_j(x) = p_j(x) + q_j(x)
$$

Falls wir die Ableitungen nicht gegeben haben, können wir sie selbst wählen. Dafür gibt es verschiedene Methoden, zum Beispiel *Akima* und *PCHIP*.

**Eigenschaften:** Überall stetig differenzierbar. PCHIP ist auch *formerhaltend* (wenn die Ursprungsfunktion lokal monoton ist, ist es der Interpolant auch).

## Splines

Wir wollen nun wieder nur mit den Funktionswerten an den Messpunkten interpolieren. Sei $G = \{a=x_0, x1, \dots, x_N=b\}$ das Gitter auf dem Intervall $[a,b]$. Der Raum der **Splines** von Grad $d$ (oder Ordnung $d+1$) ist
$$
    \mathcal{S}_{d,G} = \{ s \in C^{d-1}[a,b], \space s_j := s_{[x_{j-1}, x_j]} \space \text{ist ein Polynom vom Grad höchstens} \space d \}
$$

Es sind also alle stückweise Funktionen, die (d-1)-mal stetig differenzierbar sind und aus Polynomen von höchstens Grad $d$ bestehen. $\mathcal{S}_{d,G}$ ist ein linearer Raum. Seine Freiheitsgrade sind
$$
    \dim \mathcal{S}_{d,G} = (d+1)N - (N-1)d = N + d
$$

Wenn wir kubische Splines mit $d=3$ verwenden wollen, dann brauchen wir also noch zwei Bedingungen, da wir nur $N+1$ Punkte haben. Nur dann erhalten wir ein eindeutig lösbares lineares Gleichungssystem. Es gibt folgende kubische Spline Interpolationen:

+ **vollständige:** $s'(x_0)=c_0$ und $s'(x_N)=c_N$
+ **natürliche:** $s''(x_0) = s''(x_N) = 0$
+ **periodische:** $s'(x_0) = s'(x_N)$ und $s''(x_0) = s''(x_N)$ nur falls $y_0 = y_N$
+ **not-a-knot:** $s'''$ ist stetig in $x_1$ und $x_{N-1}$

**Weitere Eigenschaften:** Für alle Methoden kann man die Gleichungssysteme in $O(N)$ lösen. Die natürliche Spline minimiert die Krümmung des Funktionsgraphen. Die Splines sind nicht so anfällig gegenüber dem Runge-Phänomen und Messfehler an einem Punkt wirken sich nicht (stark) auf die anderen Punkte aus.