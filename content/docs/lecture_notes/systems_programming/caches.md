---
title: Caches
weight: 11
---

Memory is slow, but we want to have fast access to the data we work on. This is why modern systems use **caches**.
A cache is an intermediate storage for frequently-used values with a much **lower access latency** than DRAM.

Most systems have multiple layers of caches. The smallest (and fastest) **L1** cache typically only takes a few (like one or two) cycles to access.
(This is called the **hit time**)
The **L2** cahe can take about ten cycles to access.
This is very fast compared to DRAM, which can have latencies of **100+** cycles.

The **miss rate** of a cache tells us what percentage of memory accesses we cannot find in the cache.
The **miss penalty** is the additional time it takes to fetch such a value (from a slower cache of main memory).
Consider that a small change in the hit rate can have a big impact on the average latency.
If our hit rate sinks from 99% to 97%, the average access time doubles!

There are four types of cache misses:

+ **Cold** miss: first access to block
+ **Conflict** miss: placement policy evicted block
+ **Capacity** miss: cache too small to hold all blocks
+ **Coherency** miss: due to multiprocessor

Caches work because of **temporal** and **spatial** locality. Temporal locality means that we will probably access the same value multiple times (and therefore it is worth storing it in a cache).
Spacial locality means that, if we access a value at one address, we are likely to access values at addresses around it in the near future.

## Organization

A cache works in **blocks** (also called **lines**). A typical block size could be 32 bytes. All bytes in a block are loaded into the cache together.
Blocks are grouped into **sets**. A set could, for example, hold 4 blocks.
The amount of blocks a set holds is called **associativity**.

If we want to determine into which set a block goes, we firstly remove log(blocksize) bits from the address.
The next log(associativity) bits are calles **index bits**.
The value in the index bits determines the set.
Since we can store multiple blocks in a set, we keep the rest of the bits as a **tag** and store them with the cache line.
If we want to find a block in the cache, we compute the set and compare the tag with all blocks in that set.

Which blocks we evict when a set is full is another question.
There are multiple **replacement policies**. We could, for example, always remove the least recently used block (**LRU** policy).

Each set might store additional bits for the replacement policy.
Each block stores a **valid bit**, which indicates if the value in the block is up-to-date.

## Missing and Loading

Another distinction are write-through and write-back caches.
In a **write-through** cache, we write any value we would write to the cache simultaneously into memory. In a **write-back** cache we wait with the write-back until we evict the cache line.
This needs an additional **dirty bit** to track if the line is modified (and needs to be written back).

We can also choose to load a value into the cache when we write to it (and not only when we read it from main memory).
This would be called a **write-allocate** cache.

There are lots of other design choice regarding caches.

## Optimizing for Caches

If we want to use the caches in our CPU efficiently, we need to maximize the spatial and temporal locality.
This can be done by accessing matrices in the right order (row-major vs. column-major).

For matrix multiplication, we can use **blocking**. Instead of doing the mulitplication textbook-style, we divide the result matrix into smaller blocks and calculate the values in each block.
This changes the access pattern.
Instead of walking along entire rows and columns, we walk inside smaller blocks, improving locality.

Cache utilization is a huge performance factor and should be taken into account!