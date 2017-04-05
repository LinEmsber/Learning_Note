# The Macro: container_of()


## Introduction
What does it do? Well, precisely what its name indicates.
It takes three arguments – a pointer, type of the container, and the name of the member the pointer refers to.
The macro will then expand to a new address pointing to the container which accommodates the respective member.

On the header linux/kernel.h at line 830, as follow:

```c
/**
* container_of - cast a member of a structure out to the containing structure
* @ptr:        the pointer to the member.
* @type:       the type of the container struct this is embedded in.
* @member:     the name of the member within the struct.
*
*/
#define container_of(ptr, type, member) ({                      \
        const typeof( ((type *)0)->member ) *__mptr = (ptr);    \
        (type *)( (char *)__mptr - offsetof(type,member) );})
```

## Statements in Expressions:

There is just some kind of weird ({}) block with two statements in it.
This in fact is a GNU extension to C language called braced-group within expression.
The compiler will evaluate the whole block and use the value of the last statement contained in the block.

Take for instance the following code. It will print 5.

```c
int x = ({1; 2;}) + 3;
printf("%d\n", x);
```

## typeof():
This is a non-standard GNU C extension. It takes one argument and returns its type.

```c
int x = 5;
typeof(x) y = 6;
printf("%d %d\n", x, y);
```

## Zero Pointer Dereference:

It’s a little pointer magic to get the type of the member. It won’t crash, because the expression itself
will never be evaluated. All the compiler cares for is its type. The same situation occurs in case we ask
back for the address. The compiler again doesn’t care for the value, it will simply add the offset of the
member to the address of the structure, in this particular case 0, and return the new address.

```c
struct s {
	int32_t member_0;
        int8_t member_1;
        int8_t member_2;
        int16_t member_3;
        int32_t member_4;
};

/* This will print 6 */
printf("&( (struct s*) 0 ) -> member_3: %zu\n", &( (struct s*) 0 ) -> member_3 );
```



## offsetof:

This macro will return a byte offset of a member to the beginning of the structure.
It is a little bit of the same 0 pointer dereference magic as we saw earlier and to avoid that modern
compilers usually offer a built-in function, that implements that.

Here is the messy version (from the kernel):

```c
#define offsetof(TYPE, MEMBER) ( (size_t) & ( (TYPE *) 0 ) -> MEMBER )

// printf("&( (struct s*) 0 ) -> member_3: %zu\n", &( (struct s*) 0 ) -> member_3 );
printf("offsetof(struct s, member_3): %zu\n", offsetof(struct s, member_3) );

```

It returns an address of a member called MEMBER of a structure of type TYPE that is stored in
memory from address 0 (which happens to be the offset we’re looking for).

## Putting It All Together:

```c
#define container_of(ptr, type, member)({ \
        const typeof( ((type *)0)->member ) *__mptr = (ptr); \
        (type *)( (char *)__mptr - offsetof(type,member) );})
```

The first line is not intrinsically important for the result of the macro, but it is there for type
checking purposes. And what the second line really does? It subtracts the offset of the structure’s
member from its address yielding the address of the container structure.


## EXAMPLE:

When you use the cointainer_of macro, you want to retrieve the structure that contains the pointer
of a given field. For example:

```c
struct numbers {
        int one;
        int two;
        int three;
} n;

int *n_two_ptr = &n.two;
struct numbers *n_ptr;
n_ptr = container_of(n_two_ptr, struct numbers, two);
```

You have a pointer that points in the middle of a structure (and you know that is a pointer to the
field two [the field name in the structure]), but you want to retrieve the entire structure
(numbers). So, you calculate the offset of the field two in the structure:

```c
offsetof(type,member)
```

and subtract this offset from the given pointer.

The result is the pointer to the start of the structure.
Finally, you cast this pointer to the structure type to have a valid variable.



## references:
- [linux/kernel.h](http://lxr.free-electrons.com/source/include/linux/kernel.h#L830)
- [magical-container_of-macro](http://radek.io/2012/11/10/magical-container_of-macro/)
- [understanding-container-of-macro-in-linux-kernel](http://stackoverflow.com/questions/15832301/understanding-container-of-macro-in-linux-kernel)
- [list-entry-in-linux](http://stackoverflow.com/questions/5550404/list-entry-in-linux)
