# __builtin_constant_p

## Description
You can use the built-in function __builtin_constant_p to determine if a value is known to be constant at compile time and hence that GCC can perform constant-folding on expressions involving that value.

The argument of the function is the value to test. The function returns the integer 1 if the argument is known to be a compile-time constant and 0 if it is not known to be a compile-time constant. A return of 0 does not indicate that the value is not a constant, but merely that GCC cannot prove it is a constant with the specified value of the -O option.

You typically use this function in an embedded application where memory is a critical resource. If you have some complex calculation, you may want it to be folded if it involves constants, but need to call a function if it does not. For example:

```shell=
#define Scale_Value(X)  ( __builtin_constant_p (X) ? ( (X) * SCALE + OFFSET ) : Scale (X) )
```

You may also use __builtin_constant_p in initializers for static data. For instance, you can write

```shell=
static const int table[] = {
        __builtin_constant_p (EXPRESSION) ? (EXPRESSION) : -1,
};
```

## References
GCC manual 4.8.5 Ch.6.55 Other Built-in Functions Provided by GCC
