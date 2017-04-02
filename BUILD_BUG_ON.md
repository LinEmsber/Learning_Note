===============================================================================
                        build bug on zero
===============================================================================

We can use the macro, BUILD_BUG_ON_ZERO, to determine the expression, e, which we send in, is whether correct
or not. This macro help us to determine the condition that is error or should not happen on compiler-time.

        30 /* Force a compilation error if condition is true, but also produce a
        31    result (of value 0 and type size_t), so the expression can be used
        32    e.g. in a structure initializer (or where-ever else comma expressions
        33    aren't permitted). */
        34 #define BUILD_BUG_ON_ZERO(e) (sizeof(struct { int:-!!(e); }))
        35 #define BUILD_BUG_ON_NULL(e) ((void *)sizeof(struct { int:-!!(e); }))

You should read this macro like this:

1. (e):
        Compute expression e.

2. !!(e):
        Logically negate twice: 0 if e == 0; otherwise 1.

3. -!!(e):
        Numerically negate the expression from step 2: 0 if it was 0; otherwise -1.

4. struct{int: -!!(0);} -> struct{int: 0;}:

        If it was zero, then we declare a struct with an anonymous integer bitfield that has width zero.
        Everything is fine and we proceed as normal.

5. struct{int: -!!(1);} -> struct{int: -1;}:

        On the other hand, if it isn't zero, then it will be some negative number.
        Declaring any bitfield with negative width is a compilation error.


Some people have asked: Why not just use an assert?

        These macros implement a compile-time test, while assert() is a run-time test.


References:
http://stackoverflow.com/questions/9229601/what-is-in-c-code
http://lxr.free-electrons.com/source/tools/include/linux/kernel.h#L30
http://lxr.free-electrons.com/source/include/linux/bug.h#L30
http://frankchang0125.blogspot.tw/2012/10/linux-kernel-buildbugonzero.html
