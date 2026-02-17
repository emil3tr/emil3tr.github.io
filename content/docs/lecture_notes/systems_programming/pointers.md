---
title: Pointers
weight: 3
---

C is a stack-based language. The **stack** is a region of memory that grows from a high address to a low address. It contains the stack **frames** of functions (for example their local variables). The **heap** is another region in (virtual) memory. (It contains for example memory allocated with malloc.)

A **pointer** is a variable that stores a memory location. We can **dereference** it to get the value of that location. A special type of pointer is the **nullpointer**. It is guaranteed that a nullpointer never points to a valid region of memory (and is often returned when a function fails). The macro `NULL` is defined in `<stdlib.h>` and stands for a nullpointer.

```c
int x = 10;
int* p = &x; // get the address of x
*p = 5; // set x to 5 by dereferencing p

int ** pp = &p; // pointer to p
**pp = 7; // set x to 7
```

## Arithmetic

Note that pointers are **typed**, meaning it is specified to which type of value they point. A special typ is the `void*`. It does not specify a value.

> [!CAUTION] 
> We cannot do pointer arithmetic on void pointers!

If we add or subtract to a pointer, it will **not** simply add or subtract the number. It will add or subtract as many bytes as the value takes up times that number. So if there were multiple values with the same type in an array, adding one to a pointer would jump to the next one.

```c
int* ip = (int*) 4;
char* cp = (char*) 4;

ip += 1;
cp += 4;

// ip now holds the same value as cp
```

We can chech how much size a type takes up using `sizeof(type)`.

## Pointers and Arrays

Arrays are **not** pointers. But in expressions they are treated like pointers, except

+ in `sizeof()`
+ when we take the memory address of an array with `&` (returns address of the array, not the first element)
+ when it is a string literal initializer

An array as function parameter is a pointer and we can call it with a pointer or array.

Arrays cannot be reassigned. So `arr1 = arr2` is not valid. (But if the array is treated as a pointer as a function parameter, then it is okay. It is a pointer.)

```c
int arr[10]; // array
int (*pa)[] = &arr; // pointer to an array
int* pi = &arr[0]; // pointer to first element
```

Taking `arr[i]` is the same as doing `*(arr+1)` and the compiler will do the second one.

## Function Arguments

Function arguments in C are **pass-by-value**. This means the entire argument will get copied and modifying it inside the function will not change the original one. We can use pointers to **pass-by-reference**. 

> [!NOTE]
> Note that arrays as parameters are treated as pointers
> and are therefore pass-by-reference.

```c
void mod1(int x) {
    x = 1;
}

void mod2(int* x) {
    *x = 1;
}

void mod3(int x[]) {
    x[0] = 1;
}

int x = 0;
int a[1] = {0}
mod1(x); // x is still 0
mod2(x); // now it is 1
mod3(a); // a[0] is now 1
```

## Function Pointers

We can declare pointers to functions too.

```c
int add(int a, int b) {
    return a + b;
}

int (*func)(int, int) = add; // pointer to add
int x = 0;
int y = 1;
int z = func(x, y); // use add from pointer
```

## The Spiral Rule

There is a very helpful rule on how to parse C declarations (which can be quite confusing at times). It goes as follows:

1. Start at the variable name
2. Go right as long as you can, spiral to the left when you must (encounter a closing bracket)
3. End with the basic type

We can parse the symbols as:

| Symbol | Speech |
| :----: | :----- |
| [] | array |
| (...) | function passing ... returning |
| * | pointer to |

For example, `int*(*f[4])(int)` would be and array of pointers to a function taking an int and returning a pointer to an int. Note that `int f*()` is not valid, the asterix should be before the name as in `int (*f)()`.

See [cdecl.org](https://cdecl.org/) and this [post](https://c-faq.com/decl/spiral.anderson.html) to learn more and test some declarations.