---
title: Integers
weight: 2
---

## Encoding

Integers are encoded using **two's complement**. If the most significant bit (MSB) is 0, then the number is positive (or zero). If the MSB is 1, then the number is negative. The MSB will add negatively to the sum of the number and all the others like normal. 

+ For a signed number with $l$ bits, the **largest** possible value is $2^{l-1}-1$ and the **smallest** $-2^{l-1}$. For an unsigned on it is $2^l - 1$ and $0$.
+ Arithmetic works the same for signed and unsigned numbers. 
+ For signed numbers we have -x = ~x + 1

On **little endian** machines, the least significant **byte** is stored at the lowest address. On **big endian** ones it is the highest address. x86 is little endian.

## Operations

We can do logical operations (bitwise) and shifts in C.

```c
~x // not
x ^ y // xor
x | y // or
x & y // and
x << y // left shift
x >> y // right shift
```

If x is unsigned, the right shift will be **logical** (simply shift). If it is signed it will be **arithmetic** (fill with MSB to maintain sign). If we shift too much or negative the result will be undefined.

> [!CAUTION]
> When we mix signed and unsigned values in an expression (for example in a comparison)
> then the signed value will get casted to unsigned!

For example `0 > -1` will evaluate to true, but `0u > -1` is false.

When we cast smaller values to larger ones, they will be **sign extended**. When we cast larger ones to smaller ones they will be **truncated**.

## Arithmetic

Signed and unsigned integers will **overflow** if the result of an operation does not fit inside it. They form an abelian group with addition.

If the result of a multiplication is too large for the interger, the higher order bits simply get ignored. Unsigned integers form a commutative ring with addition and multiplication (modular arithmetic). (Signed too, but they do **not** work like normal numbers. For example, two negative numbers multiplied could get positive.)

Instead of multiplying by $2^n$, we can also left shift by $n$. For unsigned (or positive) numbers we can simply arithmetic right shift instead of dividing. But if the number is signed and negative, we first need to add $2^n-1$ to it. This way we round to zero and not negative infinity.

```c
(x + (1<<n) - 1) >> n // divide negative
x >> n // divide positive
```