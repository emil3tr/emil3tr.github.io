---
title: Kapitel 1
weight: 1
---

## Fehler

Für $x$ eine Approximation an $x_0$:

+ **absoluter Fehler** = $|x - x_0|$
+ **relativer Fehler** = $|x - x_0| / |x_0|$
+ **machine precision** = $10^{-16}$

Wir schreiben **Gleitkommazahlen** mit 0.mantissa.

**Auslöschung** passiert, wenn wir zwei grosse Zahlen mit (eigentlich) kleinem relativen Fehler *subtrahieren* und eine kleine Zahl mit grossem relativen Fehler erhalten. Zum Beispiel ist

$$
    \frac{f(x+h) - f(x)}{h}
$$

sehr fehlerbehaftet. Wir sollten also Subtraktion vermeiden.

### Differenzenquotient besser

Wie wollen die Ableitung besser approximieren. Die **naive Methode** (oben) hat Auslöschung.

Mit **rein imaginären Schritt** bekommen wir

$$
    f'(x_0) \approx Im(\frac{f(x_0 + ih)}{h})
$$

Mit **Konvergenzbeschleunigung**

$$
    f'(x_0) \approx \frac{f(x_0 + h) - f(x_0 - h)}{2h}
$$

Beide haben einen *Fehler* von $O(h^2)$ statt nur $O(h)$ und der imaginäre Schritt vermeidet Auslöschung. 

### Konvergenzbeschleunigung nach Richardson (1.1.20)

Wir wollen die Ableitung mit so schneller Konvergenz berechnen, dass wir schon vor der Auslöschung abbrechen können. Wir brauchen dafür einen Ausdruck der Form

$$
    f(x) - d(h) = c_1h^2 + c2_h^4 + \dots
$$

Das Schema funktioniert immer, wenn wir den Fehler (rechts) als Summe von geraden Potenzen schreiben können. Das **Richardson Schema** für den Differenzenquotient funktioniert so, wenn wir $f'$ bei $x$ approximieren wollen:

$$
    R_{l,0} = d(h/2^l) = \frac{f(x + h/2^l) - f(x - h/2^l)}{2(h/2^l)}
$$

Wir berechnen dann mit DP die anderen Terme (wobei der äussere loop über k geht und der innere über l):

$$
    R_{l,k} = \frac{4^kR_{l,k-1} - R_{l-1,k-1}}{4^k-1}
$$

Und der *Fehler* ist $f'(x) - R_{l,k} = C * (h/2^l)^{2k+2} $

## Matrizen

Der Rechenaufwand für wichtige Operationen:

+ $AB$ in $O(n^3)$
+ $a^Tb$ in $O(n)$
+ $ab^T$ in $O(n^2)$
+ $Dx$ in $O(n) = n*k$ falls D eine k-diagonal Matrix ist
+ $Mx = (uv^T)x = u(v^Tx)$ in $O(n)$ falls rank(M)=1 und wir sie somit als outer product schreiben können 