# Referential transparency

## Introduction
Referential transparency and referential opacity are properties of parts of computer programs. An expression is said to be referentially transparent if it can be replaced with its corresponding value without changing the program's behavior. As a result, evaluating a referentially transparent function gives the same value for same arguments. Such functions are called pure functions. An expression that is not referentially transparent is called referentially opaque.

## Examples 1:

Consider a function that returns the input from some source. In pseudo code, a call to this function might be GetInput(Source) where Source might identify a particular disk file, the keyboard, etc. Even with identical values of Source, the successive return values will be different. Therefore, function GetInput() is neither deterministic nor referentially transparent.


## Examples 2:

In fact, all functions in the mathematical sense are referentially transparent: sin(x) is transparent, since it will always give the same result for each particular x.
A function such as:

```c
int plusone(int x) {
        return x+1;
}
```

is transparent, as it will not implicitly change the input x and thus has no such side effects.

## Examples 3:

As an example, let's use two functions, one which is referentially opaque, and the other which is referentially transparent:

```c
int globalValue = 0;

int rq(int x)
{
        globalValue++;
        return x + globalValue;
}

int rt(int x)
{
        return x + 1;
}
```

The function rt is referentially transparent, which means that rt(x) = rt(y) if x = y. For instance, rt(6) = 6 + 1 = 7, rt(4) = 4 + 1 = 5, and so on. However, we can't say any such thing for rq because it uses a global variable that it modifies.

The referential opacity of rq makes reasoning about programs more difficult. For example, say we wish to reason about the following statement:

```c
int p = rq(x) + rq(y) * (rq(x) - rq(x));
```

One may be tempted to simplify this statement to:

```c
int p = rq(x) + rq(y) * (0);
int p = rq(x) + 0;
int p = rq(x);
```

However, this will not work for rq() because each occurrence of rq(x) evaluates to a different value. Remember that the return value of rq is based on a global value that isn't passed in and which gets modified on each call to rq. This means that mathematical identities such as, x - x = 0, no longer hold.


## References
 - [Referential_transparency Wiki](https://en.wikipedia.org/wiki/Referential_transparency)
