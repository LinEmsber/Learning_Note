# Side effect (computer science)

# Introduction
In computer science, a function or expression is said to have a side effect if it modifies some state or has an observable interaction with calling functions or the outside world. For example, a particular function might modify a global variable or static variable, modify one of its arguments, raise an exception, write data to a display or file, read data, or call other side-effecting functions.

In the presence of side effects, a program's behaviour may depend on history; that is, the order of evaluation matters. Understanding and debugging a function with side effects requires knowledge about the context and its possible histories.

Assembly language programmers must be aware of hidden side effects — instructions that modify parts of the processor state which are not mentioned in the instruction's mnemonic. A classic example of a hidden side effect is an arithmetic instruction that implicitly modifies condition codes (a hidden side effect) while it explicitly modifies a register (the overt effect)

## Temporal side effects

Side effects caused by the time taken for an operation to execute are usually ignored when discussing side effects and referential transparency. There are some cases, such as with hardware timing or testing, where operations are inserted specifically for their temporal side effects e.g. Sleep(5000) or for(int i=0; i < 10000; i++){}. These instructions do not change state other than taking an amount of time to complete.

## Example

 * Environment: Linux 4.4.0-59-generic Ubuntu 16.04 x86_64
 * GCC: gcc version 5.4.0 20160609 (Ubuntu 5.4.0-6ubuntu1~16.04.4)
 * Clang: clang version 3.8.0-2ubuntu4 (tags/RELEASE_380/final)

### case 1
 * The source code

```clike=
#include <stdio.h>
int main()
{
        int i;
        int arr[] = {6, 7, 8, 9, 10};
        int *ptr = arr;

        // increment once
        *(ptr++) += 123;

        // print out two different results in GCC and Clang.
        printf("*ptr: %d, *(++ptr): %d\n " , *ptr, *(++ptr) );

        for ( i = 0; i < 5; i++ ) {
        printf("arr[%d]: %d  ", i, arr[i]);
        }
        printf("\n");
}
```
### execution in GCC

```shell=
> gcc -Wall side_effect_01.c && ./a.out

side_effect_01.c: In function ‘main’:
side_effect_01.c:18:46: warning: operation on ‘ptr’ may be undefined [-Wsequence-point]
  printf("*ptr: %d, *(++ptr): %d\n" , *ptr, *(++ptr) );
                                              ^
*ptr: 8, *(++ptr): 8
arr[0]: 129  arr[1]: 7  arr[2]: 8  arr[3]: 9  arr[4]: 10
```

### execution in Clang

```shell=
> clang -Wall side_effect_01.c && ./a.out
side_effect_01.c:18:46: warning: unsequenced modification and access to 'ptr' [-Wunsequenced]
        printf("*ptr: %d, *(++ptr): %d\n" , *ptr, *(++ptr) );
                                             ~~~    ^
1 warning generated.
*ptr: 7, *(++ptr): 8
arr[0]: 129  arr[1]: 7  arr[2]: 8  arr[3]: 9  arr[4]: 10
```


### case 2

```clike=
#include <stdio.h>
int main()
{
        int i;
        int arr[] = {6, 7, 8, 9, 10};
        int *ptr = arr;

        // increment twice
        *(ptr++) = *(ptr++) + 123;

        // print out two different results in GCC and Clang.
        printf("*ptr: %d, *(++ptr): %d\n " , *ptr, *(++ptr) );

        for ( i = 0; i < 5; i++ ) {
                printf("arr[%d]: %d  ", i, arr[i]);
        }
        printf("\n");
}
```

### execution in GCC

```shell=
> gcc -Wall side_effect_01.c && ./a.out

side_effect_01.c: In function ‘main’:
side_effect_01.c:15:18: warning: operation on ‘ptr’ may be undefined [-Wsequence-point]
  *(ptr++) = *(ptr++) + 123;
                  ^
side_effect_01.c:18:46: warning: operation on ‘ptr’ may be undefined [-Wsequence-point]
  printf("*ptr: %d, *(++ptr): %d\n" , *ptr, *(++ptr) );
                                              ^
*ptr: 9, *(++ptr): 9
arr[0]: 130  arr[1]: 7  arr[2]: 8  arr[3]: 9  arr[4]: 10
```

### execution in Clang

```shell=
> clang -Wall side_effect_01.c && ./a.out

side_effect_01.c:15:7: warning: multiple unsequenced modifications to 'ptr' [-Wunsequenced]
        *(ptr++) = *(ptr++) + 123;
             ^          ~~
side_effect_01.c:18:46: warning: unsequenced modification and access to 'ptr' [-Wunsequenced]
        printf("*ptr: %d, *(++ptr): %d\n" , *ptr, *(++ptr) );
                                             ~~~    ^
2 warnings generated.
*ptr: 8, *(++ptr): 9
arr[0]: 6  arr[1]: 129  arr[2]: 8  arr[3]: 9  arr[4]: 10
```


## References
 * [Side effect (computer science)](https://en.wikipedia.org/wiki/Side_effect_(computer_science))
 * [comp.lang.c FAQ list · Question 3.2](http://c-faq.com/expr/evalorder2.html)
 * [operation on `x' may be undefined?](https://bytes.com/topic/c/answers/222558-operation-x-may-undefined)
 * KnR C, 2nd, Ch2.12, p.54
