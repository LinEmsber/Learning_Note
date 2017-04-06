# Makefile

## Basic usage

```Makefile
CC = gcc
CFLAGS = -g -Wall -O0

EXEC = filename
SRCS = main.c filename.c

$(EXEC): $(SRCS)
        $(CC) $(CFLAGS) $(SRCS) -o $(EXEC)
clean:
        rm $(EXEC)
```

## Variable Assignment


### dynamic assign

- Makefile:

```Makefile
VAR1 = first
VAR2 = $(VAR1)
$(warning $(VAR2))

VAR1 = second
$(warning $(VAR2))
```

```bash
> make
Makefile:3: first
Makefile:6: second
make: *** No targets.  Stop.
```

### immediate assign


- Makefile:

```Makefile
VAR1 = first
VAR2 := $(VAR1)
$(warning $(VAR2))

VAR1 = second
$(warning $(VAR2))
```

```bash
> make
Makefile:3: first
Makefile:6: first
make: *** No targets.  Stop.
```


### default

- Makefile:

```Makefile
VAR1 ?= default value
$(warning $(VAR1))
```

```bash
> make
Makefile:2: default value
make: *** No targets.  Stop.
```

```bash
> make VAR1=NEW
Makefile:2: NEW
make: *** No targets.  Stop.
```


### append

- Makefile:

```Makefile
VAR1 = first
$(warning $(VAR1))

VAR1 += second
$(warning $(VAR1))
```

```bash
> make
Makefile:2: first
Makefile:5: first second
make: *** No targets.  Stop.
```

## Variables Used by Implicit Rules

The recipes in built-in implicit rules make liberal use of certain predefined variables. You can alter the values of these variables in the makefile, with arguments to make, or in the environment to alter how the implicit rules work without redefining the rules themselves. You can cancel all variables used by implicit rules with the ‘-R’ or ‘--no-builtin-variables’ option.

For example, the recipe used to compile a C source file actually says ‘$(CC) -c $(CFLAGS) $(CPPFLAGS)’. The default values of the variables used are ‘cc’ and nothing, resulting in the command ‘cc -c’. By redefining ‘CC’ to ‘ncc’, you could cause ‘ncc’ to be used for all C compilations performed by the implicit rule. By redefining ‘CFLAGS’ to be ‘-g’, you could pass the ‘-g’ option to each compilation.

To see the complete list of predefined variables for your instance of GNU make you can run ‘make -p’ in a directory with no makefiles.

```bash
> make -p
```

## Recursive Use of make

Recursive use of make means using make as a command in a makefile. This technique is useful when you want separate makefiles for various subsystems that compose a larger system. For example, suppose you have a sub-directory subdir which has its own makefile, and you would like the containing directory’s makefile to run make on the sub-directory. You can do it by writing this:

```Makefile
subsystem:
        cd subdir && $(MAKE)
```

or, equivalently, this

```Makefile
subsystem:
        $(MAKE) -C subdir
```


## References:
- [gnu-make-autotools-cmake](http://www.slideshare.net/zzz00072/gnu-make-autotools-cmake)
- [gnu make manual](https://www.gnu.org/software/make/manual/make.html)
- [gnu make manual recursive](https://www.gnu.org/software/make/manual/html_node/Recursion.html)
