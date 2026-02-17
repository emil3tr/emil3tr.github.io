---
title: Kapitel 6
weight: 6
---

Eine **nichtlineare** Gleichung hat oft keine einfache algebraische Lösung. (Zum Beispiel $e^{-x} - x = 0 $.) Deshalb verwenden wir **iterative** Methoden, die mit einem Punkt $x_0$ anfangen und aus den vorherigen Punkten eine immer bessere Lösung berechnen.

Wir wollen allgemein $x^\star$ finden mit $F(x^\star) = 0$.

## Konvergenz

Eine iterative Methode **konvergiert** zur Lösung, falls gilt
$$
    \lim_{k \to \infty} x_{k} = x^\star
$$

und der **Fehler** in jeder Iteration ist
$$
    e_{k} = x_{k} - x^\star
$$

Falls die Methode konvergiert, können wir ihre **Konvergenzordnung** $p > 0$ feststellen falls
$$
    \lim_{k \to \infty} \frac{|e_{k+1}|}{|e_k|^p} = C
$$

mit $C$ konstant. Das bedeutet, in der Unendlichkeit verändert sich der Fehler von Schritt zu Schritt mit Potenz $p$. Wenn $p=1$ sagen wir **lineare Konvergenz**, für grössere $p$ **superlinear**. Falls $p=2$ konvergiert die Methode **quadratisch**. Quadratische Konvergenz bedeutet, dass sich die Anzahl der korrekten Ziffern in der Approximation bei jeder Iteration verdoppeln.

Mit Hilfe der Dreiecksungleichung können wir den Fehler **abschätzen**. Für lineare Konvergenz mit Rate $L$ gilt
$$
    \| x_{k+1} - x^\star \| \leq \frac{L}{1-L} \| x_{k+1} - x_k \|
$$

## Abbruch

Wenn wir zu lange iterieren, überwiegen Rundungsfehler und wir verschwenden zeit. Deshalb wollen wir unsere Methode irgendwann abbrechen. Zwei mögliche **Abbruchkriterien** sind der **absolute Fehler**
$$
    \| x_{k+1} - x_k \| < \epsilon_{abs}
$$

und der **relative Fehler**
$$
    \frac{\| x_{k+1} - x_k \|}{\| x_{k+1} \|} < \epsilon_{rel}
$$

Meistens werden beide kombiniert
$$
    \| x_{k+1} - x_k \| < \epsilon_{abs} + \epsilon_{rel} \| x_{k+1} \|
$$

Gute Wahlen sind $\epsilon_{abs}=10^{-8}$ und $\epsilon_{rel}=10^{-6}$.

Eine andere Möglichkeit ist da **Residuum** $r_k = F(x_k)$. Wir stoppen dann, falls
$$
    r_k < \epsilon_{res}
$$

Das heisst, wir sind nahe genug an null dran. **Aber** diese Methode kann auch schiefgehen, falls die Funktion eine beinahe-Nullstelle hat, bei der wir stoppen.

## Intervallhalbierungsverfahren

Falls die Funktion $f$ stetig ist und gilt $f(a) * f(b) < 0$, dann gibt es eine Nullstelle auf dem Intervall $[a,b]$. Wir können das verwenden, um ähnlich wie bei binary-search, in jeder Iteration das Intervall zu halbieren
$$
    m_k = \frac{a_k + b_k}{2}, \quad f_m = f(m_k)
$$

Falls $f(a_k) * f_m < 0$ liegt die Nullstelle im linken Teil und wir setzen $a_{k+1} = a_k, \space b_{k+1} = m_k$. Ansonsten liegt es im rechten Teil und wir arbeiten auf $[m_k, b_k]$ weiter.

Da wir das Intervall in jedem Schritt halbieren, konvergiert die Methode linear mit
$$
    |m_k - x^\star| \leq \frac{b_0 - a_0}{2^{k+1}}
$$

und wir können absolute Genauigkeit $\epsilon_{abs}$ mit der Wahl $k \geq \log_2((b_0-a_0)/\epsilon_{abs})$ erreichen.

Die Methode **konvergiert immer** (für geeignete Startpunkte), aber **langsamer** als andere Methoden (wie Newton).

## Fixpunkt-Methode

Ein **Fixpunkt** ist ein Vektor $x^\star$ sodass $\Phi(x^\star) = x^\star$. Anstatt die Lösung $F(x)=0$ zu finden, können wir das Problem zur Gleichung $\Phi(x) = x$ umschreiben und den Fixpunkt suchen. Falls
$$
    \Phi(x) = x \iff F(x) = 0
$$

ist die Fixpunktiteration **konsistent**. Unsere Iteration wird dann
$$
    x_{k+1} = \Phi(x_k)
$$

Diese Methode konvergiert **lokal** und **mindestens linear**, falls 
$$
    |\Phi'(x^\star)| < 1
$$

