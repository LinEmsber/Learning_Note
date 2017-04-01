# Generic selsection

## Introduction
Provides a way to choose one of several expressions at compile time, based on a type of a controlling expression.

## source code
# Generic selsection

## Introduction
Provides a way to choose one of several expressions at compile time, based on a type of a controlling expression

## source code

- [source code](https://github.com/LinEmsber/C_Programming/blob/master/other/generic_selection_01.c)

        /* An example of _Generic */                                     

        #include <stdio.h>

        #define printf_dec_format(x) _Generic((x), \
                char: "%c",             \
                signed char: "%hhd",    \
                unsigned char: "%hhu",  \
                signed short: "%hd",    \
                unsigned short: "%hu",  \
                signed int: "%d",       \
                unsigned int: "%u",     \
                double: "%f",           \
                char *: "%s",           \
                void *: "%p")

        #define print(x) printf(printf_dec_format(x), x)
        #define printnl(x) printf(printf_dec_format(x), x), printf("\n");


        struct node{
                int val;
                struct node * next;
        };


        int main()
        {
                struct node * n;

        #if defined (__LINKED_LIST__)
                printnl(n);             // detect a new type
        #endif
                printnl('a');           // prints "97"
                printnl((char)'a');     // prints "a"
                printnl(123);           // prints "123"
                printnl(1.234);         // prints "1.234000"

                return 0;
        }


## References:
- [Generic selection](http://en.cppreference.com/w/c/language/generic)
- [C11 - Generic Selections](http://www.robertgamble.net/2012/01/c11-generic-selections.html)
