---
title: Linking
weight: 10
---

Say you have a very cool function in `myfunc.c` and you want to use it in your projects. But that function is finished, so it would be a waste to recompile it every time we want to compile the project. Linking allows for **modularity**.
We can compile the source files (which might contain references to functions in other files)
independently and then **link** them together.

The linker gets (amongst others) `.o` files and produces an executable.

In C, the linker does two things. First, it **resolves symbols** and stores them (and information about them) in a **symbol table**.
It then matches **references** to the according **definitions**.
For example, say you define `void func(){...}` in one file and call it as `func();` in another one.
The linker will process the symbols and match the reference to the definition.

Secondly, the linker **relocates** code. This means it merges the source files into a single executable.
It also replaces the **relative** locations in the source files with **absolute ones**.
Say your procedure `func` is at address `0x123` in the original source file. Only after relocation do we know the final address in the executable, it might be `0x456`.
The linker then fills in that final address for the calls to `func`.

## Executables

There are different kinds of **object files**.

+ A **relocatable** object file (.o) stems from one source file (.c). The linker can combine those into an executable.
+ **Executables** contain the final code.
+ **Shared** object files (.so) can be linked **dynamically** at runtime (.dll on Windows).

The **ELF** (Executable and Linkable Format) is a standard for object files. It has the following sections:

+ **Header** has infos like word size, byte ordering, ...
+ **Segment header table**
+ **.text** contains the code
+ **.rodata** contains read-only data (like jump tables)
+ **.data** contains initialized global variables
+ **.bss** has uninitialized global variables
+ **.symbtab** has the symbol table
+ **.rel.text** contains relocation info for .text
+ **.re.data** has reloation info for .data
+ **.debug**
+ **section header table** size, location of each section

## Linker Symbols

There are three types of symbols:

+ **Global** symbols are defined by one module (file) and can be referenced by other ones. For example, non-static functions and non-static global variables.
+ **External** symbols are referenced by in one module, but defined in another.
+ **Local** symbols are only in one module. For example, static variables (or functions). Note that local variables in functions are **not** local linker symbols.

The following example illustrates the differences.

```c
int x = 7; // global
static int y; // local
extern int z; // external

void foo() { // global
    int c; // linker does not see
    c = 7;
    return c;
}

int main() { // global
    func(); // external
    return 0;
}
```

## Duplicate Definitions

There are two types of symbols:

+ **Strong** symbols are procedures (functions) and **initialized** global variables
+ **Weak** symbols are **uninitialized** global variables

The linker processes them according to these rules:

1. There **cannot** be multiple strong symbols
2. If some symbols are weak and one is string, we choose the **strong** one
3. If there are multiple weak symbols (and no strong one), we choose some (arbitrary) one.

For rule 3, there are two compiler flags to adjust this rule. `-fcommon` allows multiple weak symbols (and just chooses one of them). `-fno-common` is standard on newer compilers. It does not allow multiple weak symbols without a strong one and will throw an error.

Say we have two files and call `gcc fileA.c fileB.c`. Some examples are:

+ FileA: `int a;` and FileB: `extern int a;` is good.
+ FileA: `int a = 2;` and FileB: `int a;`is good.
+ FileA: `int a = 2;` and FileB: `int a = 3` is error.
+ FileA: `extern int a;` and FileB: `extern int a` is error.
+ FileA: `int a;` and FileB: `int a;` is error with `-fno-common`. With `-fcommon` it will choose one
+ FileA: `double a;` and FileB: `int a;` errors with `-fno-common`. With `-fcommon` writes in FileA might corrupt memory.

## Libraries

**Static Libraries** are `.a` archive files. They contain multiple object files bundled. The linker can scan those libraries for unresolved references during linking.
We can use an archiver to create such libraries.

The following code shows how to use `ar` to create a math library and use it in a program. Note that the command line order matters for linking. If we reversed the order, the linker would not be able to resolve the references to the library in `program.c`.

```
gcc -c math.c
ar rs mathlib.a math.o
gcc program.c mathlib.a
```

**Shared libraries** save space by being **dynamically linked** when loading the program. On Windows they are `.dll` files.