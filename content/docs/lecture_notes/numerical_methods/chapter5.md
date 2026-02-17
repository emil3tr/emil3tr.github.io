---
title: Kapitel 5
weight: 5
---

Wir wollen ein Integral
$$
    I = \int_a^b f(x)dx
$$

durch eine Summe
$$
    Q_n(f) = \sum_{i=1}^n w_i f(c_i)
$$

approximieren. Wir wollen also nur bestimmte Punkte $c_i$ auswerten und mit $w_i$ gewichten. Der **Fehler** ist
$$
    E(n) = | I - Q_n(f) |
$$

Eine Quadraturformel konvergiert **algebraisch** mit **Konvergenzordnung** $p$, falls gilt $E(n) = O(\frac{1}{n^p})$ und **exponentiell**, falls $E(n) = O(q^n)$.

Das bedeutet, wenn wir $n$ ver-x-fachen, reduziert sich der Fehler um den Faktor $x^p$. Wir verwenden wieder die **Lagrange-Polynome** und erinnern uns
$$
    l_i = \prod_{j \neq i} \frac{x - c_j}{c_i - c_j}, \quad l_i(c_j) = \delta_{ij}, \quad \sum l_i(x) = 1
$$

Wir erhalten dann
$$
    f(x) \approx \sum_{i=1}^n f(c_i) l_i(x)
$$

und somit
$$
    \int_a^b f(x)dx \approx \sum_{i=1}^n w_i f(c_i) \quad \text{mit} \quad w_i = \int_a^b l_i(x)
$$

Die **Ordnung** einer Quadraturformel ist $n+1$, falls sie alle Polynome bis Grad $n$ genau integriert (und erst bei Grad $n+1$) fehlschlägt.

Eine Quadraturformel auf $[-1,1]$ ist **symmetrisch**, falls
$$
    w_i = w_{n+1-i} \quad \text{und} \quad c_i = -c_{n+1-i}
$$

Die Ordnung einer symmetrischen Quadraturformel ist immer gerade.

## Regeln

Die **Mittelpunkt-Regel** lautet
$$
    Q(f,a,b) = (b-a) f(\frac{a+b}{2})
$$

und hat Ordnung 2. Die **Trapez-Regel**
$$
    Q(f,a,b) = \frac{b-a}{2} (f(a) + f(b))
$$

hat auch Ordnung 2. Die **Simpson-Regel**
$$
    Q(f,a,b) = \frac{b-a}{6} (f(a) + 4f(\frac{a+b}{2}) + f(b))
$$

hat Ordnung 4. Sie integriert also alle Polynome bis Grad 3 genau. Regeln mit höheren Ordnungen werden in der Praxis **nicht** auf äquidistanten Punkten verwendet.

## Summierte Regeln

Wir unterteilen das Intervall $[a,b]$ in $N$ Teile und wenden die Regeln auf jedes Teilintervall mit Länge $h$ an. Wir haben also die Punkte $a = x_0, \dots, x_n = b$ Dann summieren wir die Ergebnisse. Das ergibt für die **Mittelpunkt-Regel**
$$
    Q^n (f) = h \sum_{k=0}^{n-1} f(\frac{x_k + x_{k+1}}{2})
$$

Für die **Trapez-Regel**
$$
    Q^n(f) = \frac{h}{2} (f(a) + 2 \sum_{i=1}^{n-1} f(x_i) + f(b))
$$

Für die **Simpson-Regel**
$$
    Q^n(f) = \frac{h}{6} \sum_{i=0}^{n-1} (f(x_i) + 4 f(\frac{x_i + x_{i+1}}{2}) + f(x_{i+1}))
$$

## Monte-Carlo Quadratur

Wir approximieren
$$
    \int_a^b f(x)dx \approx \frac{1}{n} \sum_{i=1}^n f(x_i)
$$

wobei wir die $x_i$ zufällig gleichverteilt aus $[a,b]$ wählen. Monte-Carlo konvergiert zwar langsam, lässt sich aber einfach auch in höheren Dimensionen einsetzen.

## Fehler

Wir unterscheiden zwischen **lokalem** und **globalem** Fehler. Wenn wir summierte Regeln verwenden, hat jedes Intervall einen lokalen Fehler, der mit $h = (b-a)/N$ skaliert. Der globale gesamte Fehler ist dann aber $N * E_{local}$, was einen Faktor $h$ wegnimmt.

