# Calling Convention

## Introduction
In computer science, a calling convention is an implementation-level (low-level) scheme for how subroutines receive parameters from their caller and how they return a result. Differences in various implementations include where parameters, return values, return addresses and scope links are placed, and how the tasks of preparing for a function call and restoring the environment afterward are divided between the caller and the callee.

## Calling conventions may differ in:

 - Where parameters, return values and return addresses are placed (in registers, on the call stack, a mix of both, or in other memory structures)
 - The order in which actual arguments for formal parameters are passed (or the parts of a large or complex argument)
 - How a (possibly long or complex) return value is delivered from the callee back to the caller (on the stack, in a register, or within the heap)
 - How the task of setting up for and cleaning up after a function call is divided between the caller and the callee
 - Whether and how metadata describing the arguments is passed
 - Where the previous value of the frame pointer is stored, which is used to restore the frame pointer when the routine ends (in the stack frame, or in some register)
 - Where any static scope links for the routine's non-local data access are placed (typically at one or more positions in the stack frame, but sometimes in a general register, or, for some architectures, in special-purpose registers)
 - How local variables are allocated can sometimes also be part of the calling convention (when the caller allocates for the callee)

## In some cases, differences also include the following:
 - Conventions on which registers may be directly used by the callee, without being preserved (otherwise regarded as an ABI detail)
 - Which registers are considered to be volatile and, if volatile, need not be restored by the callee (often regarded as an ABI detail)

## MIPS
For MIPS, argument registers $a0-$a3 are used to pass the first four arguments to a subroutine. These arguments are not copied into the Argument Section by the calling subroutine, but the space is reserved. If this procedure is non-leaf, before calling other subroutine, it should save the argument registers in the reserved area of caller’s stack frame. Then it can reuse the argument registers to pass the arguments to the callee.

## ARM (A32)

The standard 32-bit ARM calling convention allocates the 15 general-purpose registers as:

 - r14 is the link register. (The BL instruction, used in a subroutine call, stores the return address in this register).
 - r13 is the stack pointer. (The Push/Pop instructions in "Thumb" operating mode use this register only).
 - r12 is the Intra-Procedure-call scratch register.
 - r4 to r11: used to hold local variables.
 - r0 to r3: used to hold argument values passed to a subroutine, and also hold results returned from a subroutine.

The 16th register, r15, is the program counter.

If the type of value returned is too large to fit in r0 to r3, or whose size cannot be determined statically at compile time, then the caller must allocate space for that value at run time, and pass a pointer to that space in r0.

Subroutines must preserve the contents of r4 to r11 and the stack pointer. (Perhaps by saving them to the stack in the function prologue, then using them as scratch space, then restoring them from the stack in the function epilogue). In particular, subroutines that call other subroutines *must* save the return address in the link register r14 to the stack before calling those other subroutines. However, such subroutines do not need to return that value to r14—they merely need to load that value into r15, the program counter, to return.

The ARM calling convention mandates using a full-descending stack.

This calling convention causes a "typical" ARM subroutine to

 - In the prologue, push r4 to r11 to the stack, and push the return address in r14, to the stack. (This can be done with a single STM instruction).
 - copy any passed arguments (in r0 to r3) to the local scratch registers (r4 to r11).
 - allocate other local variables to the remaining local scratch registers (r4 to r11).
 - do calculations and call other subroutines as necessary using BL, assuming r0 to r3, r12 and r14 will not be preserved.
 - put the result in r0
 - In the epilogue, pull r4 to r11 from the stack, and pull the return address to the program counter r15. (This can be done with a single LDM instruction).

## ARM (A64)
The 64-bit ARM (AArch64) calling convention allocates the 31 general-purpose registers as:

 - x30 is the link register (used to return from subroutines)
 - x29 is the frame register
 - x19 to x29 are callee-saved
 - x16 to x18 are the Intra-Procedure-call scratch register.
 - x9 to x15: used to hold local variables (caller saved)
 - x8: used to hold indirect return value address
 - x0 to x7: used to hold argument values passed to a subroutine, and also hold results returned from a subroutine.

## Caller and Callee

 - Caller-save registers are those that the subroutine may change, such as R0 through R3 in the ARM convention. They are caller-save because since a caller of a subroutine must save the registers’ values if it wants the values after the subroutine completes.

 - Callee-save registers are those that a subroutine must leave unchanged, like R4 through R12. Upon being called, the subroutine (the callee) must save the registers’ values if it wishes to use them.


### Why it needs to separate caller-save and callee-save registers
It is beneficial for a calling convention to designate both caller-save registers and callee-save registers. If the convention designated all registers as callee-save, then subroutines would not be able to use any registers at all without saving them onto the stack first — which would be a waste, since some of the saved registers would be transient values that the calling subroutine did not care about long-term. And if the convention designated all registers as caller-save, then programmers would be forced to save many registers before every call to a subroutine and to restore them afterwards, lengthening the amount of time to call a subroutine.

### Summary
 - Caller only needs to save the caller-save registers that is live across the subroutine call. If it will not be used after the calling, there is no need to save/restore them.
 - Callee only needs to save the callee-save registers that are used in the body of its subroutine.
 - If it is a leaf procedure, just use the caller-save registers as often as possible. If it is a non-leaf procedure.
 - If the variable is live across the call, callee-save register is preferred.  Otherwise, just use the caller-save registers.
 
## References:
 - [Calling convention Wiki](https://en.wikipedia.org/wiki/Calling_convention)
 - [Caller-save registers and callee-save registers](https://zhongshugu.wordpress.com/2011/02/23/caller-save-registers-and-callee-save-registers/s)
