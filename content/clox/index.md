+++
title = "Lox Interpreter (C)"
description = "Bytecode Interpreter for Lox Programming Language"
date = 2025-03-20
updated = 2026-07-01
draft = false

[taxonomies]
tags = ["c", "interpreters", "parsing", "lox", "language design"]

[extra]
#images = [""]
status = "Complete"
source = "https://codeberg.org/oraqlle/clox/src/branch/uni/reads-native"
+++

Interpreter for a simple, Python-like language called Lox.

This variant of the Lox interpreter compiles a Lox program into a custom bytecode
[Intermediate Representation (IR)](https://en.wikipedia.org/wiki/Intermediate_representation)
before executing it directly. Lox bytecode is parsed using a specific
[Operator Precedence Parser](https://en.wikipedia.org/wiki/Operator-precedence_parser)
known as a *Pratt Parser*, ensuring rapid parsing and thus execution of bytecode tokens.

The interpreter is written entirely in C, featuring custom data structures; such as a hash
table structure, a dynamic array structure, a *mark-and-sweep* garbage collector, has its
own custom virtual machine and the ability to pack of Lox data type objects in a single
64-bit underlying variant type by exploiting the nature of IEEE-754 double precision
floating-point numbers.

I have also extended the original implementation to allow user input via a builtin
`reads()` command.

## Credit

Implementation of Bob Nystrom's Lox language in Java following his book
[Crafting Interpreters](https://craftinginterpreters.com/)
