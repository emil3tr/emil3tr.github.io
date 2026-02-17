---
title: Kapitel 2
weight: 2
---

Wir wollen Polynome aus einer Folge von Punkten $(x_i,y_i)$ interpolieren. Dazu suchen wir eine Basis $b(x)_j$ und Koeffizienten $\alpha_j$ und schreiben $\sum_1^n \alpha_j b(x)_j$.

Um ein Polynom schnell auszuwerten, verwenden wir das **Horner Schema**. Sei $p(x) = a_nx^n + \dots + a_0$:

> $R = a_n$  
> for $i$ from $n-1$ to $0$:  
> $R = Rx + a_i$

Es ist numerisch stabil und läuft in $O(n)$.

## Naive Methode

Ein Polynom von Grad $k$ ist durch $k+1$ Punkte (Stützstellen) eindeutig bestimmt. Also können wir für die Punkte ein lineares Gleichungssystem aufstellen und lösen. Diese Methode gibt aber die *Vandermode Matrix* (mit Einsen in der linken Spalte), welche schlecht *konditioniert* ist (die Lösung wird stark von numerischen Fehlern beeinflusst).

Da das Polynom durch die Punkte eindeutig ist, geben die anderen Methoden dasselbe Polynom (bei denselben Punkten).

Laufzeit: $O(n^2)$ für das lösen des Systems und $O(n)$ für die Auswertung.

## Newton Basis und dividierte Differenzen

Wir wollen nun leicht neue Punkte zum Polynom hinzufügen. Dazu fügen wir zu einem alten Interpolationspolynom einen neuen Term hinzu, welcher 0 ist bei den alten Punkten und bei dem neuen Punkt zum richtigen Wert ausgleicht.

Die **Newton-Basis** ist $N_n(x) = \prod_{i=0}^{n-1}(x-x_i)$. Also z.B.

$$
N_0(x) = 1, \; N_1(x) = (x-x_0), \; N_2(x) = (x-x_0)(x-x_1)
$$

Wir erhalten so n Basispolynome und die Koeffizienten könnten wir durch ein lineares Gleichungssystem bestimmen.

Besser geht es mit den **dividierten Differenzen**. Wir schreiben hier für die Interpolationspunkte $(x_i,y_i)$
$$
    y[a,a] = y_a \; \text{und} \; y[a,b] = \frac{y[a+1,b] - y[a,b-1]}{x_b - x_a}
$$

Aus den y mit "Länge" 1 können wir also die y mit "Länge" 2 berechnen und so weiter. Wir machen also DP. Die Koeffizienten sind $\beta_j = y[0,j]$ und ergeben das Polynom
$$
\sum_{j=0}^n \beta_j N_j(x) = y[0,0] + y[0,1](x-x_0) + \dots
$$

Wenn wir nur $O(n)$ Speicher verwenden wollen, können wir nur ein Array nehmen und im Durchgang i nur die Elemente ab i ausrechnen und die schon berechneten $\beta$ stehen lassen (siehe CE 2.1.d):

```
NewtonCoeffs(x, y):
    n = len(x)
    for j in range(1,n):
        y[j:n] = (y[j:n] - y[j-1:n-1]) /  (x[j:n] - x[:n-j])
    return y
```

Wir können das Polynom wieder mit einer Idee wie dem Horner-Schema auswerten.

Laufzeit: $O(n^2)$ für dividierte Differenzen und $O(n)$ für die Auswertung und $O(1)$ für das Hinzufügen neuer Punkte.

Der **Fehler** ist für n Stützstellen in [a,b] maximal
$$
    (b-a)^{n+1} \frac{\| f^{(n+1)} \|_\infty}{(n+1)!}
$$

Wobei $\|x\|_\infty$ die maximale Komponente von x ist. (In dem Fall der maximale Funktionswert.) Das folgt aus einer genaueren Schätzung in Theorem 2.2.12 im Skript.

## Lagrange und baryzentrische Interpolation

Wir definieren die **Lagrange-Polynome**
$$
    l_i(x) = \prod_{j=0, j \neq i}^n \frac{x-x_j}{x_i-x_j}
$$

Diese Polynome haben praktische Eigenschaften:

+ $l_i(x_j) = 0 \quad \forall x \neq j$
+ $l_i(x_i) = 1$
+ $deg(l_i) = n$
+ $\sum_{k=0}^n l_k^{(m)}(x) = 1 / 0$ für $m=0/m>0$ 

Wir können dann interpolieren mit
$$
    p(x) = \sum_{j=0}^n y_il_i(x)
$$

Um zur **baryzentrischen Formel** zu kommen, schreiben wir
$$
    \lambda_k = \prod_{j \neq k} \frac{1}{x_k - x_j}
$$

und dann
$$
    p(x) = \frac{\sum_{k=0}^n \frac{\lambda_k}{x-x_k} y_k}{\sum_{k=0}^n \frac{\lambda_k}{x-x_k}}
$$

Laufzeit: $O(n^2)$ für das Berechnen der $\lambda_k$ und dann $O(n)$ für eine Auswertung. Ein paar neue Punkte lassen sich in $O(n)$ hinzufügen.

Für den **Fehler** definieren wir die Lebesgue-Konstante $\Lambda_n$. Mit ihr schätzen wir die Auswirkung von Messfehlern ab. Bei der **Runge-Funktion** $f(x) = 1/(x^2+1)$ treten starke Fehler and den Rändern des Intervalls auf bei *äquidistanten Stützstellen*. Wir sollten diese also **niemals** für Polynome hohen Grades verwenden. Die Wahl der Stützstellen beeinflusst den Fehler.

## Chebyshev Interpolation

Wir wollen nun bessere Stützstellen als äquidistante Punkte finden. Wir definieren die Chebyshev-Polynome $T_n(x)$ und $U_n(x)$. Dann sind die n+1 **Chebyshev-Knoten** auf [a,b] für $k = 0, \dots, n$
$$
    x_k = a + \frac{1}{2} (b-a) (\cos \left( \frac{2k+1}{2(n+1)} \pi \right) + 1)
$$

Die **Chebyshev-Abszissen** sind
$$
    x_k = a + \frac{1}{2} (b-a) (\cos \left( \frac{k}{n} \pi \right) + 1)
$$

Wenn wir bei den Abszissen die Endpunkte auslassen wollen, gehen wir nur über $k = 1,\dots,n-1$

Die Wahl der Chebyshev-Knoten minimiert den **Fehler**. Die Chebyshev-Abszissen beinhalten die Chebyshev-Knoten (sehr vereinfacht) und werden deshalb in der Praxis verwendet.

Das **Chebyshev-Interpolationspolynom** ist dann
$$
    p(x) = c_0 + c_1T_1(x) + \dots + c_nT_n(x)
$$

Die $c_k$ können wir mit der Formel im Skript berechnen. Wenn wir die Koeffizienten haben, können wir $p(x)$ sehr stabil mit dem **Clenshaw-Algorithmus** berechnen. Wir setzen $d_{n+2} = d_{n+1} = 0$ und die Rekursion
$$
    d_k = c_k + (2x)d_{k+1} - d_{k+2}
$$

und erhalten $p(x) = d_0 - xd_1 = \frac{1}{2} (d_0 - d_2)$.