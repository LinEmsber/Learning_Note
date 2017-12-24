# X86 Architecture and Assembly

## x86 Architecture

The x86 architecture has 8 General-Purpose Registers (GPR), 6 Segment Registers, 1 Flags Register and an Instruction Pointer. 64-bit x86 has additional registers.


## General-Purpose Registers

 - stack pointer:
  The SP holds the end of the memroy array.
  All the addresses greater than or equal to the stack pointer hold valid data.
  All the addresses less than the stack pointer hold invalid data.

 - frame pointer:
  The frame pointer points to the location where the stack pointer was.
  The idea is to keep the frame pointer fixed for the duration of foo()'s stack frame.
  We can use the frame pointer to compute the locations in memory for both arguments as well as local variables.


## Instruction Pointer
  - program counter:
  The PC holds the address of the next instruction to be executed.


## References:
 - [X86_Architecture wiki book](https://en.wikibooks.org/wiki/X86_Assembly/X86_Architecture)
