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

Some applications, such as cryptographic applications, need to represent values which behave like mathematical integers, in which case the machine integer data types can't be used directly.  Some programming languages, notably languges in the LISP family, have first-class support for arbitrary-precision integers (often referred to as "bignums".)  For programming languages without such support, having an arbitrary-precision integer data type can be very useful.

# The `ApInt` data type

An instance of the `ApInt` data type (**A**rbitrary **p**recision **Int**eger) represents an arbitrary nonnegative integer value.

The `ApInt` data type is defined as follows (in the header file **apint.h**):

```c
typedef struct {
    /* TODO: representation */
} ApInt;
```

So, `ApInt` is a typedef (alias) for an unnamed struct type.  You will need to add fields to the unnamed struct type so that an allocated instance of `ApInt` can represent any nonnegative integer value.

Because `ApInt` instances can represent any arbitrarily-large integer value, they can require an arbitrary amount of memory, and so are accessed via pointers (so that their representations can be allocated dynamically.)

A set of functions is defined to allow a program to create, use, and destroy instance of `ApInt`.  All of these functions have named beginning with `apint`.  (It is a good practice when designing APIs for C programs to use a common naming prefix for functions associated with a particular data type.)  These functions are:

```c
ApInt *apintCreateFromU64(uint64_t val);
ApInt *apintCreateFromHex(const char *hex);
void apintDestroy(ApInt *ap);
int apintHighestBitSet(ApInt *ap);
ApInt *apintLshift(ApInt *ap);
ApInt *apintLshiftN(ApInt *ap, unsigned n);
char *apintFormatAsHex(ApInt *ap);
ApInt *apintAdd(const ApInt *a, const ApInt *b);
int apintCompare(const ApInt *left, const ApInt *right);
```

Here are brief descriptions of the expected behavior of these functions.

`apintCreateFromU64`: Returns a pointer to an `ApInt` instance whose value is specified by the `val` parameter, which is a 64-bit unsigned value.

`apintCreateFromHex`: Returns a pointer to an `ApInt` instance whose value is specified by the `hex` parameter, which is an arbitrary sequence of hexadecimal (base 16) digits.

`apintDestroy`: Deallocates the memory used by the `ApInt` instance pointed-to by the `ap` parameter.

`apintHighestBitSet`: Returns the position of the most significant bit set to 1 in representation of the `ApInt` pointed to by `ap`. As a special case, returns -1 if the `ApInt` instance pointed to by `ap` represents the value 0.

`apintLshift`: Returns a pointer to an `ApInt` instance formed by shifting each bit of the `ApInt` instance pointed to by `ap` one position to the left.

`apintLshiftN`: Returns a pointer to an `ApInt` instance formed by shifting each bit of the `ApInt` instance pointed to by `ap` *n* positions to the left.


`apintFormatAsHex`: Returns a pointer to a dynamically-allocated C character string containing the hexadecimal (base 16) digits of the representation of the `ApInt` instance pointed to by `ap`.  Note that the hex digits representing the values 10 through 15 should be *lower-case* `a` through `f`.  The string returned should not have any leading zeroes, except in the special case of the `ApInt` instance representing the value 0, in which case the returned string should consist of a single `0` digit.

`apintAdd`: Adds the values represented by the `ApInt` instances pointed-to by the parameters `a` and `b`, and returns a pointer to an `ApInt` instance representing their sum.

`apintCompare`: Compares the values represented by the `ApInt` instances pointed-to by the parameters `left` and `right`.  Returns a negative value if `left` is less that `right`, a positive value if `left` is greater than `right`, and 0 if the values are equal.

# Tasks

This section explains the tasks you are responsible for completing.  Note that they aren't meant to be strictly sequential!

## Task 1: Determine a data representation

You will need to determine a suitable data representation for `ApInt` values by specifying fields within the `ApInt` data type.

The basic idea is that the numeric value of an `ApInt` instance is stored in a variable-length array of fixed-precision integer values.

The representation should be reasonably space efficient.  For example, an array of `uint32_t` or `uint64_t` values, such that the bits of the overall integer value are packed into the array elements, would be a space-efficient representation, because at most some small number (63 or fewer) of bits per instance would be "wasted" (by being leading 0 bits in the array element representing the most significant chunk of the bit string.)  In contrast, representing the numeric value using hexadecimal digits stored n an array of `char` values would *not* be a space-efficient representation, since each `char` element would store one of only 16 possible values, wasting half of the bits in the array.

Note that you could represent the value using an array of small (8 or 16 bit) integer elements.  However, the `apintAdd` function can be implemented more efficiently if it can operate on larger chunks of data.

The `ApInt` representation should have a way of keeping track of the length of the array storing the bit string.  The idea is that `ApInt` values requiring more bits to represent will (in general) require more storage.  The functions which operate on `ApInt` values will need to know the array length of each `ApInt` instance.

## Task 1: Function implementation

Your main task is to implement all of the required functions as described above.


## Task 2: Unit testing

Yeah.