Die Mittelpunkt- und Trapez-Regeln haben lokalen Fehler $O(h^3)$ und global $O(h^2)$. Die Simpson-Regel lokal $O(h^5)$ und global $O(h^4)$. Beide konvergieren algebraisch. Sehr schnelle (exponentielle) Konvergenz tritt meistens nur auf, wenn die Funktion sehr glatt oder periodisch ist.

## Romberg (Richardson Extrapolation)

Bei algebraischer Konvergenz haben wir
$$
    E(h/2) \approx \frac{E(h)}{2^p}
$$

Wir können das verwenden, um den genauen Wert zu **extrapolieren**, ohne die Funktion noch einmal auswerten zu müssen. Wenn wir wissen, dass $Q_h \approx I + E(h)$ und $Q_{h/2} \approx I + E(h)/4$, dann können wir eine Abschätzung für den Fehler erhalten und diesen von $Q_{h/2}$ abziehen.

Die **Romberg Integration** verwendet dieselbe Idee wie die Konvergenzbeschleunigung nach Richardson aus Kapitel 1. Da die Trapezregel symmetrisch ist und nur gerade Potenzen von $h$ im Fehler hat, können wir das Schema hier anwenden.

Wir haben $R_{l,0} = T(2^l)$ wobei $T(n)$ die approximation mit $n$ Punkten ist. Wir starten bei $l=1$ und berechnen die Werte durch DP mit
$$
    R_{l,k} = \frac{4^kR_{l,k-1} - R_{l-1,k-1}}{4^k-1}
$$

Wir können dabei für $R_{l+1,0}$ die alten Punkte von $R_{l,0}$ wiederverwenden. Romberg funktioniert sehr gut, wenn die Funktion sehr glatt ist und keine Rundungsfehler $h$ dominieren.

## Gauss (5.5.1)

Wir wollen jetzt die Knoten $c_i$ so wählen, dass wir eine Quadraturformel mit maximaler Ordnung bekommen. Wir sind damit beschränkt, dass die **maximale Ordnung** einer Formel auf $s$ Knoten $2s$ sein kann.

Ähnlich wie bei Gram-Schmidt können wir für ein Intervall $[a,b]$ (und ein paar anderen speziellen Eigenschaften bezüglich einer Gewichtsfunktion $w(x)$) eine Folge von **Polynomen** $p_k(x)$ bauen, die jeweils auf die vorherigen **orthogonal** sind. Dabei gilt $\deg(p_k) = k$. Wenn wir $s$ Knoten brauchen, dann sind diese Knoten $c_1, \dots, c_s$ genau die Nullstellen von $p_s$.

Für zwei Wahlen von Intervallen und Eigenschaften bekommen wir die **Legendre-Polynome** und die **Hermite-Polynome**. Die Gauss-Legendre Quadratur geht auf $[-1,1]$ und für Gewicht $w(x)=1$.

Die Gauss-Knoten sind nicht verschachtelt und nicht äquidistant. (Wir können also bei höheren Ordnungen nicht die vorherigen Knoten wiederverwenden. Das ist ein Nachteil der Methode.)

Wir haben jetzt also die $s$ Knoten. Um die Gewichte für die Gauss-Legendre Quadratur auf $[-1,1]$ zu berechen, brauchen wir die Lagrange Polynome. Das Gewicht $b_i$ für $f(c_i)$ ist
$$
    b_i = \int_{-1}^1 l_i(x)dx
$$

Für ein anderes Referenzintervall geht das Integral über dieses Intevall. Die Gewichte sind immer positiv.

**Beispiel:** Wir wollen mit gleichmässigem Gewicht $w(x)=1$ auf $[-1,1]$ integrieren mit zwei Knoten. Wir bekommen das zweite Legendre Polynom $p_2(x) = (3x^2 - 1)/2$. Die Nullstellen (unsere Punkte) sind $\pm 1/\sqrt{3}$. Für die Gewichte bekommen wir 1 an beiden Punkten.

**Eigenschaften:** Die Ordnung auf $s$ Knoten ist $2s$. Wir können die Knoten in $O(s)$ berechnen. Auf sehr glatten Funktionen konvergiert Gauss

## Clenshaw-Curtis

Clenshaw-Curtis verwendet die Chebyshev-Abszissa als Knoten. Es verwendet die FFt und läuft für $2s$ Knoten in $O(s\log(s))$, konvergiert auch exponentiell, aber etwas langsamer als Gauss.

## Adaptive Quadratur

