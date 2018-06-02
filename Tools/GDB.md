# GDB learning note

## Enable TUI:

```shell=
> gdb -tui a.out
or
> gdbtui a.out
or
> gdb a.out
> tui enable
or
> ctrl + a
```

Use a TUI layout with only one window. The layout will either be ‘source’ or ‘assembly’. When the TUI mode is not active, it will switch to the TUI mode.
```shell=
C-x 1
```

Use a TUI layout with at least two windows. When the current layout already has two windows, the next layout with two windows is used. When a new layout is chosen, one window will always be common to the previous layout and the new one.
```shell=
C-x 2
```

## Disable TUI:

In the gdbtui.
```shell=
> tui disable
```

## assembly layout:

```shell=
> gdb a.out
> layout sam
```

## view both assembly and code:
```shell=
> gbdtui a.out
> layout split
```

## Changes which TUI window is currently active for scrolling.

Make the next window active for scrolling.
```shell=
> focus next
```

Make the previous window active for scrolling.
```shell=
> focus prev
```

Make the source window active for scrolling.
```shell=
> focus src
```

Make the assembly window active for scrolling.
```shell=
> focus asm
```

Make the register window active for scrolling.
```shell=
> focus regs
```

Make the command window active for scrolling.
```shell=
> focus cmd
```

Change the height of the window name by count lines.
```shell=
> winheight asm 20
```

## run GDB with arguments:

run a program with command line args
```shell=
> gdb --args executable_file arg1 arg2 arg3
```

set the args in the GDB
```shell=
> set args 1 22 33
```
show the args in the GDB
Argument list to give program being debugged when it is started is "1 22 33".
```shell=
> show args
```

## breakpoint:
A breakpoint makes your program stop whenever a certain point in the program is reached.

breakpoint at main function
```shell=
> break main
or
> b main
```

set up a break point inside C program
```shell=
> break [file_name]:line_number
or
> break [file_name]:function_name
```

Print a table of all breakpoints

```shell=
> info break
```

Remove breakpoint from line number or function

```shell=
> clear [line_number]
or
> clear [function_name]
```

## run:
```shell=
Use the run command to start your program under GDB.
> run
```

## start:

The "start" command does the equivalent of setting a temporary breakpoint at the beginning of the main procedure and then invoking the "run" command.
```shell=
> start
```

## GDB refresh screen:
``` shell=
Ctrl + L
```

## GDB step

Step to next line of code. Will step into a function.
You can step into a target function to print its local variables.
```shell=
> step
or
> s
```

## GDB next:
Execute next line of code. Will not enter functions.

```shell=
> next
or
> n
```

## GDB print expression:
This will print the value of the given expression. Most simple Ada expression formats are properly handled
by GDB, so the expression can contain function calls, variables, operators, and attribute references.

```shell=
> print variable
or
> print function
```

## GDB print variable in hexadecimal, decimal, or binary:

hexadecimal:
```shell=
> p/x variable
```

decimal:
```shell=
> p/d variable
```

binary:
```shell=
> p/t variable
```

## whatis
Print the data type of expression expr.
```shell=
> whatis expr
```

## ptype expr
Print a description of the type of expression expr.
ptype differs from, whatis, by printing a detailed description, instead of just the name of the type.

For example, for this variable declaration:

```c
struct complex {double real; double imag;} v;
```

the two commands give this output:

```shell=
(gdb) whatis v
type = struct complex
(gdb) ptype v
type = struct complex {
    double real;
    double imag;
}
```

## Exam memory:

exam program counter:
```shell=
> x/10i $pc
```

exam main function:
```shell=
> x/10i main
```

exam function:

```shell=
> x/10i [function]
```


## GDB backtrace:
Show trace of where you are currently. Which functions you are in. Prints stack backtrace.

```shell=
> breaktrace
or
> bt
```

## watch:
Set a watchpoint for an expression.
GDB will break when the expression expr is written into by the program and its value changes.
The simplest (and the most popular) use of this command is to watch the value of a single variable.

```shell=
> watch foo
```

Print a table of all watchpoints

```shell=
> info watch
```

## GDB frame:
Show current stack frame (function where you are stopped)

```shell=
> frame
```

```shell=
Move up a single frame (element in the call stack)
> up
```

```shell=
Move down a single frame
> down
```

## Show machine code

The command disassemble to display a range of addresses as machine instructions.

```shell=
disass <function>
```

## registers:

Print the names and values of all registers except floating-point and vector registers (in the selected stack frame).
```shell=
> info registers
```

Print the names and values of all registers, including floating-point and vector registers (in the selected stack frame).
```shell=
> info all-registers
```

you could print the program counter in hex with

```shell=
> p/x $pc
```

or print the instruction to be executed next with

```shell=
> x/i $pc
```

or add four to the stack pointer11 with

```shell=
> set $sp += 4
```

## eflags:
show the value of flags register now
```shell=
> info reg eflags
```

## for loop

```shell=
(gdb) set $pos=0

(gdb) print array[$pos++]
content of array[0]

(gdb)
content of array[1]

(gdb)
content of array[2]
```

## memory check:

In the shell= command line:
```shell=
> valgrind --tool=memcheck --leak-check=yes ./test
```

## References:
- [Debugging with GDB](https://sourceware.org/gdb/onlinedocs/gdb/index.html)
- [Become a GDB Power User](https://www.youtube.com/watch?v=713ay4bZUrw)
- [GDB view assembly](http://stackoverflow.com/questions/9970636/view-both-assembly-and-c-code)
- [GDB with arguemnts](http://stackoverflow.com/questions/6121094/how-do-i-run-a-program-with-commandline-args-using-gdb-within-a-shell=-script)
- [Setting Breakpoints](https://sourceware.org/gdb/onlinedocs/gdb/Set-Breaks.html#Set-Breaks)
- [GDB commands](http://www.yolinux.com/TUTORIALS/GDB-Commands.html)
- [valgrind usage](http://cs.ecs.baylor.edu/~donahoo/tools/valgrind/)
- [FLAGS register wiki](https://en.wikipedia.org/wiki/FLAGS_register)
- [gdb shows FLAGS register](https://blog.louie.lu/2016/09/04/gdb-%E9%A1%AF%E7%A4%BA-flags-register/)
- [gdb with assembler: Print status of carry flag](http://stackoverflow.com/questions/5210354/gdb-with-assembler-print-status-of-carry-flag)
- [TUI-specific Commands](https://sourceware.org/gdb/onlinedocs/gdb/TUI-Commands.html)
