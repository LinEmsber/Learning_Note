
# Using pointer to pointer to manipulate linked list.


## linked list 1

Consider the problem of inserting a number into a sorted linked singly-linked list.

```c
struct node {
        int data;
        struct node *next;
};

struct node *head;
```


A typical solution has two pointers and a few special cases to get right:

```c
void insert(struct node *new_node)
{
        /* two pointers */
        struct node *node = head, *prev = NULL;

        while (node != NULL && node->data < new_node->data) {
                prev = node;
                node = node->next;
        }

        new_node->next = node;

        if (prev == NULL)
                head = new_node;
        else
                prev->next = new_node;
}
```


But with clever use of a pointer to a pointer, you can do this with no extra cases:

```c
void insert(struct node *new_node)
{
        struct node **link = &head;

        while ( *link != NULL && (*link)->data < new_node->data ){

                // link = & ( (*link) -> next );
                link = &(*link)->next;
        }

        new_node->next = *link;
        *link = new_node;
}
```

# linked list 1 Explanation:

Support that the original node number of list: 3, 5, 7,and 9, and new_node: 8.

```c
link = &(*link)->next;
```

When link become the address of node number 9, it is not satisfied condition of while loop, so
leave this while loop.

```c
new_node -> next = *link;
```

The next node of the node number 8 is assigned the node number 9.

```c
*link = new_node;
```

It replaces the value of the number 9 by the node number 8, the value of the node number 9 is
assigned the node number 8. Thus, the next node of the node number 7 become the node number 8.


The same trick applies to node deletion. It’s also useful to store the previous links this way in
non-circular doubly-linked list implementations, such as the Linux kernel’s hlist.


## linked list 2


As a final example, let's look at how pointers to pointers can be used to eliminate a nuisance we've
had when trying to insert and delete items in linked lists. For simplicity, we'll consider lists of
integers, built using this structure:

```c
struct list {
        int item;
        struct list *next;
};
```

Suppose we're trying to write some code to delete a given integer from a list. The straightforward
solution looks like this:

```c
/* delete node containing i from list pointed to by lp */

struct list *lp, *prevlp;

for( lp = list; lp != NULL; lp = lp->next ) {

	if(lp->item == i) {

                if(lp == list){
                        list = lp->next;
                } else	{
                        prevlp->next = lp->next;
                }

		break;
	}

	prevlp = lp;
}
```

We can write another version of the list-deletion code, which is (in some ways, at least) much cleaner,
by using a pointer to a pointer to a struct list.

```c
struct list **lpp;

for(lpp = &list; *lpp != NULL; lpp = &(*lpp)->next) {

        if((*lpp)->item == i) {

                *lpp = (*lpp)->next;
                break;
        }
}
```

## source code:

- [source code](https://github.com/LinEmsber/Data_Structure/blob/master/singly_linked_list/singly_linked_list.c)


```c
int node_delete(node_t * head, int target_val)
{
        if (head == NULL)
                return -1;

        int ret = -1;

	/*
        I use tool: gdb to analyze argument, head, and variable, **PP.
	At the begin of this functions:

		(gdb) p *head
		$1 = {val = 0, next = 0x602150}
		(gdb) p head
		$2 = (node_t *) 0x602010
		(gdb) p &head
		$3 = (node_t **) 0x7fffffffdad8

	After the "node_t **pp = &head;" :

		(gdb) p **pp
		$4 = {val = 0, next = 0x602150}
		(gdb) p *pp
		$5 = (node_t *) 0x602010
		(gdb) p pp
		$6 = (node_t **) 0x7fffffffdad8

	*/

	node_t **pp = &head;
	node_t *entry = head;

	while(entry){
		if(entry->val == target_val){
                        *pp = entry->next;
                        free(entry);
                        ret = 0;

			/* Remember to break from the while loop.
			 * The next two expressions will make errors happen,
			 * because the variable, entry, is freed, it cannot be operated.
			 */
			break;
                }

		pp = &entry->next;
		entry = entry->next;
	}

	return ret;
}

```
### References:
- [Chapter 22: Pointers to Pointers](http://c-faq.com/~scs/cclass/int/sx8.html)
- [In C or C++, what are your favorite pointer tricks?](https://www.quora.com/In-C-or-C++-what-are-your-favorite-pointer-tricks/answer/Anders-Kaseorg)
- [Linus on Understanding Pointers](https://grisha.org/blog/2013/04/02/linus-on-understanding-pointers/)
- [linux/list.h v4.9](http://lxr.free-electrons.com/source/include/linux/list.h?v=4.9#L620)
