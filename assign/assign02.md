---
layout: default
title: "Assignment 2: Postfix calculator"
---

Due: *TBD*

# Overview

In this assignment, you will implement a postfix calculator program in x86-64 assembly language.

The grading breakdown is:

* C version of postfix calculator: 15%
* Assembly version of postfix calculator: 85%
* System tests (for both versions): 10%
* Unit tests for assembly language version: 10%

This is a challenging assignment.  Don't wait until the last minute to start it!  As usual, ask questions using Piazza, come to office hours, etc.

When you are done with this assignment you will have proved yourself capable of writing nontrivial x86-64 assembly code.  This is a foundational skill for hacking on operating systems and compilers, understanding security vulnerabilities such as buffer overflows, and generally becoming one with the machine.

# Postfix arithmetic

Normally when we express arithmetic we use *infix notation*, where binary operators (such as +, -, etc.) are placed between their operands.  For example, to expression the addition of the operands 2 and 3, we would write

> `2 + 3`

In *postfix notation* the operator comes *after* the operands, so adding 2 and 3 would be written

> `2 3 +`

Postfix notation has some advantages over infix notation: for example, parentheses are not necessary, since the order of operations is never ambiguous.  Postfix notation is also called [Reverse Polish notation](https://en.wikipedia.org/wiki/Reverse_Polish_notation).  It is famously used in [HP calculators](https://en.wikipedia.org/wiki/HP_calculators) and programming languages such as [Forth](https://en.wikipedia.org/wiki/Forth_(programming_language)) and [PostScript](https://en.wikipedia.org/wiki/PostScript).

Evaluating postfix expressions using a program is very simple.  The program maintains a stack of values.  The items (operands and operators) in the expression are processed in order.  Operands (e.g., literal values) are pushed onto the stack.  When an operator is encountered, its operand values are popped from the stack, the operator is applied to the operand values, and the result value is pushed onto the stack.  For example, consider the postfix expression

> `3 4 5 + *`

This expression would be evaluated as follows:

Item | Action
---- | ------
`3`    | Push 3 onto stack
`4`    | Push 4 onto stack
`5`    | Push 5 onto stack
`+`    | Pop operands 5 and 4, add them, push sum 9
`*`   | Pop operands 9 and 3, multiply them, push product 27

When any valid postfix expression is evaluated, the stack will have a single result value when the end of the expression is reached.  Also, valid postfix expressions will guarantee that operand values are always available on the stack when an operator is processed.  Here are some examples of *invalid* postfix expressions:

Expression | Why invalid
---------- | -----------
`10 2 - *`   | Only one operand value is on stack when operator \* is processed
`2 3 + 4`    | Two operand values are on stack when end of expression is reached

# Tasks

This section explains the specific tasks that you will need to complete.

Note: these steps aren't really meant to be done strictly sequentially.  For example, Steps 1 and 2 can and should be done at the same time.  Steps 1–3 should probably be done before Steps 4 and 5, though.

## Step 1: Postfix calculator in C

The first task is to implement a C version of the postfix calculator.  The C version should take a single command line argument specifying a postfix expression, and (assuming the postfix expression is valid) print a single line of output of the form

> <pre>Result is: <i>N</i></pre>

where <code><i>N</i></code> is the result of evaluating the expression.

Requirements and specifications:

* The expression will consist of positive integer literals and operators (+, -, \*, and /)
* A sequence of one or more space (`' '`) or tab (`'\t'`) characters acts as a token separator
* Operators will not necessarily be separated from other tokens by whitespace: for example, `2 3 4 5 +-*` is a valid expression which when evaluated yields the result `-12`
* Leading and/or trailing whitespace should be ignored
* All values should be represented using signed 64-bit integers (use the `long` C data type)
* All operators should be evaluated using the usual C semantics for operations on `long` values
* The operand stack is limited to 20 values
* If the program successfully evaluates the input expression, it should exit with a zero (0) exit code

If the expression is invalid, or if the maximum stack depth is exceeded, the program must print a single line of the form

> <pre>Error: <i>msg</i></pre>

where <code><i>msg</i></code> is a message describing the error.  The program should also exit immediately with an exit code of 1 if an error occurs (after printing the error message).

The source code for the C version of the program is in two files, **cPostfixCalcMain.c** and **cPostfixCalcFuncs.**.  There is also a header file **cPostfixCalc.h**, which you should use for definitions and function prototypes.  The **main** function should be in **cPostfixCalcMain.c**; all other functions should be in **cPostfixCalcFuncs.c**.

Running the command <code class="cmd">make cPostfixCalc</code> will compile the two source files and link them into an executable **cPostfixCalc**.  Here are some example invocations you can try from the command line:

Invocation | Expected output
---------- | ---------------
<code class="cmd">./cPostfixCalc '1 1 +'</code> | `Result is: 2`
<code class="cmd">./cPostfixCalc '3 4 5 + &#42;'</code> | `Result is: 27`
<code class="cmd">./cPostfixCalc '17 3 /'</code> | `Result is: 5`
<code class="cmd">./cPostfixCalc '3 10 /'</code> | `Result is: -7`
<code class="cmd">./cPostfixCalc '2 3 4 5 +-&#42;'</code> | `Result is: -12`
<code class="cmd">./cPostfixCalc '10 2 - &#42;'</code> | `Error: ...`
<code class="cmd">./cPostfixCalc '2 3 + 4'</code> | `Error: ...`

Note that in the cases where the postfix expression is invalid, the specific output text following `Error:` is not mandated — just have the program print something descriptive of the error.  For example, in the case of the invocation

> <code class="cmd">./cPostfixCalc '2 3 + 4'</code>

the full error message might be something like `Error: multiple values left on stack`.

### Hints for Step 1

*Think about the C version as a template for the assembly version.*  The purpose of the C version of the program is to help you think about the problem and discover a good way to decompose it into functions.  When you write the assembly language version of the program, you can just hand-translate each C function into an equivalent assembly language function.

*Make simplifying assumptions.* Since you're using the C version of the program as a template for the assembly version, you should try to write your C functions in a way that will be easy to duplicate in assembly language.  For example, use the `long` data type (64 bit signed integer) for all data values except for the characters in the expression string.  Use pointers as necessary to allow a called function to modify a variable whose address is passed as an argument.

## Step 2: Unit tests for C postfix calculator

As you develop each function in the C postfix calculator program, write unit tests for it.  The source file **cTests.c** is a starting point for writing unit tests.  The unit test code will use the same unit testing framework as [Assignment 1](assign01.html).

To compile the C version of the unit tests, run the command <code class="cmd">make cTests</code>.  To run the tests, run the command

> <code class="cmd">./cTests</code>

### Hints for Step 2

Test thoroughly!  Try to exercise the corner cases in your code.

In addition to testing valid inputs, you should also test invalid inputs.  One challenge for testing invalid inputs is that the correct program behavior is exiting with an exit code of 1: however, you don't want the test program to *actually* exit when an invalid input is tested, since that would exit the test program!  The file **cTests.c** has support for redirecting calls to the `exit` function so that they return control to your test function, rather than exiting the program.  Let's say, for example, that you want to test that a function called `eval` properly calls `exit` when given an invalid postfix expression.  The test function should set the `exitExpected` variable to a nonzero value, and then use `sigsetjmp` and `siglongjmp` as follows:

```c
expectedExit = 1; /* about to test code that is supposed to call exit */

if (sigsetjmp(fatalBuf, 1) == 0) {
  eval("2 3 + 4");    /* invalid postfix expression */
  FATAL("eval function failed to exit for invalid expression");
} else {
  printf("Good, eval properly called exit for invalid expression");
}
```

This approach works because **cTests.c** has its own version of the `exit` function which uses `siglongjmp` to transfer control back to the call to `sigsetjmp` but return with a nonzero return code.  The `sigsetjmp` and `siglongjmp` functions are essentially primitive forms of `try`/`catch` and `throw` (respectively).  You can read more about these functions in Chapter 8 of the textbook.)  (Note that the `FATAL` macro, which causes a unit test to immediately fail, also takes advantage of `sigsetjmp`/`siglongjmp`.

## Step 3: System-level tests

Yeah.

## Postfix calculator in assembly

Yeah.
