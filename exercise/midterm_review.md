---
layout: default
title: "Midterm exam review questions"
---

# Binary data representation

Assume that `uint8_t` is an unsigned, 8 bit integer data type.

Assume that `int8_t` is a signed, 8 bit, two's complement integer data type.

(a)

Consider the following C code:

```c
uint8_t a = 61,
        b = 129,
        c = 253;
int8_t  x = 42,
        y = -1,
        z = -126;
```

Write the binary representations of `a`, `b`, `c`, `x`, `y`, and `z`.

(b)

What is the output of the following C code? Justify your answers briefly.

```c
uint8_t a = 129, b = 137;
int8_t x = 123, y = 7;

uint8_t c = a + b;
int8_t z = x + y;
printf("%u\n", c);
printf("%d\n", z);
```

(c)

Consider the following C function:

```c
int8_t negate(int8_t x) {
  return -x;
}
```

Complete the following C function so that it behaves identically to the `negate` function, in the sense that when it is given an 8 bit binary value, it will return an 8 bit value with the same bit pattern as what would be returned from the `negate` function shown above.  You may not use type casts, but you may use bitwise operations and arithmetic for `uint8_t` values.  Hint: `~` is the bitwise complement operator.

```c
uint8_t negate_u(uint8_t x) {
  // add your code here
```
