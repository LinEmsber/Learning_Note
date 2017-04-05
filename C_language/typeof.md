# Referring to a Type with typeof()

## Introduction

An operator provided by several programming languages to determine the data type of a variable.

## There are two ways of writing the argument to typeof:

- Here is an example with an expression:

```c
typeof (x[0](1))
```

This assumes that x is an array of pointers to functions; the type described is that of the values
of the functions.

- Here is an example with a typename as the argument:

```c
typeof (int *)
```

Here the type described is that of pointers to int.



The operand of typeof is evaluated for its side effects if and only if it is an expression of variably
modified type or the name of such a type.

typeof is often useful in conjunction with statement expressions.


## Example:
Here is how the two together can be used to define a safe "maximum" macro which operates on any arithmetic type
and evaluates each of its arguments exactly once:

```c
#define max(a,b)                \
        ({ typeof (a) _a = (a); \
        typeof (b) _b = (b);    \
        _a > _b ? _a : _b; })
```

There is just some kind of weird ({}) block with two statements in it. This in fact is a GNU extension to
C language called braced-group within expression. The compiler will evaluate the whole block and use the
value of the last.

The reason for using names that start with underscores for the local variables is to avoid conflicts
with variable names that occur within the expressions that are substituted for a and b.


## Some more examples of the use of typeof:

This declares y with the type of what x points to.
```c
typeof (*x) y;
```
This declares y as an array of such values.
```c
typeof (*x) y[4];
```

This declares y as an array of pointers to characters:
```c
typeof (typeof (char *)[4]) y;
```

It is equivalent to the following traditional C declaration:
```c
char *y[4];
```


To see the meaning of the declaration using typeof, and why it might be a useful way to write,
rewrite it with these macros:

```c
#define pointer(T)  typeof(T *)
#define array(T, N) typeof(T [N])
```

Now the declaration can be rewritten this way:

```c
array (pointer (char), 4) y;
```

Thus, array (pointer (char), 4) is the type of arrays of 4 pointers to char.


## references:
- [gcc/Typeof](https://gcc.gnu.org/onlinedocs/gcc/Typeof.html)
- [Typeof wiki](https://en.wikipedia.org/wiki/Typeof)
