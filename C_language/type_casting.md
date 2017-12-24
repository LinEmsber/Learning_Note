# Type Casting

## Introduction
malloc() returns a pointer of type
```c
void*
```
and we can assign it to
```c
int*
```
to denotes a type cast that explicitly converts the pointer’s type to
```c
int*
```

is it good programming style to write this cast explicitly?

Converting pointer types is in general an unsafe operation in C. Careless use of pointer casts can lead to either spectacular crashes if you get the type wrong, or subtle misbehavior if you try to "cleverly" use the same memory with multiple types and end up violating the strict aliasing rules. But a void * pointer must be converted (implicitly or explicitly) in order for its memory to be used at all, so it’s important to have good conventions that make bugs easier to spot.


## Option 1

```c
void *p = malloc(sizeof(int));
```

This is obviously a bad idea. There’s no type conversion at the initialization site, but now all the users of p needs to convert it, so cast operators will need be scattered throughout large sections of your code. The compiler cannot check that all of the conversions are consistent with each other and with the definition. Don’t do this.

## Option 2
```c
int *p = malloc(sizeof(int));
```

Here the conversion happens once implicitly at the initialization site. p can be used without casting it, and most misuses of p will be detected by the compiler. There’s still a potential mismatch between the two mentions of the type that would not be detected, and a programmer might change one while forgetting to change the other:

```c
long *p = malloc(sizeof(int)).
```
The programmer will probably notice if the declaration and initialization are on the same line, but it might be harder to notice if they’re far away from each other:

```c
/* lots of code; */
long *p;
p = malloc(sizeof(int)).
```

## Option 3

```c
int *p = (int *)malloc(sizeof(int));
```

Some C programmers will complain that this looks redundant: we have to mention the type three times instead of two! However, the compiler will detect a mismatch between the first two mentions, so this isn’t any more error-prone than option 2. Furthermore, it has the advantage that the last two mentions will always be on the same line, even if the initialization is separated from the declaration. This makes programmers more likely to notice any mismatches that the compiler doesn’t. So it’s generally a good idea, if you can’t use one of the options below.


## Option 4

```c
int *p = malloc( sizeof(*p) );
```
or

```c
int *p;
p = malloc( sizeof(*p) );
```

If you haven’t seen this before, it looks like a circular definition, but it turns out that this kind of circular definition is perfectly fine. It’s nice and compact and leaves no possibility of type mismatches (as long as you remember to write exactly one * in sizeof *p, no matter how many *s the type might have!). There are some minor downsides, though. It’s sometimes useful to be able to search your code for all the places where an object of a given type is allocated, and this pattern makes it hard to do that with a textual search utility.


## Option 5

```c
#define malloc_type(type) ( typeof(type) * ) malloc (sizeof(type) )
int *p = malloc_type(int);
```

The macro guarantees that the pointer is immediately converted to the correct type for the allocated size, and the compiler guarantees that it’s assigned to a variable of the correct type. I think this is the best overall solution in C. Note however that this macro uses the nonstandard, but commonly implemented typeof operator—without that, it won’t work with types involving array or function pointers because the extra * won’t go in the right place.

GLib provides a macro like this as g_new(), or for its slice allocator, g_slice_new().


## Option 6 (C++):

```c
int *p = new int;
```

C++ is generally much better at type safety than C, so if you get to use C++, you should take advantage of it. Be careful not to mix new with free() or malloc() with delete, though.


## Option 7

```c
int n;
int *p = &n;
```

Just because you need a pointer doesn’t mean it needs to be allocated on the heap. It’s perfectly fine to take the address of a normal stack variable, as long as you remember that the pointer cannot be used after the variable goes out of scope.


## References
 - [Linux Programmer's Manual, MALLOC(3)](http://man7.org/linux/man-pages/man3/realloc.3.html)
 - [In a C program with the function (int *) malloc (sizeof(int)), what does (int *) mean?](https://www.quora.com/In-a-C-program-with-the-function-int-*-malloc-sizeof-int-what-does-int-*-mean)
