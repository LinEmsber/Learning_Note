# idempotence

## Introduction
Idempotence is the property of certain operations in mathematics and computer science, that can be applied multiple times without changing the result beyond the initial application.

There are several meanings of idempotence, depending on what the concept is applied to:

A unary operation (or function) is idempotent if, whenever it is applied twice to any value, it gives the same result as if it were applied once; i.e., ƒ(ƒ(x)) ≡ ƒ(x). For example, the absolute value function, where abs(abs(x)) ≡ abs(x), is idempotent.

Given a binary operation, an idempotent element (or simply an "idempotent") for the operation is a value for which the operation, when given that value for both of its operands, gives that value as the result.
For example, the number 1 is an idempotent of multiplication: 1 × 1 = 1.

A binary operation is called idempotent if all elements are idempotent elements with respect to the operation. In other words, whenever it is applied to two equal values, it gives that value as the result.
For example, the function giving the maximum value of two equal values is idempotent: max(x, x) ≡ x.


## Computer science meaning

In computer science, the term idempotent is used more comprehensively to describe an operation that will produce the same results if executed once or multiple times.

This may have a different meaning depending on the context in which it is applied. In the case of methods or subroutine calls with side effects, for instance, it means that the modified state remains the same after the first call.

However, placing an order for a car for the customer is typically not idempotent, since running the call several times will lead to several orders being placed. But, canceling an order is idempotent, because the order remains canceled no matter how many requests are made.


In the Hypertext Transfer Protocol (HTTP), idempotence and safety are the major attributes that separate HTTP verbs.

Of the major HTTP verbs, GET, PUT, and DELETE should be implemented in an idempotent manner according to the standard, but POST need not be. GET retrieves a resource; PUT stores content at a resource; and DELETE eliminates a resource.

As in the example above, reading data usually has no side effects, so it is idempotent. Storing and deleting a given set of content are each usually idempotent as long as the request specifies a location or identifier that uniquely identifies that resource and only that resource again in the future. The PUT and DELETE operations with unique identifiers reduce to the simple case of assignment to an immutable variable of either a value or the null-value, respectively, and are idempotent for the same reason; the end result is always the same as the result of the initial execution.


## References
 - [Idempotence wiki](https://en.wikipedia.org/wiki/Idempotence)
