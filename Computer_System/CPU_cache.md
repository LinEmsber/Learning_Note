# CPU cache

## Introduction

A CPU cache is a hardware cache used by the central processing unit (CPU) of a computer to reduce the average cost (time or energy) to access data from the main memory. The cache is a smaller, faster memory which stores copies of the data from frequently used main memory locations. Most CPUs have different independent caches, including instruction and data caches, where the data cache is usually organized as a hierarchy of more cache levels (L1, L2, etc.).


All modern (fast) CPUs (with few specialized exceptions) have multiple levels of CPU caches. The first CPUs that used a cache had only one level of cache, not split into L1d (for data) and L1i (for instructions). Almost all current CPUs with caches have a split L1 cache. They also have L2 caches and, for larger processors, L3 caches as well. The L2 cache is usually not split and is usually shared between cores. The L3 cache, and higher-level caches, are shared and not split.


Other types of caches exist, such as, the common to all modern CPUs, the translation lookaside buffer (TLB) that is part of the memory management unit (MMU) that most CPUs have.


When the processor needs to read from or write to a location in main memory, it first checks whether a copy of that data is in the cache. If so, the processor immediately reads from or writes to the cache, which is much faster than reading from or writing to main memory.

Most modern desktop and server CPUs have at least three independent caches: an instruction cache to speed up executable instruction fetch, a data cache to speed up data fetch and store, and a translation lookaside buffer (TLB) used to speed up virtual-to-physical address translation for both executable instructions and data.


The basic purpose of cache memory is to store program instructions that are frequently re-referenced by software during operation. Fast access to these instructions increases the overall speed of the software program.

As the microprocessor processes data, it looks first in the cache memory; if it finds the instructions there (from a previous reading of data), it does not have to do a more time-consuming reading of data from larger memory or other data storage devices.

Most programs use very few resources once they have been opened and operated for a time, mainly because frequently re-referenced instructions tend to be cached. This explains why measurements of system performance in computers with slower processors but larger caches tend to be faster than measurements of system performance in computers with faster processors but more limited cache space.


## Cache memory levels explained

Level 1 (L1) cache is extremely fast but relatively small, and is usually embedded in the processor chip (CPU).

Level 2 (L2) cache is often more capacious than L1; it may be located on the CPU or on a separate chip or coprocessor with a high-speed alternative system bus interconnecting the cache to the CPU, so as not to be slowed by traffic on the main system bus.

Level 3 (L3) cache is typically specialized memory that works to improve the performance of L1 and L2. It can be significantly slower than L1 or L2, but is usually double the speed of RAM. In the case of multicore processors, each core may have its own dedicated L1 and L2 cache, but share a common L3 cache. When an instruction is referenced in the L3 cache, it is typically elevated to a higher tier cache.


## Cache entries
Data is transferred between memory and cache in blocks of fixed size, called cache lines or cache blocks. When a cache line is copied from memory into the cache, a cache entry is created. The cache entry will include the copied data as well as the requested memory location (called a tag).

When the processor needs to read or write a location in main memory, it first checks for a corresponding entry in the cache. The cache checks for the contents of the requested memory location in any cache lines that might contain that address. If the processor finds that the memory location is in the cache, a cache hit has occurred. However, if the processor does not find the memory location in the cache, a cache miss has occurred. In the case of a cache hit, the processor immediately reads or writes the data in the cache line. For a cache miss, the cache allocates a new entry and copies data from main memory, then the request is fulfilled from the contents of the cache.


## Replacement policies
In order to make room for the new entry on a cache miss, the cache may have to evict one of the existing entries. The heuristic that it uses to choose the entry to evict is called the replacement policy. The fundamental problem with any replacement policy is that it must predict which existing cache entry is least likely to be used in the future. Predicting the future is difficult, so there is no perfect way to choose among the variety of replacement policies available. One popular replacement policy, least-recently used (LRU), replaces the least recently accessed entry.


