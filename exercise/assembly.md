---
layout: default
title: "Assembly language exercise"
---

# Getting started

This is an in-class assembly language exercise.

You should work in groups of 2 or 3 people (form a group with classmates who are sitting near you.)

# Task

You will write an x86-64 assembly language program that does the following:

1. Read 20 integer values into an array (you can represent them as either 32 bit or 64 bit, and the program should allow both positive and negative values)
2. Iterate through the array and keep track of how many values are in each of the following ranges: 0-20, 21-40, 41-60, 61-80, 81-100
3. Print out the counts for each range

A good first milestone would be to write a program that reads the values into an array, and then just prints them out and exits.

## Hello, world

Here is a simple assembly language program that you can use as a template:

```
/* hello.S */

.section .rodata

sHelloMsg:  .string "Hello, world!\n"
sPromptMsg: .string "Enter an integer: "
sInputFmt:  .string "%ld"
sResultMsg: .string "You entered: %ld\n"

.section .bss

num: .space 8

.section .text

	.globl main
main:
	subq $8, %rsp

	movq $sHelloMsg, %rdi
	call printf

	movq $sPromptMsg, %rdi
	call printf

	movq $sInputFmt, %rdi
	movq $num, %rsi
	call scanf

	movq $sResultMsg, %rdi
	movq num, %rsi
	call printf

	addq $8, %rsp
	ret

/*
vim:ft=gas:
*/
```

## Assembling and testing

You can assemble and run the Hello, world program as follows (user input shown in **bold**):

<div class="highlighter-rouge"><pre>
$ <b>gcc -c -no-pie -o hello.o hello.S</b>
$ <b>gcc -no-pie -o hello hello.o</b>
$ <b>./hello</b>
Hello, world!
Enter an integer: <b>42</b>
You entered: 42
</pre></div>

## Tips and suggestions

The easiest way to allocate storage for the arrays is to make them global variables in the `.bss` segment.  For example:

```
.section .bss

dataValues: .space (20 * 8)
```

would reserve space for 20 8-byte (64-bit) integer values.  Storage allocated in the `.bss` segment is guaranteed to be filled with zeroes.

If you want a challenge, allocate the arrays on the stack.  The frame pointer register (`%rbp`) can help you keep track of stack-allocated storage: see [Lecture 8](../lectures/lecture08-public.pdf).
