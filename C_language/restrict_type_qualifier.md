# Restrict Type Qualifier

In the C programming language, as of the C99 standard, restrict is a keyword that can be used in pointer declarations. The restrict keyword is a declaration of intent given by the programmer to the compiler. It says that for the lifetime of the pointer, only the pointer itself or a value directly derived from it (such as pointer + 1) will be used to access the object to which it points. This limits the effects of pointer aliasing, aiding optimizations. If the declaration of intent is not followed and the object is accessed by an independent pointer, this will result in undefined behavior.

During each execution of a block in which a restricted pointer P is declared (typically each execution of a function body in which P is a function parameter), if some object that is accessible through P (directly or indirectly) is modified, by any means, then all accesses to that object (both reads and writes) in that block must occur through P (directly or indirectly), otherwise the behavior is undefined:

```c
void f(int n, int * restrict p, int * restrict q)
{
    while(n-- > 0)
        *p++ = *q++; // none of the objects modified through *p is the same
                     // as any of the objects read through *q
                     // compiler free to optimize, vectorize, page map, etc.
}
void g(void)
{
    extern int d[100];
    f(50, d + 50, d); // OK
    f(50, d + 1, d); // Undefined behavior: d[1] is accessed through both p and q in f
}
```

If the object is never modified, it may be aliased and accessed through different restrict-qualified pointers (note that if the objects pointed to by aliased restrict-qualified pointers are, in turn, pointers, this aliasing can inhibit optimization).

```c
int* restrict p1 = &a;
int* restrict p2 = &b;
p1 = p2; // undefined behavior
```

Assignment from one restricted pointer to another is undefined behavior, except when assigning from a pointer to an object in some outer block to a pointer in some inner block (including using a restricted pointer argument when calling a function with a restricted pointer parameter) or when returning from a function (and otherwise when the block of the from-pointer ended):

Restricted pointers can be assigned to unrestricted pointers freely, the optimization opportunities remain in place as long as the compiler is able to analyze the code:

```c
void f(int n, float * restrict r, float * restrict s) {
   float * p = r, * q = s; // OK
   while(n-- > 0) *p++ = *q++; // almost certainly optimized just like *r++ = *s++
}
```

If an array type is declared with the restrict type qualifier (through the use of typedef), the array type is not restrict-qualified, but its element type is:

```c
typedef int *array_t[10];
restrict array_t a; // the type of a is int *restrict[10]
```


# References:
 - [restrict type qualifier](http://en.cppreference.com/w/c/language/restrict)
 - [如何理解C語言關鍵字restrict？](https://www.getit01.com/p20180105441653775/)
 - [Restrict Wiki](https://en.wikipedia.org/wiki/Restrict)
