---
title: More x86
weight: 8
---

We already saw condition codes and jumps. Here are some examples of compiled C control flow.

```
int mult(int a, int b) {
    do {
        a *= a;
    } while(a < b);
    return a;
}

// compiles to

mult:
        movl    %edi, %eax
.L2:
        imull   %eax, %eax
        cmpl    %esi, %eax
        jl      .L2
        ret
```

We see that loops are implemented as backwards jumps. For a for-loop, we need to manually check the condition once before we start.

```
int mult(int a, int i) {
    for(; i < 10; i++) {
        a *= a;
    }
    return a;
}

// compiles to

mult:
        movl    %edi, %eax
        cmpl    $9, %esi
        jg      .L2
.L3:
        imull   %eax, %eax
        addl    $1, %esi
        cmpl    $10, %esi
        jne     .L3
.L2:
        ret
```

Note that all of these examples are **compiler-specific** and might change if new optimizations or hardware arise. You can experiment yourself at [compiler explorer](https://godbolt.org/).

## Conditional Moves

**Conditional move** instructions have the form `cmovCx`, where *C* is the condition. Let's say we only want to move *%eax* to *%edi* if it is greater than 10.

```assembly
compl $10, %eax
cmovgl %eax, %edi
```

## Switches

The compiler has multiple tricks to compile switches efficiently (instead of just doing lots of if-statements in a row). If the switch is **sparse** (the case-values are far apart), then it can create a binary-tree-like structure. (Basically doing binary search on the values.)

Otherwise is can use **jump tables**. A jump table stores the addresses of labels in a table in memory. The value (we switch on) is then used as an index into that table to pick the right address.

Say we have a switch like this:

```c
switch(val) {
    case 0:
        // do sth
        break;
    case 1:
        // do sth else
        break;
    case 2:
        // and so on...
}
```

This will compile to a jump table like this:

```assembly
# jump table

.L4:
        .quad   .L10
        .quad   .L9
        .quad   .L2
        .quad   .L8
        .quad   .L7
        .quad   .L6
        .quad   .L5
        .quad   .L2
        .quad   .L11

# code

jmp     *.L4(,%rsi,8)
```

Then the program first checks it the value is greater than the size of the jump table (and jumps to *default*). Otherwise it does an **indirect jump** into the table. (Where is calculates the address based on the offset from the lable. We scale *%rsi* by 8 here, since each jump table is a pointer of 8 bits.)