---
title: Introduction to C
weight: 1
---

C is a **compiled** language for **systems programming**. Source files ending with **.c** or **.h** are processed by the **preprocessor** (macros). It gets compiled into **assembly** (.s) and then into **object code** (.o). The **linker** links those files into an executable.

## Control Flow

C has most of the standard control flow statements we know. Notable is the **goto**-statement, which lets you jump to a **label**. It should generally not be used, except for cleanup code or jumping out of nested loops.

```c
for(int i = 0; i < 10; i++) {
    for(int j = 0; j < 10; j++) {
        if(i * j == 25) goto found;
    }
}

found:
    printf("Found 25!")
```

## Types

If we declare something in a block, its scope is the block. Otherwise it is in the scope of the **entire** program. In C, a statement is also an expression.

+ There are no **booleans**, but zero is false and anything else is true.
+ We can use **static** to restrict the scope of a declaration to this file (compilation unit). Inside a function, the value of a static variable will persist between calls.
+ The type **void** is either for a function that returns nothing or a void pointer (untyped pointer).
+ A **const** variable cannot be changed.
+ We can use **enums** to give integer values names.
+ Values are **signed** per default, use **unsigned** to change that.

For example:

```c
enum {SUMMER, FALL, WINTER, SPRING} // enum
static int x; // local scope

void f(int z) {
    static int count = 0; //will persist
    count += z;
}

```

Note that the actual size of the types is implementation defined. Here are some for x86-64:

```c
char c; // 1 byte
int x; // 4 bytes
long long lx; // 8 bytes
float f; // 4 bytes
double d; // 8 bytes

unsigned int s = 10U; // unsigned
int octal = 020; // will be 16
int hex = 0x1A; // will be 26
int binary = 0b11; // will be 3
```

## Operators

C has the usual operators we know.

+ We can **cast** values using the type in brackets before it. (This will usually not change the bits of the value.)
+ The boolean operators have **lazy evaluation** and do not evaluate their second operand if the result is already clear.

```c
int main(int argc, char** argv) {
    int x;
    int* p = &x;
    char c = *((char *) p); // casting
}
```

## Arrays and Strings

We can define **arrays** with square brackets. Multidimensional arrays are also possible. **Strings** are just an array of chars terminated with a **null byte** '\0'.

```c
int arr[3] = {1, 2, 3};
int brr[3][3]; // 2 dimensions
char str[4] = {'h', 'i', '!', '\0'}; // string 
```

Note that implicit strings like `"hello"` are **immutable**, meaning we cannot change them.

## Preprocessor

The preprocessor lets us define macros and include other files. 

+ If we **include** another file, it will be copied (literally) into the file.
+ We can define **macros** to replace certain expressions in our code
+ We can also check if a word is defined with **ifdef** und **ifndef**.
+ The preprocessor will automatically concatenate strings. We can transform c into a string (literally "c") with #c and can concatenate non-strings with ##.

```c
#define a b // will replace a with b

#ifndef x // define y if x is defined and else x
#define x
#else
#define y
#endif

// will be {"c", c_value}
#define dict(a) { #c, c ## _value}

// multiple lines
// do-while trick lets it be used in if-statement
#define sth(x) \
    do {x = 10; \
        x *= 2;} while(0)

// would not not compile without trick
// since semicolon is a statement
void f(x) {
    if(x > 3)
        sth(x);
    else
        x = 5;
}
```

## Modularity

A **declaration** says that something exists and a **definition** actually defines it.

```c
// declare foo
int foo(int a, int b);

// define foo
int foo(int a, int b) {
    return a + b;
}

// external
extern int counter;
```

We can use the **extern** keyword to specify that the definition is in another file. 

We include **header files** (foo.h), which only contain declarations and then define those a C file (foo.c) (which also includes foo.h). **Header guards** ensure that a header is only included once (and not recursively)

```c {filename="foo.h"}
#ifndef __FOO_H_
#define __FOO_H_

// macros, declarations, ...

#endif
```