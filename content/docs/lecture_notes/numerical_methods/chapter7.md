---
title: Kapitel 7
weight: 7
---

Die kosten für inverses sind n^3 und lu n^3 und dann lösen n^2 wir berechnen das inverse nie direkt sondern lösen n systeme für die Einheitsvektoren.

## Schur-Komplement

Wir wollen das System
$$
    \begin{bmatrix}
        A_{11} & A_{12} \\
        A_{21} & A_{22}
    \end{bmatrix}
    \begin{bmatrix}
        x_1 \\
        x_2
    \end{bmatrix}
    =
    \begin{bmatrix}
        b_1 \\
        b_2
    \end{bmatrix}
    
$$

lösen. Dafür eliminieren wir $A_{21}$ und erhalten das System
$$
    (A_{22} - A_{21}A_{11}^{-1}A_{12})x_2 = b_2 - A_{11}^{-1}A_{12}b_1
$$

und dann erhalten wir $x_1$ mit
$$
    x_1 = A_{11}^{-1}(b_1 - A_{12}x_2)
$$

Wir nennen die verwendete Matrix das **Schur-Komplement** von $A_{11}$
$$
    S = A_{22} - A_{21}A_{11}^{-1}A_{12}
$$

## Pfeilmatrix

Eine Pfeilmatrix (arrow matrix) besteht aus einer Diagonalmatrix $D \in \mathbb{R}^{n \times n}$ und einer weiteren Zeile und Spalte. Wir wollen das System
$$
    \begin{bmatrix}
        D & a_1 \\
        a_2^T & \alpha
    \end{bmatrix}
    \begin{bmatrix}
        x \\
        \chi
    \end{bmatrix}
    =
    \begin{bmatrix}
        b \\
        \beta
    \end{bmatrix}
$$

in nur $O(n)$ Operationen lösen. Dafür schreiben wir zuerst die ersten $n$ Zeilen um in
$$
    x_i = \frac{b_i - a_{1,i}\chi}{d_i}
$$

und erhalten für die letzte Zeile dann
$$
    \chi\alpha - \chi \sum_{i=1}^n \frac{a_{1,i} a_{2,i}}{d_i} = \beta - \sum_{i=1}^n \frac{a_{2,i} b_i}{d_i}
$$

Da $\chi$ hier die einzige Unbekannte ist, lässt sie sich leicht bestimmen und dann auch die $x_i$. 

## Sherman-Morrison-Woodbury-Formel

Wenn wir das Inverse $A^{-1}$ schon einmal berechnet haben und jetzt die Matrix $A$ nur mit einer Matrix von niedrigem Rang $UV^T$ addieren wollen, dann können wir das neue Inverse in $O(nk^2)$ anstatt $O(n^3)$ berechnen. Die Formel dazu ist
$$
    (A+UV^T)^{-1} = A^{-1} - A^{-1}U(I_k+V^TA^{-1}U)^{-1}V^TA^{-1}
$$

und für $k=1$ der Spezialfall
$$
    (A+uv^T)^{-1} = A^{-1} - \frac{A^{-1}uv^TA^{-1}}{1+v^TA^{-1}u}
$$

## Dünnbesetzte Matrizen

Normalerweise werden Matrizen in Python ganz und in **row-major-order** gespeichert.

Wir notieren die Anzahl der **nicht-null-Einträge** von $A$ mit $\text{nnz}(A)$. Falls gilt $\text{nnz}(A) \ll nm$ dann ist die Matrix **dünnbesetzt**. Wir wollen jetzt Speicherplatz sparen, indem wir nur die nicht-null-Einträge der Matrix speichern.

Das **COO** oder **Triplet**-Format speichert alle Einträge als $(i_k, j_k, a_k)$. Wir brauchen dafür $3*\text{nnz}(A)$ Speicher.

