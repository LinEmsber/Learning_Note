# BUILD_BUG_ON

## Introduction
The BUILD_BUG_ON macro is intended to implement a compile-time assertion.
We can use the macro, BUILD_BUG_ON, to determine the expression, e, which we send in, is whether
correct or not. This macro help us to determine the condition that is error or should not happen
on compiler-time.

### BUILD_BUG_ON_ZERO and BUILD_BUG_ON_NULL:
        /* Force a compilation error if condition is true, but also produce a
         * result (of value 0 and type size_t), so the expression can be used
         * e.g. in a structure initializer (or where-ever else comma expressions
         * aren't permitted). */
        #define BUILD_BUG_ON(condition) ((void)BUILD_BUG_ON_ZERO(condition))
        #define BUILD_BUG_ON_ZERO(e) (sizeof(struct { int:-!!(e); }))
        #define BUILD_BUG_ON_NULL(e) ((void *)sizeof(struct { int:-!!(e); }))

### BUILD_BUG_ON:
        /*
         * BUILD_BUG_ON - break compile if a condition is true.
         * @condition: the condition which the compiler should know is false.
         *
         * If you have some code which relies on certain constants being equal, or
         * some other compile-time-evaluated condition, you should use BUILD_BUG_ON to
         * detect if someone changes it.
         *
         * The implementation uses gcc's reluctance to create a negative array, but gcc
         * (as of 4.4) only emits that error for obvious cases (e.g. not arguments to
         * inline functions).  Luckily, in 4.3 they added the "error" function
         * attribute just for this type of case.  Thus, we use a negative sized array
         * (should always create an error on gcc versions older than 4.4) and then call
         * an undefined function with the error attribute (should always create an
         * error on gcc 4.3 and later).  If for some reason, neither creates a
         * compile-time error, we'll still have a link-time error, which is harder to
         * track down.
         */

        #ifndef __OPTIMIZE__
        #define BUILD_BUG_ON(condition) ((void)sizeof(char[1 - 2*!!(condition)]))
        #else
        #define BUILD_BUG_ON(condition) \
                BUILD_BUG_ON_MSG(condition, "BUILD_BUG_ON failed: " #condition)
        #endif

## You should read this macro like this

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


## Some people have asked: Why not just use an assert?

These macros implement a compile-time test, while assert() is a run-time test.

## Example:

[source code](https://github.com/LinEmsber/Algorithm/blob/master/BUILD_BUG_ON_01.c)

Detect the error on compile-time, as follows:
- 1. check the size of a linked list node.
- 2. check the size of a linked list node is large than its member: val.


### typedef
        typedef struct node {
        	int val;
        	void * data;
        	struct node * next;
        }node_t;

### main
        int main()
        {
        	node_t * n;
        	n = malloc( sizeof(*n) );
        	n->val = 10;

        	printf("sizeof(n): %lu\n", sizeof(*n));
        	printf("sizeof(n): %lu\n", sizeof(n->val));

        	/* It only can be used in compile time value. */
        	BUILD_BUG_ON ( (sizeof(*n) != 24) );
        	BUILD_BUG_ON ( sizeof(n->val) > sizeof(*n) );

        	/* The run time value cannot be detectd, we only can use assert().
        	 * BUILD_BUG_ON ( n->val != 10 );
        	 */
        	assert ( n->val == 10 );

        	return 0;
        }

## References:

- [what-is-in-c-code](http://stackoverflow.com/questions/9229601/what-is-in-c-code)
- [linux/kernel.h](http://lxr.free-electrons.com/source/tools/include/linux/kernel.h#L30)
- [linux/bug.h](http://lxr.free-electrons.com/source/include/linux/bug.h#L30)
- [linux-kernel-buildbugonzero](http://frankchang0125.blogspot.tw/2012/10/linux-kernel-buildbugonzero.html)
