# Time Complexity

## Quick Sort Analysis

In quick sort, we pick an element called the pivot in each step and re-arrange the array in such a way that all elements less than the pivot now appear to the left of the pivot, and all elements larger than the pivot appear on the right side of the pivot. In all subsequent iterations of the sorting algorithm, the position of this pivot will remain unchanged, because it has been put in its correct place.

The total time taken to re-arrange the array as just described, is always O(n), or an where a is some constant.

Let us suppose that the pivot we just chose has divided the array into two parts - one of size k and the other of size n − k. Notice that both these parts still need to be sorted. This gives us the following relation:

```bash
T(n) = T(k) + T(n − k) + an
```

where T(n) refers to the time taken by the algorithm to sort n elements


## WORST CASE ANALYSIS

Now consider the case, when the pivot happened to be the least element of the array, so that we had k = 1 and n − k = n − 1. In such a case, we have:

```bash
T(n) = T(1) + T(n − 1) + an
```

Now let us analyse the time complexity of quick sort in such a case in detail by solving the recurrence as follows:

```bash
T(n) = T(n − 1) + T(1) + an
= [T(n − 2) + T(1) + a(n − 1)] + T(1) + an
```

(Note: I have written T(n − 1) = T(1) + T(n − 2) + a(n − 1) by just substituting n − 1 instead of n. Note the implicit assumption that the pivot that was chosen divided the original subarray of size n − 1 into two parts: one of size n − 2 and the other of size 1.)

```bash
= T(n − 2) + 2T(1) + a(n − 1 + n) (by simplifying and grouping terms together)
= [T(n − 3) + T(1) + a(n − 2)] + 2T(1) + a(n − 1 + n)
= T(n − 3) + 3T(1) + a(n − 2 + n − 1 + n)
= [T(n − 4) + T(1) + a(n − 3)] + 3T(1) + a(n − 2 + n − 1 + n)
= T(n − 4) + 4T(1) + a(n − 3 + n − 2 + n − 1 + n)
= T(n − i) + iT(1) + a(n − i + 1 + ..... + n − 2 + n − 1 + n)
= T(n − i) + iT(1) + a( sum(from j = 0 to i - 1) (n − j) j)
```

Now clearly such a recurrence can only go on until i = n − 1 (Why? because otherwise n − i would be less than 1). So, substitute i = n − 1 in the above equation, which gives us:

```bash
T(n) = T(1) + (n − 1)T(1) + a ( sum (from j = 0 to n − 2) j)
= nT(1) + a(n(n − 2) − (n − 2)(n − 1)/2)
(Notice that sum ( (from j=0 to n-2) j ) = sum ( (from j=1 to n-2 ) j ) = (n − 2)(n − 1)/2 )

which is O(n^2)
```

This is the worst case of quick-sort, which happens when the pivot we picked turns out to be the least element of the array to be sorted, in every step (i.e. in every recursive call). A similar situation will also occur if the pivot happens to be the largest element of the array to be sorted.



## Reference
 - [quick sort analysis] (https://www.cise.ufl.edu/class/cot3100fa07/quicksort_analysis.pdf)