Das **CRS** oder **Compressed Sparse Row**-Format speichert die Einträge in drei Arrays **val**, **col** und **rowptr**. Alle Einträge der i-ten Reihe sind zwischen rowptr[i] und rowptr[i+1] (wobei rowptr[i+1] nicht inkludiert ist). Zum Beispiel hätte die folgende Matrix
$$
\begin{align*}
    \text{val} &= [1,2,3] \\
    \text{col} &= [0,0,1] \\
    \text{rowptr} &= [0,1,2,3]
\end{align*}
$$

die Form
$$
    \begin{bmatrix}
        1 & 0 & 0 \\
        2 & 0 & 0 \\
        0 & 3 & 0 \\
    \end{bmatrix}
$$

Wir brauchen dafür $2*\text{nnz}(A) + n + 1$ Speicher. Bemerke, dass rowprt Länge $n+1$ und nicht $n$ hat.

Das **CSC** oder **Compressed Sparse Column**-Format funktioniert ähnlich, aber mit Spalten statt Zeilen.

## Kleinste Quadrate

Wenn unsere Matrix $A$ mehr Zeilen als Spalten hat, dann hat das System $Ax = b$ oft keine Lösung. Stattdessen wollen wir den Vektor $x$ finden, der die quadrierte Norm des Fehlers minimiert
$$
    x = \arg \min_y \| Ay - b \|_2^2
$$

Wenn $A$ vollen Spaltenrang hat, dann ist die Lösung eindeutig. Es gibt immer eine Lösung, aber sie muss keine echte Lösung des ursprünglichen Systems sein.

Geometrisch ist $x$ die Projektion von $b$ auf den Spaltenraum von $A$. Wenn die Lösung optimal ist, muss der Rest $Ax-b$ also orthogonal auf den Spaltenraum stehen. Es muss also die **Normalengleichungen** erfüllen
$$
    A^T(Ax - b) = 0
$$

Da $A^TA$ und $A$ denselben Nullraum haben, gilt
$$
    A^TA \; \text{invertierbar} \iff A \; \text{hat vollen Spaltenrang}
$$

Falls $A^TA$ **invertierbar** ist, dann ist die Lösung **eindeutig** mit
$$
    x = (A^TA)^{-1}A^Tb
$$

Falls $A^TA$ **nicht** invertierbar ist, dann ist sie **nicht** eindeutig, da der Nullraum von $A$ nicht trivial ist und für $z \in \ker(A)$ gilt für eine Lösung $x^\star$
$$
    A(x^\star + z) = Ax^\star
$$

und somit haben alle solche Vektoren denselben Rest. Wir wollen nun die Lösung mit der **kleinsten Norm** finden. Dafür verwenden wir das Moore-Penrose-Pseudoinverse
$$
    x^\star = A^+b
$$

Das **Moore-Penrose-Pseudoinverse** ist
$$
    A^+ = V(V^TA^TAV)^{-1}V^TA^T
$$

und mit der SVD
$$
    A^+ = V_r \Sigma U_r^T
$$

Wir sollten die Normalengleichungen nicht selber berechen und das Inverse auch nicht berechnen, die schnellste Methode ist *numpy.lstsq*.

## Gram-Schmidt

```
m, n = a.shape
Q = np.zeros((m, n))
R = np.zeros((n, n))

for i in range(n):
    q_i = a[:, i].copy()
    for j in range(i):
        factor = np.dot(a[:,i], Q[:, j])
        R[j, i] = factor
        q_i -= factor * Q[:, j]
    norm = np.linalg.norm(q_i)
    if norm > 1e-10:
        q_i = q_i / norm
    Q[:, i] = q_i
    R[i, i] = norm

return Q, R
```

```
m, n = a.shape
q = a.copy()
r = np.zeros((n,n))

for i in range(n):
    norm = np.linalg.norm(q[:,i])
    if norm > 1e-10:
        q[:,i] /= norm
    for j in range(i+1, n):
        dotp = np.dot(q[:,i], q[:,j])
        r[i,j] = dotp
        q[:,j] -= dotp * q[:,i]
        r[i,i] = norm

return q, r
```