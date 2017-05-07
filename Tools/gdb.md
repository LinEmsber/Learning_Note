# GDB learning note

## TUI Commands:

```bash
> gdb -tui a.out
or
> gdbtui a.out
or
> gdb a.out
> tui enable
or
> ctrl + a
```

## assembly layout:

```bash
> gdb a.out
> layout sam
```

## view both assembly and code:
```bash
> gbdtui a.out
> layout split
```

## Changes which TUI window is currently active for scrolling.

Make the next window active for scrolling.
```bash
focus next
```

Make the previous window active for scrolling.
```bash
focus prev
```

Make the source window active for scrolling.
```bash
focus src
```

Make the assembly window active for scrolling.
```bash
focus asm
```

Make the register window active for scrolling.
```bash
focus regs
```

Make the command window active for scrolling.
```bash
focus cmd
```

## run GDB with arguments:

run a program with command line args
```bash
> gdb --args executable_file arg1 arg2 arg3
```

set the args in the GDB
```bash
> set args 1 22 33
```
show the args in the GDB
Argument list to give program being debugged when it is started is "1 22 33".
```bash
> show args
```



## breakpoint:
A breakpoint makes your program stop whenever a certain point in the program is reached.

```bash
breakpoint at main function
> break main
or
> b main
```

set up a break point inside C program
```bash
break [file_name]:line_number
or
break [file_name]:func_name
```

## run:
```bash
Use the run command to start your program under GDB.
> run
```

## GDB refresh screen:
``` bash
Ctrl + L
```

## GDB step

Step to next line of code. Will step into a function.
You can step into a target function to print its local variables.
```bash
> step
or
> s
```

## GDB next:
Execute next line of code. Will not enter functions.

```bash
> next
or
> n
```

## GDB print expression:
This will print the value of the given expression. Most simple Ada expression formats are properly handled
by GDB, so the expression can contain function calls, variables, operators, and attribute references.

```bash
> print variable
or
> print function
```

## GDB print variable in hexadecimal, decimal, or binary:

hexadecimal:
```bash
> p/x variable
```

```bash
decimal:
> p/d variable
```

binary:
```bash
> p/t variable
```

## GDB backtrace:
Show trace of where you are currently. Which functions you are in. Prints stack backtrace.

```bash
> breaktrace
or
> bt
```

## GDB frame:
Show current stack frame (function where you are stopped)

```bash
> frame
```

```bash
Move up a single frame (element in the call stack)
> up
```

```bash
Move down a single frame
> down
```

## registers:
Print the names and values of all registers except floating-point and vector registers (in the selected stack frame).

```bash
> info registers
```

Print the names and values of all registers, including floating-point and vector registers (in the selected stack frame).
```bash
> info all-registers
```

you could print the program counter in hex with

```bash
> p/x $pc
```

or print the instruction to be executed next with

```bash
> x/i $pc
```

or add four to the stack pointer11 with

```bash
> set $sp += 4
```

## eflags:
show the value of flags register now
```bash
> info reg eflags
```

## for loop

```bash
(gdb) set $pos=0

(gdb) print array[$pos++]
content of array[0]

(gdb)
content of array[1]

(gdb)
content of array[2]
```

## memory check:
```bash
> valgrind --tool=memcheck --leak-check=yes ./test
```

## References:
- [Become a GDB Power User](https://www.youtube.com/watch?v=713ay4bZUrw)
- [GDB view assembly](http://stackoverflow.com/questions/9970636/view-both-assembly-and-c-code)
- [GDB with arguemnts](http://stackoverflow.com/questions/6121094/how-do-i-run-a-program-with-commandline-args-using-gdb-within-a-bash-script)
- [Setting Breakpoints](https://sourceware.org/gdb/onlinedocs/gdb/Set-Breaks.html#Set-Breaks)
- [GDB commands](http://www.yolinux.com/TUTORIALS/GDB-Commands.html)
- [valgrind usage](http://cs.ecs.baylor.edu/~donahoo/tools/valgrind/)
- [FLAGS register wiki](https://en.wikipedia.org/wiki/FLAGS_register)
- [gdb shows FLAGS register](https://blog.louie.lu/2016/09/04/gdb-%E9%A1%AF%E7%A4%BA-flags-register/)
- [gdb with assembler: Print status of carry flag](http://stackoverflow.com/questions/5210354/gdb-with-assembler-print-status-of-carry-flag)
- [TUI-specific Commands](https://sourceware.org/gdb/onlinedocs/gdb/TUI-Commands.html)
