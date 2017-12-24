# pointer to pointer

## Basic operation

The declaration of a pointer-to-pointer looks like

```c
int **ipp;
```

where the two asterisks indicate that two levels of pointers are involved.

Starting off with the familiar, uninspiring, kindergarten-style examples, we can demonstrate the use of ipp by declaring some pointers for it to point to and some ints for those pointers to point to:

```c
int i = 5, j = 6; k = 7;
int *ip1 = &i, *ip2 = &j;
```

Now we can set

```c
ipp = &ip1;
```

and ipp points to ip1 which points to i. *ipp is ip1, and **ipp is i, or 5.

If we say

```c
*ipp = ip2;
```

we've changed the pointer pointed to by ipp (that is, ip1) to contain a copy of ip2, so that
it (ip1) now points at j.

If we say

```c
*ipp = &k;
```

we've changed the pointer pointed to by ipp (that is, ip1 again) to point to k.


## References
 - [Pointers to Pointers](http://c-faq.com/~scs/cclass/int/sx8.html)
