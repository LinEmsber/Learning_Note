# X86 Assembly: Procedure Example


## caller() and swap_add() in C

```c
int swap_add(int *xp, int *yp)
{
        int x = *xp;
        int y = *yp;
        *xp = y;
        *yp = x;
        return x + y;
}

int caller()
{
        int arg1 = 534;
        int arg2 = 1057;
        int sum = swap_add(&arg1, &arg2);
        int diff = arg1 - arg2;

        return sum * diff;
}
```

## caller() in assembly

```nasm
1       caller:
2               pushl %ebp              // Save old %ebp
3               movl %esp, %ebp         // Set %ebp as frame pointer
4               subl $24, %esp          // Allocate 24 bytes on stack
5               movl $534, -4(%ebp)     // Set arg1 to 534
6               movl $1057, -8(%ebp)    // Set arg2 to 1057
7               leal -8(%ebp), %eax     // Compute &arg2
8               movl %eax, 4(%esp)      // Store on stack
9               leal -4(%ebp), %eax     // Compute &arg1
10              movl %eax, (%esp)       // Store on stack
11              call swap_add           // Call the swap_add function
12              movl -4(%ebp), %edx
13              subl -8(%ebp), %edx
14              imull %edx, %eax
15              leave
16              ret
```

## swap_add() in assembly
```nasm
1       swap_add:                       
2               pushl %ebp              // Save old %ebp
3               movl %esp, %ebp         // Set %ebp as frame pointer
4               pushl %ebx              // Save %ebx
5               movl 8(%ebp), %edx      // Get xp
6               movl 12(%ebp), %ecx     // Get yp
7               movl (%edx), %ebx       // Get x
8               movl (%ecx), %eax       // Get y
9               movl %eax, (%edx)       // Store y at xp
10              movl %ebx, (%ecx)       // Store x at yp
11              addl %ebx, %eax         // Return value = x+y
12              popl %ebx               // Restore %ebx
13              popl %ebp               // Restore %ebp
14              ret                     // Return
```


## Parser the assembly code line by line

- caller() line 2: pushl %ebp
pushl %ebp is equivalent to
```c
subl $4,%esp
movl %ebp,(%esp)
```
 1. Decrement stack pointer, move %esp to the next frame.
 2. Store %ebp on stack, the %bsp can be used for frame variable or obtain the argument from pervious frame ( e.g. 8(%ebp), or 4(%ebp) ).

- caller() line 3: movl %esp, %ebp
 1. Set the %ebp as frame pointer, as the references for local variables or arguments from the previous frame.

- caller() line 4: subl $24, %esp
 1. Decreament stack pointer 24 bytes. It means allocate 24 bytes for this frame.

- caller() line 5: movl $534, -4(%ebp)
- caller() line 6: movl $1057, -8(%ebp)
 1. Set arg1 to 534 and set arg2 to 1057

- caller() line 7: leal -8(%ebp), %eax
- caller() line 8: movl %eax, 4(%esp)
- caller() line 9: leal -4(%ebp), %eax
- caller() line 10: movl %eax, (%esp)
 1. Compute &arg1 and &arg2
 2. Store -8(%ebp) on offset 4 of stack, as the argument for calling function in the future.
 3. Store -4(%ebp) on stack, as the argument for calling function in the future.

- caller() line 11: call swap_add Call
 1. the swap_add function

- swap_add() line 2: pushl %ebp
 1. Save old %ebp, stack downgrows for new frame

- swap_add() line 3: movl %esp, %ebp         
 1. Set %ebp as frame pointer, as the references for local variables

- swap_add() line 4: pushl %ebx              
 1. Save %ebx.
 2. Function swap_add requires register %ebx for temporary storage.
 3. Since this is a callee-save register, it pushes the old value onto the stack as part of the stack frame setup

- swap_add() line 5: movl 8(%ebp), %edx      
- swap_add() line 6: movl 12(%ebp), %ecx     
 1. Get xp and yp
 2. This code retrieves its arguments from the stack frame for caller.
 3. Since the frame pointer has shifted, the locations of these arguments has shifted from positions +4 and 0 relative to the old value of %esp to positions +12 and +8 relative to new value of %ebp.


- swap_add() line 7: movl (%edx), %ebx       
- swap_add() line 8: movl (%ecx), %eax       
 1. Get x and y

- swap_add() line 9: movl %eax, (%edx)       
- swap_add() line 10: movl %ebx, (%ecx)       
 1. Store y at xp, and store x at yp

- swap_add() line 11: addl %ebx, %eax  
 1. Return value = x+y
 2. The sum of variables x and y is stored in register %eax to be passed as the returned value.

- swap_add() line 12: popl %ebx               
 1. Restore %ebx

- swap_add() line 13: popl %ebp               
 1. Restore %ebp
 2. Reset the stack pointer so that it points to the stored return address.
 3. The ret instruction transfers control back to caller.

- swap_add() line 14: ret                     
 1. Return

## Summary
 - This code retrieves the values of arg1 and arg2 from the stack in order to compute diff.

 - The use of register %eax as the return value from swap_add.

 - The use of the leave instruction to reset both the stack (%esp) and the frame pointer (%ebp) prior to return.

 - We have seen in our code examples that the code generated by gcc sometimes uses a leave instruction to deallocate a stack frame, and sometimes it uses one or two popl instructions. Either approach is acceptable, and the guidelines from Intel and AMD as to which is preferable change over time.

 - We can see from this example that the compiler generates code to manage the stack structure according to a simple set of conventions.

 - Arguments are passed to a function on the stack, where they can be retrieved using positive offsets (+8, +12,...) relative to %ebp.

 - Space can be allocated on the stack either by using push instructions or by subtracting offsets from the stack pointer.

 - Before returning, a function must restore the stack to its original condition by restoring any callee-saved registers and %ebp, and by resetting %esp so that it points to the return address.

 - It is important for all procedures to follow a consistent set of conventions for setting up and restoring the stack in order for the program to execute properly.


## References
- Computer Systems: A Programmer's Perspective 2nd Section 3.7
