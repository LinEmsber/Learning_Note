# Compare and Swap

## Introduction

In computer science, compare-and-swap (CAS) is an atomic instruction used in multithreading to achieve synchronization. It compares the contents of a memory location with a given value and, only if they are the same, modifies the contents of that memory location to a new given value. This is done as a single atomic operation. The atomicity guarantees that the new value is calculated based on up-to-date information; if the value had been updated by another thread in the meantime, the write would fail.

The result of the operation must indicate whether it performed the substitution; this can be done either with a simple boolean response (this variant is often called compare-and-set).

## Overview

A compare-and-set operation is an atomic version of the following pseudocode, where * denotes access through a pointer

```c
function cas(p : pointer to int, old : int, new : int) returns bool {
    if *p ≠ old {
        return false
    }
    *p ← new
    return true
}
```

This operation is used to implement synchronization primitives like semaphores and mutexes,[1] as well as more sophisticated lock-free and wait-free algorithms.

Algorithms built around CAS typically read some key memory location and remember the old value. Based on that old value, they compute some new value. Then they try to swap in the new value using CAS, where the comparison checks for the location still being equal to the old value. If CAS indicates that the attempt has failed, it has to be repeated from the beginning: the location is re-read, a new value is re-computed and the CAS is tried again.


## References:
 - [Comapre-and-Swap wiki](https://en.wikipedia.org/wiki/Compare-and-swap#Implementation_in_C)
