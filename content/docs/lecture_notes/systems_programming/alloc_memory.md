---
title: Allocating Memory
weight: 4
---

We already know about the heap and stack. Now we want to allocate memory that can persist function calls and does not only live on the stack.

```c
int* ptr_to_pi() {
    int pi = 3;
    return &pi;
    //pi gets dealloced here!
}

int* ptr = ptr_to_pi();
int x = 3 + *ptr; // fails
```

## Malloc

There are four functions for memory management in `stdlib.h`. 

+ `malloc` lets us allocate some uninitialized memore
+ `calloc` like malloc but zeroes the memory
+ `realloc` changes the size of an allocated block of memory
+ `free` gives the memory back (meaning we cannot use it anymore)

Now we can initialize a value on the heap and return a pointer to it.

```c
int* to_four() {
    int* mem = (int*) malloc(sizeof(int));
    if(mem == NULL)
        exit(1); // malloc failed
    *mem = 4;
    return mem;
}

int* my_four = to_four();
int three = *my_four - 1;
free(my_four); // never forget to free!
```

It is very important that memory is freed again at some point. Otherwise we have a **memory leak** and the program keeps taking up more and more RAM over time.

Some languages have a **garbage collector** which does the freeing for us. This eliminates (almost all) memory leaks.

> [!CAUTION]
> Always free memory you allocate!

## Structs and Unions

If we want to store some data in a structured way, we can use **structs**. Note that structs are not like objects in Java. They are copied on assignment and pass-by-value.

```c
struct person {
    int age;
    int height;
};

struct person tom = {24, 180}
struct person* p = &tom;

tom.age = 25;
p->height = 182; // same as (*p).height
```

**Unions** only ever hold one of the values declared in it. They only have one memory space for all the values (which means they will override each other).

```c
union fandi {
    float f;
    int i;
};

union fandi u;
u.i = 10;
// u.i = 10 and u.f = 0.0
u.f = 0.5;
// u.i = 1056964608 and u.f = 0.5
```

## Typedef

Say we do not want to type "union" or "struct" every time or we want to simplify complicated type declarations. `typedef` allows us to rename types.

```c
typedef unsigned int natural_number;
typedef struct point {
    int x, y;
} point;

natural_number x = 10;
point my_point;
```

C has different namespaces for labels (goto), tags (for all structs, unions, enums), member names (one per struct, ...) and everything else.