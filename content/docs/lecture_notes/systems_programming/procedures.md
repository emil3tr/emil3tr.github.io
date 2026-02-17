---
title: Procedures
weight: 9
---

We already learned about the **stack**. The *%rsp* register holds the current stack pointer (which points to the lowest address on the stack (which is the address of the last element on the stack)).

> [!NOTE]
> Remember that the stack grows from high memory addresses to low memory addresses.

The `pushx src` instruction pushes a value onto the stack. It decrements *%rsp* by the size of type x and then writes *src* to the new address.

The `popx dest` instruction pops (retrieves) a value from the stack. It moves the value at *%rsp* into *dest* and then adds its size to *%rsp*.

## Procedures

A **procedure** is like a function (in higher level languages). The `call label` instruction pushes the current instruction pointer on the stack and then jumps to *label*. (This is like calling a function.) The `ret` instruction pops the return address from the stack and jumps to it.

Each procedure has its own **stack frame**. The *%rbp* register marks the end of the last stack frame (usually) and the *%rsp* the end of the stack. Between that is the current stack frame. We can store local variables, the old base pointer and more there. The **return address** is actually saved as the last value on the old stack frame (and is popped from there with the `ret` instruction).

## Calling Conventions

We call the procedure that calls another procedure with `call` the **caller** and the procedure being called the **callee**. 

We know that the return address is actually the last value stored on the callers stack frame. But how do we pass arguments to a function? We usually use the following integer registers (in this order) to pass **arguments**:

> %rdi, %rsi, %rdx, %rcx, %r8, %r9

For the **return value** we use:

> %rax, %rdx

Some registers are **callee saved**. This means that the caller can expect the value to not change when they call the procedure. Which means that the callee is forced to save them (on the stack) when they want to use the registers (and restore the value again before returning). The callee saved registers are:

> %rbx, %rbp, %r12, %r13, %r14, %r15

The other registers are **caller saved**, meaning the callee can modify them as they wish and the caller needs to take care of saving the values before calling the callee.

If we need more than six arguments, they need to be passed on the stack. We can also put values on the stack beyond the stack pointer and only increase it afterwards. This area is called the **red zone**. This simple function saves the return value of `get_one` on the stack before calling `get_two`:

```assembly
function:
    call    get_one
    movl    %eax, -4(%rsp)
    subq    $4, %rsp
    call    get_two
    addl    (%rsp), %eax
```

If we don't call another function, we can simply not decrease the stack pointer at all and work with our values in the red zone. Another optimization is **tail call** optimization, where we simply `jmp` to a function istead of calling it. This is possible if it is the last thing we want to do in a function anyways and don't need to return to it afterwards.

## Variadic Functions

C has **variadic functions**, which can accept a non-fixed amount of arguments. (For example, `printf` is such a function.) We can use them as follows:

```c
#include <stdarg.h>

int sum_up(int num, ...) {
    int res = 0;

    // list of arguments
    va_list args;

    // start it after num
    va_start(args, num);

    for(int i = 0; i < num; i++) {
        //get the next argument and cast to int
        res += va_arg(args, int);
    }

    // end it
    va_end(args);

    return res;
}

// returns 6
sum_up(3, 1, 2, 3);
```