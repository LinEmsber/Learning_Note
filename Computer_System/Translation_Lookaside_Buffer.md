# Translation Lookaside Buffer (TLB)

## Introduction

When a virtual memory address is referenced by a program, the search starts in the CPU. First, instruction caches are checked. If the required memory is not in these very fast caches, the system has to look up the memory’s physical address. At this point, TLB is checked for a quick reference to the location in physical memory.

A Translation lookaside buffer (TLB) is a memory cache that is used to reduce the time taken to access a user memory location. It is a part of the chip’s memory-management unit (MMU). The TLB stores the recent translations of virtual memory to physical memory and can be called an address-translation cache. A TLB may reside between the CPU and the CPU cache or between CPU cache and the main memory.

The majority of desktop, laptop, and server processors include one or more TLBs in the memory management hardware, and it is nearly always present in any processor that utilizes paged or segmented virtual memory.

If the requested address is not in the TLB, it is a miss, and the translation proceeds by looking up the page table in a process called a page walk. The page walk is time consuming when compared to the processor speed, as it involves reading the contents of multiple memory locations and using them to compute the physical address. After the physical address is determined by the page walk, the virtual address to physical address mapping is entered into the TLB.


A TLB has a fixed number of slots containing page table entries and segment table entries; page table entries map virtual addresses to physical addresses and intermediate table addresses, while segment table entries map virtual addresses to segment addresses, intermediate table addresses and page table addresses. The virtual memory is the memory space as seen from a process; this space is often split into pages of a fixed size (in paged memory), or less commonly into segments of variable sizes (in segmented memory).

The page table, generally stored in main memory, keeps track of where the virtual pages are stored in the physical memory. This method uses two memory accesses (one for the page table entry, one for the byte) to access a byte. First, the page table is looked up for the frame number. Second, the frame number with the page offset gives the actual address.

## References
 - [Translation_lookaside_buffer wiki](https://en.wikipedia.org/wiki/Translation_lookaside_buffer)
 - [Virtual Memory: 11 TLB Example Youtube](https://www.youtube.com/watch?v=95QpHJX55bM)