## Write policies
If data is written to the cache, at some point it must also be written to main memory; the timing of this write is known as the write policy. In a write-through cache, every write to the cache causes a write to main memory. Alternatively, in a write-back or copy-back cache, writes are not immediately mirrored to the main memory, and the cache instead tracks which locations have been written over, marking them as dirty. The data in these locations is written back to the main memory only when that data is evicted from the cache. For this reason, a read miss in a write-back cache may sometimes require two memory accesses to service: one to first write the dirty location to main memory, and then another to read the new location from memory. Also, a write to a main memory location that is not yet mapped in a write-back cache may evict an already dirty location, thereby freeing that cache space for the new memory location.

There are intermediate policies as well. The cache may be write-through, but the writes may be held in a store data queue temporarily, usually so that multiple stores can be processed together (which can reduce bus turnarounds and improve bus utilization).

Cached data from the main memory may be changed by other entities (e.g. peripherals using direct memory access (DMA) or another core in a multi-core processor), in which case the copy in the cache may become out-of-date or stale. Alternatively, when a CPU in a multiprocessor system updates data in the cache, copies of data in caches associated with other CPUs become stale. Communication protocols between the cache managers that keep the data consistent are known as cache coherence protocols.


## Cache performance
Cache performance measurement has become important in the recent times where the speed gap between the memory performance and the processor performance is increasing exponentially. The cache was introduced to reduce this speed gap. Thus knowing how well the cache is able to bridge the gap in the speed of processor and memory becomes important, especially in high-performance systems. The cache hit rate and the cache miss rate play an important role in determining this performance. To improve the cache performance, reducing the miss rate becomes one of the necessary steps among other steps. Decreasing the access time to the cache also gives a boost to its performance.


## CPU stalls
The time taken to fetch one cache line from memory (read latency due to a cache miss) matters because the CPU will run out of things to do while waiting for the cache line. When a CPU reaches this state, it is called a stall. As CPUs become faster compared to main memory, stalls due to cache misses displace more potential computation; modern CPUs can execute hundreds of instructions in the time taken to fetch a single cache line from main memory.


## Cache Miss


### Cache Miss Types

| Miss Type | Description |
| -------- | -------- |
| Compulsory | The first reference to a block of memory, starting with an empty cache. |
| Capacity | The cache is not big enough to hold every block you want to use. |
| Conflict | Two blocks are mapped to the same location and there is not enough room to hold both. |

### Reducing Cache Misses

The following table summarizes the effects that increasing the given cache parameters has on each type of miss.
  - +: means there's an improvement in the cache miss rate
  - 0: means no change and
  - -: means the situation gets worse.

| Miss Type | Cache Size | Block Size | Associativity |
| -------- | -------- |
| Compulsory | 0/- | + | 0 |
| Capacity | + | 0 | 0 |
| Conflict | 0 | 0/+ | + |

  - As you increase the cache size, keeping the other two parameters constant, the number of potential compulsory misses will go up as there are more blocks to miss on in a cold cache. The limit case is a cache with a single block - you will only get one compulsory miss.
  - Increasing the block size means more adjacent words will be fetched on each miss, so references to these words will not cause compulsory misses - this exploits spatial locality.
  - Associativity only affects how cache blocks are arranged, not how they are fetched from main memory, so will not affect compulsory misses.
  - Capacity misses are not really affected by block size because the decrease in the number of blocks that can be held is offset by their increased size.
  - Associativity has no effect on capacity misses as the total number of blocks remains the same no matter what the associativity.
  - Conflict misses are not affected by cache size since conflict misses arise from blocks from main memory mapping to the same position in the cache, which is mostly independent of the cache size. Increasing the block size may increase the number of conflict misses. There is a greater chance to displace a useful block from the cache.






## References
 - Operating Sytem Concepts 9th
 - [wiki CPU_cache](https://en.wikipedia.org/wiki/CPU_cache)
 - [cache memory](http://searchstorage.techtarget.com/definition/cache-memory)
 - [Cache Miss Types](https://courses.cs.washington.edu/courses/cse378/02sp/sections/section9-2.html)
 - [The difference between conflict miss and capacity miss](https://stackoverflow.com/questions/33314115/whats-the-difference-between-conflict-miss-and-capacity-miss)
