# ABA problem

## Introductoin

In multithreaded computing, the ABA problem occurs during synchronization, when a location is readtwice, has the same value for both reads, and "value is the same" is used to indicate "nothing haschanged". However, another thread can execute between the two reads and change the value, do otherwork, then change the value back, thus fooling the first thread into thinking "nothing has changed"even though the second thread did work that violates the assumption.

## Instance
The ABA problem occurs when multiple threads (or processes) accessing shared memory interleave.Below is the sequence of events that will result in the ABA problem:

 1. Process P_1 reads value A from shared memory,
 2. P_2 is preempted, allowing process P_2 to run,
 3. P_2 modifies the shared memory value A to value B and back to A before preemption,
 4. begins execution again, sees that the shared memory value has not changed and continues.

Although P_1 can continue executing, it is possible that the behavior will not be correct due to the"hidden" modification in shared memory.

## Discussion
A common case of the ABA problem is encountered when implementing a lock-free data structure. If an
item is removed from the list, deleted, and then a new item is allocated and added to the list, it is
common for the allocated object to be at the same location as the deleted object due to optimization.
A pointer to the new item is thus sometimes equal to a pointer to the old item which is an ABA
problem.


## Reference
 - [ABA problem Wiki](https://en.wikipedia.org/wiki/ABA_problem)
