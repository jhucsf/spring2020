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

FIXME: figure out how to incorporate design and coding style into grading breakdown.

This is a challenging assignment.  Don't wait until the last minute to start it!  As usual, ask questions using Piazza, come to office hours, etc.

**Important**: When you write assembly code, you must *actually* write assembly code. Using the compiler to generate assembly code is *not* allowed.  See <a href="#step-4-postfix-calculator-in-assembly">Step 4</a> for more details.

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

Note: these steps aren't meant to be done strictly sequentially.  For example, Steps 1 and 2 can and should be done at the same time.  Steps 1–3 should probably be done before Steps 4 and 5, though.

## Step 1: Postfix calculator in C

The first task is to implement a C version of the postfix calculator.  The C version should take a single command line argument specifying a postfix expression, and (assuming the postfix expression is valid) print a single line of output of the form

> <pre>Result is: <i>N</i></pre>

where <code><i>N</i></code> is the result of evaluating the expression.

### Requirements and specifications

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

### Compiling and running the program

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

*Think about the C version as a template for the assembly version.*  The purpose of the C version of the program is to help you think about the problem and discover a good way to decompose it into functions.  When you write the assembly language version of the program, you can hand-translate each C function into an equivalent assembly language function.

*Make simplifying assumptions.* Since you're using the C version of the program as a template for the assembly version, you should try to write your C functions in a way that will be easy to duplicate in assembly language.  For example, use the `long` data type (64 bit signed integer) for all data values except for the characters in the expression string.  Use pointers as necessary to allow a called function to modify a variable whose address is passed as an argument.

Here is a suggested set of functions to write, which will be reasonably straightforward to translate to assembly language:

```c
long isSpace(long c);
long isDigit(long c);
const char *skipws(const char *s);
long tokenType(const char *s);
const char *consumeInt(const char *s, long *pVal);
void stackPush(long stack[], long *pCount, long val);
long stackPop(long stack[], long *pCount);
long eval(const char *expr);
```

Note that you are not required to implement these exact functions, but you may if you wish.

## Step 2: Unit tests for C postfix calculator

As you develop each function in the C postfix calculator program, write unit tests for it.  The source file **cTests.c** is a starting point for writing unit tests.  The unit test code will use the same unit testing framework as [Assignment 1](assign01.html).

To compile the C version of the unit tests, run the command <code class="cmd">make cTests</code>.  To run the tests, run the command

> <code class="cmd">./cTests</code>

### Hints for Step 2

Test thoroughly!  Try to exercise the corner cases in your code.

In addition to testing valid inputs, you should also test invalid inputs.  One challenge for testing invalid inputs is that the correct program behavior is exiting with an exit code of 1: however, you don't want the test program to *actually* exit when an invalid input is tested, since that would cause the test program to exit!  The file **cTests.c** has support for redirecting calls to the `exit` function so that they return control to your test function, rather than exiting the program.  Let's say, for example, that you want to test that a function called `eval` properly calls `exit` when given an invalid postfix expression.  The test function should set the `exitExpected` variable to a nonzero value, and then use `sigsetjmp` and `siglongjmp` as follows:

```c
expectedExit = 1; /* about to test code that is supposed to call exit */

if (sigsetjmp(exitBuf, 1) == 0) {
  eval("2 3 + 4");    /* invalid postfix expression */
  FATAL("eval function failed to exit for invalid expression");
} else {
  printf("Good, eval properly called exit for invalid expression...");
}
```

This approach works because **cTests.c** has its own version of the `exit` function which uses `siglongjmp` to transfer control back to the call to `sigsetjmp` but return with a nonzero return code.  The `sigsetjmp` and `siglongjmp` functions are essentially primitive forms of `try`/`catch` and `throw` (respectively).  You can read more about these functions in Chapter 8 of the textbook.  (Note that the `FATAL` macro, which causes a unit test to immediately fail, also takes advantage of `sigsetjmp`/`siglongjmp`.)

## Step 3: System-level tests

In addition to writing unit tests to test the individual functions in the C version of the postfix calculator, program, you should also write "system"-level tests to test the overall program on various inputs.  Add these tests to **sysTests.sh**.  This script supports two kinds of tests:

* `expect` runs the calculator program on a valid postfix expression and tests that the correct result is computed
* `expect_error` runs the calculator program on an invalid postfix expression and tests that an error message is printed

A few example tests are provided:

```bash
expect 5 '2 3 +'
expect 42 '6 7 *'
expect 42 '6 6 6 6 6 6 6 + + + + + +'
expect_error '2 2'
expect_error '1 *'
```

You should add your own tests.  As with the unit tests, try to think of corner cases in your code, and add tests to exercise them.

To run the system tests on your C postfix calculator implementation, run the command

> <code class="cmd">./sysTests.sh ./cPostfixCalc</code>

## Step 4: Postfix calculator in assembly

