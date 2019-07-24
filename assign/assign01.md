---
layout: default
title: "Assignment 1: Arbitrary-precision arithmetic"
---

Due: *TBD*

# Overview

In this assignment you will implement a simple C library for arbitrary-precision integer arithmetic.

The grading breakdown is as follows:

* Impementation of data type and functions: 60%
* Unit tests: 30%
* Design and coding style: 10%

# Arbitrary-precision integer arithmetic

As you know from the material we have been covering, including Chapter 2 of the textbook, the hardware-supported numeric data types are *finite*, meaning that only a finite set of values can be directly represented using those data types.  Most programming languages (such as C, C++, and Java) use hardware numeric data types to implement their primitive numeric types (`int`, `long`, `double`, etc.)  As you have seen, the finite nature of machine data types leads to some odd situations, such as the existence of values *a* and *b* such that (for example)

> *a ≥ 0*

> *b ≥ 0*

> *a + b &lt; a*

The above inequalities could be true if, for example, the sum *a + b* is too large to represent using the data type to which *a* and *b* belong.

Some applications, such as cryptographic applications, need to represent values which behave like mathematical integers, in which case the machine integer data types can't be used directly.  For such applications, having an arbitrary-precision integer data type can be very useful.

# The `ApInt` data type

An instance of the `ApInt` data type (**A**rbitrary **p**recision **Int**eger) represents an arbitrary nonnegative integer value.  Because `ApInt` instances can represent any arbitrarily-large integer value, they can require an arbitrary amount of memory, and so are accessed via pointers (so that their representations can be allocated dynamically.)

The `ApInt` data type is defined as follows (in the header file **apint.h**):

```c
typedef struct {
    /* TODO: representation */
} ApInt;
```

So, `ApInt` is a typedef (alias) for an unnamed struct type.  You will need to add fields to the unnamed struct type so that an allocated instance of `ApInt` can represent any nonnegative integer value.
