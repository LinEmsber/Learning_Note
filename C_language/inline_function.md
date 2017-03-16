# inline function

## Introduction
In the C and C++ programming languages, an inline function is one qualified with the keyword inline;this serves two purposes. Firstly, it serves as a compiler directive that suggests (but does not require) that the compiler substitute the body of the function inline by performing inline expansion, i.e. by inserting the function code at the address of each function call, thereby saving the overhead of a function call.

The second purpose of inline is to change linkage behavior; the details of this are complicated. This is necessary due to the C/C++ separate compilation + linkage model, specifically because the definition (body) of the function must be duplicated in all translation units where it is used, to allow inlining during compiling, which, if the function has external linkage, causes a collision during linking (it violates uniqueness of external symbols; in C++, the One Definition Rule).

An inline function can be written in C or C++ like this:

        inline void swap(int *m, int *n)
        {
                int temp = *m;
                *m = *n;
                *n = temp;
        }

Then, a statement such as the following:

        swap(&x, &y);

will be translated into:

        int temp = x;
        x = y;
        y = temp;

When implementing a sorting algorithm doing lots of swaps, this can increase the execution speed.

It encourages the compiler to build the function into the code where it is used (generally with the goal of improving execution speed).

static inline is usually used with small functions that are better done in the calling routine than by using a call mechanism, simply because they are so short and fast that actually doing them is better than calling a separate copy.



## __always_inline:


By declaring a function inline, you can direct GCC to make calls to that function faster. One way GCC can achieve this is to integrate that function's code into the code for its callers. This makes execution faster by eliminating the function-call overhead


GCC does not inline any functions when not optimizing unless you specify the ‘always_inline’ attribute for the function, like this:

        /* Prototype.  */
        inline void foo (const char) __attribute__((always_inline));


## References:
- [wiki Inline_function](https://en.wikipedia.org/wiki/Inline_function) 
- [Why is static keyword required for inline function?](http://stackoverflow.com/questions/17438510/why-is-static-keyword-required-for-inline-function)
- [why-declare-a-c-function-as-static-inline](http://stackoverflow.com/questions/21835664/why-declare-a-c-function-as-static-inline)
- [GNU gcc online doc](https://gcc.gnu.org/onlinedocs/gcc/Inline.html)