In this step, you will write an x86-64 assembly language version of the postfix calculator.  Use the source files **asmPostfixCalcMain.S** and **asmPostfixCalcFuncs.S** for the `main` function and implementation functions, respectively.  Follow the design of the C implementation you wrote in Step 2.

Note that the **.S** file extension means "preprocessed assembly", so you can use C-style comments in your assembly code.  You can also use `#define` to define named constants, just as you would in a C program.

To assemble and link the assembly language program, run the command <code class="cmd">make asmPostfixCalc</code>.  Run it exactly the same way as the C version, e.g.

> <code class="cmd">./asmPostfixCalc '1 2 +'</code>

### Writing assembly code

The purpose of this assignment is for you to learn how to write assembly code by hand.  So, using the compiler to compile C to assembly code, and then pretending you wrote the assembly code, is *not* a legitimate way to complete this assignment.

You may inspect compiler-generated assembly code as a learning tool, but you should do so sparingly, and you should *never* directly copy any compiler-generated code into your assembly source files.

### Hints for Step 4

This step is challenging!  Here are some hints to help you make progress.

*Start with the simplest functions.*  If you are implementing the functions suggested in Step 2, you might start by implementing the `isSpace` function, which should return 1 if its argument is space (`' '`) or tab (`'\t'`), and return 0 otherwise.  The `isDigit` function is also a good place to start.

*Write the unit tests as you implement your functions.* Your job will be *vastly* easier if you develop each function and its unit test(s) at the same time.  You will find that unit tests allow you to test and debug each function in isolation, and free you from having to worry unnecessarily about interactions with other functions.

Once you get all of the individual functions to work correctly, you can tie them together into a complete program.  See Step 5 below for more details on how to write unit tests for your assembly functions.

*Follow the calling conventions.* Make sure your assembly language functions correctly adhere to the x86-64 calling conventions.  By doing so,

* You will be able to call C library functions (such as `printf`) as needed
* You will be able to write your unit tests in C (i.e., you can make calls to your assembly language functions from the unit test code written in C)

Make sure that when your assembly code calls a function, the stack pointer (`%rsp`) contains an address that is a multiple of 16.  Because a call instruction pushes the current program counter value (`%rip`) onto the stack, on entry to a function, the stack pointer is misaligned by 8 bytes (the size of a code address), so each of your functions will need to adjust `%rsp` by *N* bytes such that *N* mod 16 = 8.  I.e., subtract 8 bytes, or 24 bytes, or 40 bytes, etc.

*Use registers for local variables.*  For most local variables in your code, you can use a register as the storage location.  The callee-saved registers (`%rbx`, `%rbp`, `%r12`–`%r16`) are the most straightforward to use, but you will need to save their previous contents and then restore them before returning from the function.  (The `pushq` and `popq` instructions make saving and restoring register contents easy.)  The caller-saved registers (`%r10` and `%r11`) don't need to be saved and restored, but their values aren't preserved across function calls, so they're tricker to use correctly.

*Use the frame pointer to keep track of local variables in memory.*  Some variables will need to be allocated in memory.  You can allocate such variables in the stack frame of the called function.  The frame pointer register (`%rbp`) can help you easily access these variables.  A typical setup for a function which allocates variables in the stack frame would be something like

```
myFunc:
    pushq %rbp                      /* save previous frame pointer */
    subq $N, %rsp                   /* reserve space for local variable(s) */
    movq %rsp, %rbp                 /* set up frame pointer */

    /*
     * implementation of function: local variables can be accessed
     * relative to %rbp, e.g. 0(%rbp), 8(%rbp), etc.
     */

    addq $N, %rsp                   /* deallocate space for local variable(s) */
    popq %rbp                       /* restore previous frame pointer */
    ret
```

The code above allocates *N* bytes of memory in the stack frame for local variables. Note that *N* needs to be a multiple of 16 to ensure correct stack pointer alignment.  (Think about it!)

*Use `leaq` to compute addresses of local variables.* It is likely that one or more of your functions takes a pointer to a variable as a parameter.  When calling such a function, the `leaq` instruction provides a very convenient way to compute the address of a variable.  For example, let's say we want to pass the address of a local variable 8 bytes offset from the frame pointer (`%rbp`) as the first argument to a function.  We could load the address of this variable into the `%rdi` register (used for the first function argument) using the instruction

```
leaq 8(%rbp), %rdi
```

## Step 5: Unit tests for assembly postfix calculator

In the source file **asmTests.c**, implement unit tests for the functions of the assembly language version of the postfix calculator.

You will need to add function prototypes for your assembly functions.  If your assembly language calculator implementation follows the design of the C version exactly, you can use the same function prototypes (i.e., you can copy them from **cPostfixCalc.h**).

Even better, if your assembly functions are equivalent to your C functions, *you can copy the actual tests*.  So, the unit tests for your assembly functions will likely be similar (or even identical) to the unit tests for your C functions.
