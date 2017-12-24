# Integer Multiply

## Unsigned Multiply

Integers x and y in the range 0 ≤ x, y ≤ 2^w − 1 can be represented as w-bit unsigned numbers, but their product x. y can range between 0 and (2w − 1)^2 = 2^(2w) − 2^(w+1) + 1. Instead, unsigned multiplication in C is defined to yield thew-bit value given by the low-order w bits of the 2w-bit integer product. Thus, the effect of the w-bit unsigned multiplication operation * is

```bash
x * y = (x * y) mod 2w
```



## Two’s-Complement Multiply

Integers x and y in the range − 2^(w−1) ≤ x, y ≤ 2^(w−1) − 1 can be represented as wbit two’s-complement numbers, but their product x * y can range between:

```bash
− 2^(w−1) * (2 ^ (w−1) − 1) = − 2 ^ (2w−2) + 2 ^ (w−1)
and
− 2 ^ (w−1) * − 2 ^ (w−1) = 2 ^ (2w−2).
```

```bash
Mode            x               y               x * y           Truncated x * y
Unsigned        5 [101]         3 [011]         15 [001111]     7 [111]
Two’s comp.     −3 [101]        3 [011]         −9 [110111]     −1 [111]
Unsigned        4 [100]         7 [111]         28 [011100]     4 [100]
Two’s comp.     −4 [100]        −1 [111]        4 [000100]      −4 [100]
Unsigned        3 [011]         3 [011]         9 [001001]      1 [001]
Two’s comp.     3 [011]         3 [011]         9 [001001]      1 [001]
```

## References:
 - Computer Systems A Programmers Perspective 2nd, Ch.2.3
