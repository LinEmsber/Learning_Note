# Options That Control Optimization

## Introduction

  * These options control various sorts of optimizations.

  * Without any optimization option, the compiler’s goal is to reduce the cost of compilation and to make debugging produce the expected results. Statements are independent: if you stop the program with a breakpoint between statements, you can then assign a new value to any variable or change the program counter to any other statement in the function and get exactly the results you expect from the source code.

  * These options control various sorts of optimizations.

  * Without any optimization option, the compiler’s goal is to reduce the cost of compilation and to make debugging produce the expected results. Statements are independent: if you stop the program with a breakpoint between statements, you can then assign a new value to any variable or change the program counter to any other statement in the function and get exactly the results you expect from the source code.

  * You can invoke GCC with -Q --help=optimizers to find out the exact set of optimizations that are enabled at each level.

## Options

### -fno-inline
  * Do not expand any functions inline apart from those marked with the always_inline attribute. This is the default when not optimizing.

  * Single functions can be exempted from inlining by marking them with the noinline attribute.

### -fgcse
  * Perform a global common subexpression elimination pass. This pass also performs global constant and copy propagation.

  * Note: When compiling a program using computed gotos, a GCC extension, you may get better run-time performance if you disable the global common subexpression elimination pass by adding -fno-gcse to the command line.

### -faggressive-loop-optimizations
  * This option tells the loop optimizer to use language constraints to derive bounds for the number of iterations of a loop. This assumes that loop code does not invoke undefined behavior by for example causing signed integer overflows or out-of-bound array accesses. The bounds for the number of iterations of a loop are used to guide loop unrolling and peeling and loop exit test optimizations. This option is enabled by default.

### -flto[=n]
  * This option runs the standard link-time optimizer. When invoked with source code, it generates GIMPLE (one of GCC’s internal representations) and writes it to special ELF sections in the object file. When the object files are linked together, all the function bodies are read from these ELF sections and instantiated as if they had been part of the same translation unit.

  * To use the link-time optimizer, -flto and optimization options should be specified at compile time and during the final link. It is recommended that you compile all the files participating in the same link with the same options and also specify those options at link time. For example:

```shell=
gcc -c -O2 -flto foo.c
gcc -c -O2 -flto bar.c
gcc -o myprog -flto -O2 foo.o bar.o
```

### -ffat-lto-objects
  * Fat LTO objects are object files that contain both the intermediate language and the object code. This makes them usable for both LTO linking and normal linking. This option is effective only when compiling with -flto and is ignored at link time.

### -funroll-loops
  * Unroll loops whose number of iterations can be determined at compile time or upon entry to the loop. It also turns on complete loop peeling (i.e. complete removal of loops with a small constant number of iterations). This option makes code larger, and may or may not make it run faster.

## Reference
  * [GCC Optimize-Options](https://gcc.gnu.org/onlinedocs/gcc/Optimize-Options.html)
