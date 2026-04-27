+++
title = "Lispy (C)"
description = "A simple interpreter for a Lisp-like language"
date = 2022-09-02
updated = 2026-04-27
draft = false

[taxonomies]
tags = ["c", "interpreters", "lisp"]

[extra]
#images = [""]
source = "https://codeberg.org/oraqlle/lispy"
+++

Lispy implementation in C.

Lispy is a small but robust programming language that inherits many features from Lisp.
Lispy comes with an REPL as well as an interpreter. Like other Lisps, Lispy uses
*macros-like* objects called Q-expressions to build out unevaluated code whose structure
and content can manipulated and later evaluated.

## Example

```clj
; Fibonacci (this is a comment)
; `()` denotes a S-expression which will evaluate immediately
; `{}` denotes a Q-expression which will remain unevaluated
; Define a function `fib` taking an parameter called `n`.
; `fun` takes two Q-expressions the first being the name and parameters,
; the second is the body of the function
(fun {fib n} {
    ; `select` expressions evaluate the first element (here a S-expression),
    ; returning the evaluated the second element if the first evaluates to true
    ; continues until a selection is made, `otherwise` is reached or no 
    ; selection is made which result in an `error`.
    select
        { (== n 0) 0 }
        { (== n 1) 1 }
        { otherwise (+ (fib (- n 1)) (fib (- n 2))) }
})
; Create list of numbers and apply `fib` to each element
; Yielding a new list
(print (map fib {0 1 2 3 4 5 6 7 8 9}))
```

## Running Files

```sh
$ ./build/dev/lispy_interpreter examples/hello.lpy
"hello world!"
```

## REPL

```sh
$ ./build/dev/lispy_interpreter
Lispy v0.3.1
Press Ctrl+D to exit.

Lispy>
```

## Credit

Lispy is built from the language created by Daniel Holden in his
[Build Your Own Lisp](https://www.buildyourownlisp.com/) Book/Blog. Most of the design of
this implementation reflects that of the source code in his implementation. If you are
interesting in creating your own programming language or even just learning C, I highly
recommend giving it a read.
