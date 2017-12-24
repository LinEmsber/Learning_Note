# Constant pointers and Pointer to constants

## Constant Pointers:

A constant pointer is a pointer that cannot change the address its holding.
In other words, we can say that once a constant pointer points to a variable then it cannot point to any other variable.

A constant pointer is declared as follows:

```bash
(type of pointer) * const (name of pointer)
```

An Exammple declaration would look like :
```c
int * const ptr;
```

### Exammple:

```c
#include<stdio.h>

int main(void)
{
        int var1 = 0, var2 = 0;
        int *const ptr = &var1;
        ptr = &var2;
        printf("%d\n", *ptr);

        return 0;
}
```


### Compile

```bash
$ gcc -Wall constptr.c -o constptr
constptr.c: In function ‘main’:
constptr.c:7: error: assignment of read-only variable ‘ptr’
```


## Pointer to Constant

These type of pointers can change the address they point to but cannot change the value kept at
those address.

A pointer to constant is defined as

```bash
const (type of pointer) * (name of pointer)
```

An exammple of definition could be

```c
const int* ptr;
```

### Exammple

```c
#include<stdio.h>
{        int main(void)
        {
                int var1 = 0;
                const int* ptr = &var1;
                *ptr = 1;
                printf("%d\n", *ptr);

                return 0;
        }
}
```

### Compile

```bash
$ gcc -Wall constptr.c -o constptr
constptr.c: In function ‘main’:
constptr.c:7: error: assignment of read-only location ‘*ptr’
```


## Constant Pointer to a Constant

A constant pointer to constant is a pointer that can neither change the address its pointing to and nor it can change the value kept at that address.

A constant pointer to constant is defined as

```bash
const (type of pointer) * const (name of pointer)
```

for Exammple
```c
const int* const ptr;
```

### Exammple

```c
#include<stdio.h>

int main(void)
{
        int var1 = 0,var2 = 0;
        const int* const ptr = &var1;
        *ptr = 1;
        ptr = &var2;
        printf("%d\n", *ptr);

        return 0;
}
```

### Compile

```bash
$ gcc -Wall constptr.c -o constptr
constptr.c: In function ‘main’:
constptr.c:7: error: assignment of read-only location ‘*ptr’
constptr.c:8: error: assignment of read-only variable ‘ptr’
```
