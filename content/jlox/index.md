+++
title = "Lox Interpreter (Java)"
description = "Tree Walking Interpreter for Lox Programming Language"
date = 2024-07-08
updated = 2026-07-01
draft = false

[taxonomies]
tags = ["java", "interpreters", "parsing", "lox", "language design"]

[extra]
#images = [""]
status = "Complete"
source = "https://codeberg.org/oraqlle/jlox"
+++

Interpreter for a simple, Python-like language called Lox.

This interpreter parses programs by generating a Abstract-Sytntax-Tree (AST) of the Lox
program, maintaining the relationship between syntax objects. The program is run by
descending the tree and evaluating each leaf node to execute the correct underlying
behaviour meaning this interpreter falls in the category of *tree-walking-interpreters*.

This interpreter heavily utilises the [Java Virtual Machine (JVM)](https://en.wikipedia.org/wiki/Java_virtual_machine)
which is itself a *Bytecode intepreter*.

## Credit

Implementation of Bob Nystrom's Lox language in Java following his book
[Crafting Interpreters](https://craftinginterpreters.com/)
