===============================================================================
                        Makefile
===============================================================================

-------------------------------------------------------------------------------
Basic usage
-------------------------------------------------------------------------------

CC = gcc
CFLAGS = -g -Wall -O0

EXEC = filename
SRCS = main.c filename.c

$(EXEC): $(SRCS)
        $(CC) $(CFLAGS) $(SRCS) -o $(EXEC)
clean:
        rm $(EXEC)


-------------------------------------------------------------------------------
Variable Assignment
-------------------------------------------------------------------------------

1. dynamic assign

Makefile:
        VAR1 = first
        VAR2 = $(VAR1)
        $(warning $(VAR2))

        VAR1 = second
        $(warning $(VAR2))

> make
        Makefile:3: first
        Makefile:6: second
        make: *** No targets.  Stop.


2. immediate assign

Makefile:
        VAR1 = first
        VAR2 := $(VAR1)
        $(warning $(VAR2))

        VAR1 = second
        $(warning $(VAR2))

> make
        Makefile:3: first
        Makefile:6: first
        make: *** No targets.  Stop.


3. default

Makefile:
        VAR1 ?= default value
        $(warning $(VAR1))

> make
        Makefile:2: default value
        make: *** No targets.  Stop.

> make VAR1=NEW
        Makefile:2: NEW
        make: *** No targets.  Stop.



4. append

Makefile:
        VAR1 = first
        $(warning $(VAR1))

        VAR1 += second
        $(warning $(VAR1))

> make
        Makefile:2: first
        Makefile:5: first second
        make: *** No targets.  Stop.


References:
http://www.slideshare.net/zzz00072/gnu-make-autotools-cmake
https://www.gnu.org/software/make/manual/make.html