Falls $|\Phi'(x^\star)| = 1$ kann beides passieren und ansonsten divergiert sie. Wir können die Konvergenz am Fixpunkt also überprüfen, indem wir die erste Ableitung berechnen. In höheren Dimensionen ist die Bedingung $\| D\Phi(x^\star)\| < 1$ (Norm der Jacobi-Matrix).

Wenn $\Phi$ (m+1)-mal differenzierbar ist und gilt
$$
    \Phi^{(l)} = 0 \quad \text{for} \space l = 1, \dots, m
$$

dann konvergiert sie lokal mit Ordnung mindestens $m+1$.

## Newton-Verfahren

Wir verwenden die erste Taylor-Approximation und bekommen das **Newton-Update**
$$
    x_{k+1} = x_k - \frac{f(x_k)}{f'(x_k)}
$$

was wie eine Fixpunkt-Methode mit $\Phi(x) = x - f(x)/f'(x)$ ist. Wenn wir die Ableitung ausrechnen, bekommen wir
$$
    \Phi'(x) = \frac{f(x)f''(x)}{f'(x)^2}
$$

und somit ist $\Phi(x^\star) = 0$. Das bedeutet, Newton konvergiert lokal *quadratisch*. Newton kann aber auch fehlschlagen, falls $f'(x^\star) \approx 0$, der Startpunkt sehr weit weg ist, oder die Funktion nicht differenzierbar ist. Die Newton-Methode funktioniert also **nicht global** und kann für schlechte Startpunkte irgendwo anders hinspringen.

## Sekantenverfahren

Wir wollen (oder können) die Ableitung der Funktion für das Newton-Verfahren nicht auswerten. Stattdessen approximieren wir sie durch die **Sekante**
$$
    f'(x_k) \approx \frac{f(x_k) - f(x_{k-1})}{x_k - x_{k-1}}
$$

und erhalten ein neues Verfahren
$$
    x_{k+1} = x_k - f(x_k) \frac{x_k - x_{k-1}}{f(x_k) - f(x_k{-1})}
$$

Die Konvergenzordnung des Sekantenverfahrens ist $p \approx 1.6$ (der goldene Schnitt), also konvergiert sie *superlinear*.

## Mehrdimensionales Newton-Verfahren

Wir müssen jetzt einen **Vektor** $x^\star$ finden, sodass $F(x^\star)=0$. Wieder durch Taylor erhalten wir die Iteration
$$
    x_{k+1} = x_k - J_F(x_k)^{-1} F(x_k)
$$

wobei $J_F(x)$ die Jacobi-Matrix von $F$ ist. Wir sollten aber das inverse einer Matrix **niemals** manuell ausrechnen. Stattdessen lösen wir
$$
    J_F(x_k) s_k = -F(x_k)
$$

und setzen
$$
    x_{k+1} = x_k + s_k
$$

Wir müssen also in jeder Iteration ein $n \times n$ System lösen, was sehr teuer werden kann.

Die Methode konvergiert lokal *quadratisch*, falls sie zweimal stetig differenzierbar bei $x^\star$ und die Jacobi-Matrix invertierbar ist.

## Gedämpftes Newton-Verfahren

Das Newton-Verfahren springt manchmal zu weit und divergiert für Ausgangspunkte nicht nahe an der Lösung. Deshalb dämpfen wir jeden Schritt mit einem **Dämpfungsfaktor** $\alpha_k$
$$
    x_{k+1} = x_k - \alpha_k \frac{f(x_k)}{f'(x_k)}
$$

Je kleiner der Dämpfungsfaktor, desto langsamer nähert sich das Verfahren der Lösung an, aber desto höher ist die Wahrscheinlichkeit, dass es auch konvergiert.

Eine gute Methode, um $\alpha_k$ zu wählen, ist zuerst mit $\alpha_k = 1$ zu starten. Solange das Verfahren $|f(x_{k+1})|$ erhöht, verkleinern wir den Faktor (zum Beispiel halbieren), solange bis wir uns der Lösung annähern.

## Broyden-Quasi-Newton

Anstatt die (teure und möglicherweise schlecht konditionierte) Jacobi-Matrix in jeder Iteration zu berechnen, wollen wir sie approximieren. Die Jacobi-Matrix $J_k$ für jeden Schritt sollte
$$
    J_k(x_k - x_{k-1}) = F(x_k) - F(x_{k-1})
$$

erfüllen. Wir suchen uns die Matrix (aus allen möglichen) aus, welche am nähesten an der alten ist, also $\| J_k - J_{k-1} \|_F$ minimiert. Übrigens ist die **Frobeniusnorm**
$$
    \boxed{\|A\|_F = \sqrt{\sum_i \sum_j |a_{ij}|^2}}
$$

Unsere neue Jacobi-Matrix ist in jedem Schritt also
$$
    J_k = J_{k-1} + \frac{(y_k - J_{k-1}s_k)s_k^T}{\|s_k\|_2^2}
$$

mit $s_k = x_k - x_{k-1}$ und $y_k = F(x_k) - F(x_{k-1})$.