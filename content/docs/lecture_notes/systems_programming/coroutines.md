---
title: Coroutines
weight: 12
---

**Coroutines** allow us to exit and later continue functions. This can be useful when we have a yielder and consumer function.
They are not threads, but can be used to implement something similar

To implement coroutines, we can use the `setjmp` and `longjmp` functions from `<setjmp.h>`. `setjmp`stores the state of a function in a buffer and returns 0. `longjmp` then continues execution at that state. (To us, it looks like the original call to `setjmp` just returned again, but now with the value given to it by `longjmp`.) A very minimal example could look like this.

```c
#include <setjmp.h>
#include <stdio.h>

static jmp_buf buffer;

void other() {
    longjmp(buffer, 1);
    printf("Hello from other");
    return;
}

int main() {
    if(!setjmp(buffer))
        other();
    else
        printf("Hello from me");
    return 0;
}
```

Here, the first call to `setjmp` in `main` will return 0 and `main` will call `other` normally. `other` will then `longjmp`, which means that `setjmp` will return again with 1. At this point `main` prints and then the program ends. The print statement it `other` is never executed.