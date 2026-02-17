---
title: Numpy Funktionen
weight: 9
---

## Arrays

+ **np.zeros(n)**
+ **np.arange(start, end, step)** array starting with start not including end
+ **arr.reshape()** returns copy with new shape
+ **arr.shape** modifies shape 
+ **arr[start:end:step]** access array (end exclusive)
+ **arr[x:y:z, a:b:c, ...]** in multiple dimensions
+ **arr[:, np.newaxis]** adds a new axis (makes each element to an array with that element)
+ **arr < 5** gives boolean array
+ **arr[condition(arr)]** index with boolean array
+ **np.any(arr) / np.all(arr)** AND / OR of a boolean array
+ **np.sum(arr)** sum of array
+ **np.cumsum(arr)** cumulative sum
+ **arr[::-1]** reverse array
+ **np.ravel(arr)** flattens the array
+ **len(arr)** gives length
+ **np.zeros_like(arr)** gives zeros length of arr
+ **np.ones_like(arr)** gives ones length of 
+ **np.linspace(a, b, num=, endpoint=)** returns evenly spaced values including a excluding b
+ **np.hstack((a,b,...))** stacks arrays along the horizontal axis (puts them side by side)
+ **np.vstack((a,b,...))** stacks arrays along the vertical axis (puts them above each other)
+ **np.asarray(x)** gives the array of some data that can be converted to array
+ **np.array(x)** array from data
+ **np.empty(shape)** uninitialized array

## Python Lists

When we need a list to append elements to the easiest way is to make a python list **list = []**, then use **list.append(elem)** and then convert it to a numpy array with **np.array(list)**.

## Sorting

**np.sort(x)** sorts the array x. If we get arrays of x and y values then use **np.argsort(x)**, which returns the indices that would sort x.

```
x, y <- input
indices = np.argsort(x)
sorted_x = x[indices]
sorted_y = y[indices]
```

## Math

+ **1.j** is the imaginary unit
+ **np.real(x)** gives the real part
+ **np.imag(x)** gives the imaginary part
+ **np.power(base, exp)** take base to power exp

## Matrices

+ **A @ B** matrix product
+ **A.diagonal()** returns diagonal as array
+ **A.diagonal()[:,np.newaxis] * B** multiplies B with the diagonal matrix A fast
+ **np.diag(x)** makes a diagonal matrix with x
+ **np.diag(A, k=a)** returns the diagonal k above / below the diagonal
+ **np.triu(A)** upper triangle of A
+ **A.T** transpose of a
+ **n,p=A.shape** get dimensions of A
+ **np.dot(a, b)** dot product
+ **np.eye(n)** n x n identity matrix
+ **np.diag(A, k=)** extracts diagonal (or subdiagonal k)
+ **np.triu(A, k=)** extracts upper triangle (or +k above) and sets all other entries to 0
+ **np.triu(A, k=)** same for lower triange

## Sparse Matrices

+ **sc.sparse.coo_matrix(data, (i, j), shape=(n,m))** creates matrix from coo format
+ **A.tocsr()** and **A.tocoo()** and **A.tocsc()** transform to different formats

## Polynomials

+ **[an, a(n-1), ..., a0]** is the array representing the coefficents in python (high-first low-last)
+ **polyval(p, x)** evaluates p at x using horner scheme
+ **polyfit(x,y,d)** fits to the points (x,y) with degree d (might be poorly conditioned)
+ **sc.interpolate.CubicSpline(x,y)** cubic spline interpolator. Call again to evaluate at points. You can set bc_type=periodic/clamped/natural/not-a-knot

## Linalg

+ **np.linalg.solve(A,b)** solves linear equation system
+ **np.linalg.solve(A, B)** gives matrix X with columns that solve the multiple systems
+ **np.linalg.norm(A)** norm of a with options ord=2 for 2-norm, ord='fro' for frobenius and ord='inf' for max-norm
+ **np.linalg.lstsq(A,b)** does least squares and returns solution, residuals, rank of A, singular values of A
+ **np.linalg.pinv(A)** gives pseudoinverse of A
+ **U,S,V = np.linalg.svd(A)** computes svd, use option full_matrices = false to get the reduced svd

We can solve systems of equations with QR and LU like this:

```
# want to solve Ax = b

# with LU
    lu, piv = sc.linalg.lu_factor(A)
    x = sc.linalg.lu_solve((lu, piv), b)
# with QR
    q, r = np.linalg.qr(A)
    new_rhs = q.T @ b
    x = np.linalg.solve(r, new_rhs)
# also uses LU
    x = np.linalg.solve(A, b)
```

## Fourier

+ **np.fft.fft(y)** fast fourier transform 
+ **np.fft.ifft(c)** inverse fourier transform
+ **np.fft.fftfreq(n, d=x)** gives the frequencies in cycles per unit for n values with spacing d
+ **np.fft.fftshift(y)** gives fourier coefficients in other order

## Quadrature

+ **sc.integrate.quad(f, a, b)** quadrature
+ **sc.integrate.trapezoid(y, x)** trapezoid rule where x are even spaced
+ **sc.integrate.simpson(y, x)** simpson rule where x are even spaced
+ **r, w = sc.special.roots_legendre(n)** computes roots r and weights w for gauss legendre quadrature on n nodes in [-1,1]. **roots_sh_legendre** does the same for [0,1].

## Lambdas

```
# Make function that returns lambda
def get_lambda(a, b):
    return lambda x: a + b*x

# Define inline lambda
integrate(lambda x: x + 2, start, end)

# Use
integrate(g(2, 1), start, end)
```

## Roots

+ **sc.optimize.root(f, x0).x** find root with initial guess x0

## Other

+ **np.isclose(a,b, rtol=, atol=)** compares almost-equality of floats
+ **dtype=complex** sometimes needs to be added when making an array
+ **np.random.rand(shape)** gives shape random values in [0,1)
+ **np.random.default_rng(seed)** gives rng object which has method **.random()** to get random value in [0,1)
+ **str(x)** makes number to string
+ **convergence rate:** p =  -1 * np.polyfit(np.log(nodes), np.log(err), 1)[0]