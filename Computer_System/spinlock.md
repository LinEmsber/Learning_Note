# spinlock

## Introduction
In software engineering, a spinlock is a lock which causes a thread trying to acquire it to simply wait in a loop ("spin") while repeatedly checking if the lock is available. Since the thread remains active but is not performing a useful task, the use of such a lock is a kind of busy waiting. Once acquired, spinlocks will usually be held until they are explicitly released.

Because they avoid overhead from operating system process rescheduling or context switching, spinlocks are efficient if threads are likely to be blocked for only short periods. However, spinlocks become wasteful if held for longer durations, as they may prevent other threads from running and require rescheduling. The longer a thread holds a lock, the greater the risk that the thread will be interrupted by the OS scheduler while holding the lock. If this happens, other threads will be left "spinning" (repeatedly trying to acquire the lock), while the thread holding the lock is not making progress towards releasing it. The result is an indefinite postponement until the thread holding the lock can finish and release it.

Implementing spin locks correctly offers challenges because programmers must take into account the possibility of simultaneous access to the lock, which could cause race conditions. Generally, such implementation is possible only with special assembly-language instructions, such as atomic test-and-set operations, and cannot be easily implemented in programming languages not supporting truly atomic operations.


The following example uses x86 assembly language to implement a spinlock.
It will work on any Intel 80386 compatible processor.


        ; Intel syntax

        locked:                      ; The lock variable. 1 = locked, 0 = unlocked.
             dd      0

        spin_lock:
             mov     eax, 1          ; Set the EAX register to 1.

             xchg    eax, [locked]   ; Atomically swap the EAX register with the lock variable.
                                     ; This will always store 1 to the lock, leaving the
                                     ; previous value in the EAX register.

             test    eax, eax        ; Test EAX with itself. Among other things, this will set the
                                     ; processor's Zero Flag if EAX is 0.
                                     ; If EAX is 0, then the lock was unlocked and we just locked it.
                                     ; Otherwise, EAX is 1 and we didn't acquire the lock.

             jnz     spin_lock       ; Jump back to the MOV instruction if the Zero Flag is not set.
                                     ; The lock was previously locked, and so we need to spin until it
                                     ; becomes unlocked.

             ret                     ; The lock has been acquired, return to the calling function.

        spin_unlock:
             mov     eax, 0          ; Set the EAX register to 0.

             xchg    eax, [locked]   ; Atomically swap the EAX register with the lock variable.

             ret                     ; The lock has been released.



## References
 - [Spinlock wiki](https://en.wikipedia.org/wiki/Spinlock)
 - [x86 xchg instruction](https://c9x.me/x86/html/file_module_x86_id_328.html)
