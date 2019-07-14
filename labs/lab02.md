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
