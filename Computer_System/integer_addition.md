# Integer Addition

## Unsigned Addition

Consider two nonnegative integers x and y, such that 0 ≤ x, y ≤ 2^w − 1. Each of these numbers can be represented by w-bit unsigned numbers. If we compute their sum, however, we have a possible range 0 ≤ x + y ≤ 2^w − 2.

```bash
x + y = x + y,   x + y < 2^w
or
x + y − 2^w,     2^w ≤ x + y < 2^(w+1)
```

For example, consider a 4-bit number representation with x = 9 and y = 12, having bit representations [1001] and [1100], respectively. Their sum is 21, having a 5-bit representation [10101]. But if we discard the high-order bit, we get [0101], that is, decimal value 5. This matches the value 21 mod 16 = 5 .


## Two’s-Complement Addition

Given integer values x and y in the range −2^(w − 1) ≤ x, y ≤ 2^(w − 1) − 1, their sum is in the range
− 2^w ≤ x + y ≤ 2^w − 2, potentially requiring w + 1 bits to represent exactly.

```bash
x + y = x + y − 2^w,    2^(w−1) <= x + y              , Positive overflow
or
x + y,                  −2^(w−1) <= x + y < 2^(w−1)   , Normal
or
x + y + 2^w,            x + y < -2^(w−1)              , Negative overflow
```

## References
 - Computer Systems A Programmers Perspective 2nd, Ch.2.3
