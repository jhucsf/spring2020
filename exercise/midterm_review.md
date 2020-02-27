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

Write the values of `a`, `b`, `c`, `x`, `y`, and `z` in binary.

(b)

What is the output of the following C code? Justify your answers briefly.

```c
uint8_t a = 129, b = 137;
int8_t x = 129, y = 7;

uint8_t c = a + b;
int8_t z = x + y;
printf("%u\n", c);
printf("%d\n", z);
```
