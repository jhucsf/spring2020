---
layout: default
title: "Lab 2: Postfix calculator"
---

Due: *TBD*

# Overview

In this lab, you will implement a postfix calculator program in x86-64 assembly language.

The grading breakdown is:

* C version of postfix calculator: 15%
* Assembly version of postfix calculator: 85%
* System tests (for both versions): 10%
* Unit tests for assembly language version: 10%

This is a challenging assignment.  Don't wait until the last minute to start it!  As usual, ask questions using Piazza, come to office hours, etc.

When you are done with this assignment you will have proved yourself capable of writing nontrivial x86-64 assembly code.  This is a foundational skill for hacking on operating systems and compilers, understanding security vulnerabilities such as buffer overflows, and generally being one with the machine.

# Postfix arithmetic

Normally when we express arithmetic we use *infix notation*, where binary operators (such as +, -, etc.) are placed between their operands.  For example, to expression the addition of the operands 2 and 3, we would write

> 2 + 3

In *postfix notation* the operator comes *after* the operands, so adding 2 and 3 would be written

> 2 3 +

Postfix notation has some advantages over infix notation: for example, parentheses are not necessary, since the order of operations is never ambiguous.  Postfix notation is also called [Reverse Polish notation](https://en.wikipedia.org/wiki/Reverse_Polish_notation).  It is famously used in [HP calculators](https://en.wikipedia.org/wiki/HP_calculators) and programming languages such as [Forth](https://en.wikipedia.org/wiki/Forth_(programming_language)) and [PostScript](https://en.wikipedia.org/wiki/PostScript).

Evaluating postfix expressions using a program is very simple.  The program maintains a stack of values.  The items (operands and operators) in the expression are processed in order.  Operands (e.g., literal values) are pushed onto the stack.  When an operator is encountered, its operands are popped from the stack, the operator is applied to the operands, and the result is pushed onto the stack.  For example, consider the postfix expression

> 3 4 5 + \*

This expression would be evaluated as follows:

Item | Action
---- | ------
3    | Push 3 onto stack
4    | Push 4 onto stack
5    | Push 5 onto stack
+    | Pop operands 5 and 4, add them, push sum 9
\*   | Pop operands 9 and 3, multiply them, push product 27

When any valid postfix expression is evaluated, the stack will have a single result value when the end of the expression is reached.  Also, valid postfix expressions will guarantee that operand values are always available on the stack when an operator is processed.  Here are some examples of *invalid* postfix expressions:

Expression | Why invalid
---------- | -----------
10 2 - *   | Only one operand value is on stack when operator \* is processed
2 3 + 4    | Two operand values are on stack when end of expression is reached

# Tasks

This section explains the specific tasks that you will need to complete.

## Postfix calculator in C

The first task is to implement a C version of the postfix calculator.  The C version should take a single command line argument specifying a postfix expression, and (assuming the postfix expression is valid) print a single line of output of the form

> <pre>Result is: <i>N</i></pre>

where <code><i>N</i></code> is the result of evaluating the expression.

Requirements and specifications:

* The expression will consist of positive integer literals and operators (+, -, \*, and /)
* All values should be represented using signed 64-bit integers (use the **long** C data type)
* All operators should be evaluated using the usual C semantics for operations on **long** values
* The operand stack is limited to 20 values
* If the program successfully evaluates the input expression, it should exit with a zero (0) exit code

If the expression is invalid, or if the maximum stack depth is exceeded, the program must print a single line of the form

> <pre>Error: <i>msg</i></pre>

where <code><i>msg</i></code> is a message describing the error.  The program should also exit immediately with a nonzero exit code (after printing the error message) if an error occurs.

## System-level tests

Yeah.

## Postfix calculator in assembly

Yeah.
