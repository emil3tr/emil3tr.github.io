---
title: x86 Architecture
weight: 7
---

x86 is an **instruction set architecture** (ISA). It specifies how to write assembly code for that architecture. The **microarchitecture** is the implementation of an ISA on the processor. It was initially developed by Intel. We use AT&T syntax here (opposed to Intel syntax).

x86 is a **complex** instruction set (CISC). (Compared to a reduced instruction set (RISC)). This means that it has lesser, but more complicated, instructions. It uses the stack to pass arguments and also has condition codes. A RISC instruction set would have shorter, simpler instructions (but need more of them to get a task done).

The **assembler** takes an assembly file (.s) and produces **object code** (.o). Object code is the byte-representation of the instructions. The **linker** then resolves references between the files (for example, when a function is declared in a file, but defined in another).

## Integer Registers

A **register** is the fastest type of memory the CPU can access. Some of the important registers for us are

+ **%rax** is used for return values
+ **%rdi**, **%rdi** are used to pass arguments
+ **%rbx**, **%rcx**, **rdx** are other general purpose registers
+ **%rsp** holds the **stack pointer**, which points to the lowest (last) address on the stack
+ **%rbp** is used as a **base pointer**, which holds the beginning of the current stack frame
+ **%r8** - **r15** are more general purpose registers

All of these are **general purpose** registers, meaning they can be used by the program to store values. A non-general purpose register would be **%rip**, which stores the instruction pointer.

All of those registers are 64bit. We can address only the lower 32 bits by replacing the "r" with an "e" in the name for the named registers. For example, **%eax** addresses the lower 32 bits of %rax. For %r8 - %r15 we need to put a "d" behind the name, like "%r8d".

For %rax - %rdx, we can access the lower / higher 16 bits of their lower 32 bit half by using %al / %ah respectively (and similar for the other letters).

## Instructions

Lots of instructions in x84 have **suffixes** to specify the size of their operands. For example, `movl` moves 32 bits, while `movq` moves 64 bits.

| Postfix | Bytes | Name |
| :---: | :---: | --- |
| b | 1 | Byte |
| w | 2 | Word |
| l | 4 | Longword |
| q | 8 | Quadword |

There are also different **operand types** for instructions:

+ We just saw **registers**.
+ **Immediates** are constant values and denoted with a dollar sign. We can give them in hex or decimal. For example, `$17` and `$0x11` both denote the numbe seventeen.
+ **Memory** like below.

But how do we address memory? A memory address is written as `D(Rb, Ri, S)` and calculated as

$$
    D + Rb + S*Ri
$$

Note that *Rb* and *Ri* are registers (but you cannot use *%rsp* as *Ri*). *D* can either be 1,2, or 4 and *S* can be 1, 2, 4, or 8.

Some important instructions (with the suffix denoted by x) are

+ `movx src, dest` moves value from *src* to *dest*. Cannot move between two memory locations!
+ `leax src, dest` loads the address of *src* into *dest*. For example, `leaq 4(%rax), %rdx` adds four to the address stored in *%rax* and loads it into *%rdx*. (This is used for fast multiply-and-add calculations.)
+ `incx dest` and `decx dest` increment or decrement the destination.
+ `addx src, dest` adds the source and destination and stores the value in *dest*.
+ `subx src, dest` subtracts the source from the destination and stores the value in *dest*.
+ `imulx`, `xorx`, ... exist for the other computations.
+ `shrx k, dest` is a logical right-shift, while `sarx k, dest` is an arithmetic one.

> [!NOTE]
> A 4 byte instruction like `addl %rdi, %eax` will set the upper 4 bytes of the destination register to zero.

## Example

The following code swaps the (integer) values at memory locations in *%rdi* and *%rsi*.

```assembly
movl    (%rdi), %eax
movl    (%rsi), %edx
movl    %eax, (%rsi)
movl    %edx, (%rdi)
```

The next code loads a value from *%rdi*, multiplies it by two and adds *%esi* and four to it and stores it back.

```assembly
movl    (%rdi), %eax
leal    4(%esi, %eax, 2), %eax
movl    %eax, (%rdi)
```

Note how we used *%eax* to store a 4-byte value and *%rdi* to store an 8-byte pointer.

## Condition Codes

**Condition codes** are set (as a side effect) by some instructions. Some important ones are

| Code | Meaning |
| :---: | --- |
| CF | Carry Flag |
| ZF | Zero Flag |
| SF | Sign Flag |
| OF | Overflow Flag |

For example, the `addx` instruction (producing the result res = src + dest) will set

+ CF if there is a carry out.
+ ZF if the result is zero.
+ SF if the result is negative.
+ OF if the signed addition overflows. (If src and dest have the same sign, but res has a different one.)

The `cmpx` instruction does the same thing, but does not actually store the result in the destination register. The `testx a, b` instruction will compute a & b (bitwise "and") and set ZF if the result is zero and SF when the result is negative. The `leax` instruction will not set condition codes.

We can use the `setx` instruction to set a byte based on a condition code.

## Jumps

We can use **jumps** to jump (continue execution at) a **label** in the code. The `jmp` instruction will always jump. The `jx` instruction will jump if the condition code x is fulfilled. Some common ones are

| Instruction | Condition |
| :---: | --- |
| je | ZF is set |
| jne | ZF not set |
| js | SF set |
| jg | ~(SF^OF) & ~ZF |
| jge | ~(SF^OF) |
| jle | (SF^OF) \| ZF |

The `jge` flag is, for example, used to check if *a* is greater or equal to *b* after doing `cmpx b, a`.

The following code stores the larger value of *%rdi* and *%rsi* in *%rax*.

```assembly
    movq    %rdi, %rax
    cmpq    %rsi, %rdi
    jge     skip
    movq    %rsi, %rax
skip:
```