Wir wollen unsere Messpunkte so verwenden, dass wir einen kleinen Fehler erhalten. Die Idee ist, dass wir an stellen mit höherer Krümmung (Variation in der Funktion) mehr Punkte einsetzen und an "nicht so interessanten" Stellen weniger Punkte.

Um den lokalen Fehler in einem Intervall zu schätzen, werten wir es mit der Trapez- und (der genaueren) Simpson-Regel aus. Dann vergleichen wir beide Werte. Wenn der Unterschied gross ist, ist der Fehler wahrscheinlich hoch und wir fügen an dieser Stelle einen neuen Punkt ein (welchen wir bei der Simpson-Regel eh schon ausgewertet haben).

## Dünne Gitter

Wir können ein Integral in einem d-dimensionalen Raum naiv durch $n^d$ Auswertungen approximieren. Dies ist aber sehr teuer und die Konvergenz verschlechtert sich für höhere Dimensionen. Es tritt auch der **Fluch der Dimensionen** auf. In $d$ Dimensionen ist die Konvergenzrate nur noch $O(N^{-r/d})$ für eine Quadraturformel mit eindimensionaler Konvergenzrate $r$.

Dünne Gitter benötigen weniger Punkte als der naive Ansatz.

## Monte-Carlo

Wir approximieren
$$
    \int_0^1 f(x)dx \approx \frac{1}{N} \sum_{i=1}^N f(x_i)
$$

wobei die Punkte $x_i$ gleichverteilt zufällig aus $[0,1]$ gewählt sind. Für ein Intervall $[a,b]$ gibt das
$$
    \int_a^b f(x)dx \approx \frac{b-a}{N} \sum_{i=1}^N f(z_i)
$$

wobei $z_i = a + x_i(b-a)$. 

Je kleiner die **Varianz** der Methode, desto besser ist die Approximation. Ein **Vertrauensintervall** wird für *jede* Schätzung berechnet. Wenn wir ein Vertrauensintervall mit Wahrscheinlichkeit X Prozent haben, dann wird bei sehr vielen Schätzungen bei X Prozent davon der wahre Wert in dem Intervall für *diese* Schätzung liegen. 

Je kürzer das Vertrauensintervall, desto niedriger die Wahrscheinlichkeit, dass der Wert darin liegt. Um ein besseres zu erhalten, müssen wir die Varianz verkleinern oder $N$ vergrössern.

**Vorteile/Nachteile:** Die Monte-Carlo Quadratur ist nützlich, da sie auch für hohe Dimensionen oder unglatte Funktionen funktioniert und eine kurze Laufzeit hat. Sie konvergiert nur langsam mit $O(N^{-1/2})$, aber dafür unabhängig von der Dimension. Es gibt kein Runge-Phänomen. Sie ist probabilistisch. 

## Reduktion der Varianz

Bei **Control Variates** nehmen wir eine Funktion $\phi(t)$ und integrieren
$$
    \int_0^1 f(t)dt = \int_0^1 f(t) - \phi(t)dt + \int_0^1 \phi(t)dt
$$

Wir wählen eine leicht integrierbare Funktion mir Integral $I_\phi$ und haben
$$
    \int_0^1 \approx \frac{1}{N} \sum_{i=1}^N (f(x_i) - \phi(x_i)) + I_\phi 
$$

Wenn wir $\phi \approx f$ wählen, dann können wir den wichtigsten Teil von $f$ genau berechnen und den Rest durch Monte-Carlo. Zum Beispiel können wir die ersten (grössten) Terme einer Summe nehmen.

Beim **Importance Sampling** verwenden wir eine neue Dichtefunktion $g$ für die Zufallsvariablen. (Sie sind also nicht mehr gleich-, sondern nach $g$ verteilt.) Wir approximieren dann
$$
    \int_{[0,1]^d} f(x)dx = \frac{1}{N} \sum_{i=1}^N \frac{f(x_i)}{g(x_i)}
$$

Wir wollen $g$ so wählen, dass die Werte, wo $f$ gross ist, eine höhere Wahrscheinlichkeit bekommen.

Die **Quasi Monte-Carlo** Methoden verwenden anstatt "wirklichen" Zufallszahlen quasi-zufällige Sequenzen. In $d$ Dimensionen nimmt der Fehler dann mit $O(N^{-1}\log(N)^d)$ ab. Für kleine Dimensionen ist das sehr gut, für sehr grosse Dimensionen aber oft nicht mehr.

Für sehr komplizierte Funktionen brauchen wir oft einen Ansatz mit Fourier-Analyse.