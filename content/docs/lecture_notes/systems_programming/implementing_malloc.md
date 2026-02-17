---
title: Implementing Malloc
weight: 5
---

We want to implement a memory allocator now. It should take requests from `malloc`, `free` and friends. But it has to answer them immediately, maintain **alignment rules** and use the available space efficiently.

## Efficiency

One measure of efficiency is **throughput**. It is the number of requests we can handle in a certain time period. For example, if we need one minute (60 seconds) for 600 calls, then the throughput would be 10 calls per second.

**Peak memory utilization** measures how well we use the available space. We number the requests from $0$ to $n$ and define:

+ **Payload** of a call to `malloc` as the number of bytes it reserves.
+ Aggregate Payload $P_k$ is the sum of all currently allocated payloads.
+ Heap size $H_k$ as the current size of the heap.

Then the peak memory utilization after $k$ requests is

$$
    U_k = (\max_{i<k} P_i) / H_k
$$

which measures how much of the heap we used when we had the highest payload yet. (We assume that the heap is nondecreasing here.)

**Fragmentation** happens when we cannot use the available space efficiently. **Internal** fragmentation inside a block occurs for example because of overhead (for maintaining heap data structures) or alignment policies. **External** fragmentation happens when there are lots of small free spaces in the heap (but we cannot use them, because the block we want to allocate does not fit inside one of them).

## Implicit Lists

To keep track of our blocks, we add an additional word (unit of memory) at the start of each block (the **header**). It contains the length of the block and a **free-bit**, which signals if the block is free or allocated. (Since we mostly have to align the blocks anyways (to a power of two), we can use the lowest order bit as the free-bit).

Now, if we want to find a free block, we simply traverse the list. (Since we can easily find the next block using the size of the current one.) 

+ **First fit** chooses the first block that fits our requirements.
+ **Next fit** does the same, but starts where the previous search finished (and not at the beginning every time)
+ **Best fit** looks for the block that best fits the required bytes

Implicit lists are slow, since we need to traverse all blocks.

## Coalescing

But what if we free a memory block with another free block besides it? To reduce fragmentation, we should combine them. To make this possible with both the previous and next block, we introduct **boundary tags**. We copy the header of a block also to the end of the block. Now we can **coalesce** blocks by manipulating those headers.

Say, for example, we have a free block of size 4B and an allocated block of size 12B after it. If we free the allocated block, we can combine them to a free block of size 16B by changing the header at the start of the first block and copying it to the end of the second block.

It is a desing choice how, if and when we **split** free blocks (if we want to allocate a smaller block inside it) and if we coalesce immediately or **defer** it (for example when iterating over the list again).

Boundary tags, however, lead to more internal fragementation (since we need to store the headers).

## Explicit Free Lists

We now want to only store the free blocks in a list. To do this, we store not only the size in the header, but also a **next**- and **prev**-pointer. (But only in the header at the front.) This way, we can easily only loop over the free blocks.

To free a block, we simply remove it from the linked list (as we do as usual for linked lists). We can then insert it at the front of the list (LIFO - last in first out) or search the list and keep the addresses ordered (address-ordered policy). The latter takes more time, but leads to lower fragmentation.

When we coalesce with other free blocks, we need to remove those from the list too. Explicit free lists are **faster** than implicit ones, because they only loop over the free blocks.

## Segregated Free Lists

To make things even faster, we might want to keep several different lists for different sizes. We could, for example, have a seperate list for all power-of-two block sizes (and some special ones for smaller sizes).

Now, to allocate a block with size X, we only need to search the list that holds the smallest size larger than X. (If we don't find a block there, we need to search the other, larger, lists.) 

Segregated free lists are **faster** (only need logarithmic time for determining the size class) and have **better utilization** (approximate best-fit policy).

## Garbage Collection

Garbage collection aims to automatically free memory that is note used by the program anymore (which cannot be accessed again). There are a lot of ways to do this. One of them is **mark-and-sweep**:

1. View memory as a graph nodes (blocks) and edges (pointers).
2. Start with the **root** nodes, which are not on the heap. (For example, pointers on the stack, in registers, ...)
3. Traverse this graph and mark all blocks found as **reachable**.
4. Sweep and delete all non-marked blocks. Those blocks can never be accessed again (since there is no pointer to them), which means the program does not need them anymore.

This does of course not work in C, since we can do manual pointer arithmetic.

## Common Bugs

Some common bugs related to memory are

+ dereferencing nullpointers
+ not freeing blocks
+ freeing to often
+ accessing non-allocated memory
+ forgetting to initialize memory
+ and many more ...