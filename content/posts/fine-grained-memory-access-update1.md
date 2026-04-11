+++
title = "Fine-Grained Access Semantics for Program Memory :: Update #1"
date = 2026-02-21
[taxonomies]
tags= ["thoughts", "language-design", "programming", "memory",
    "Fine-Grained Access Semantics for Program Memory"]
+++

When doing completely unrelated work, I was pointed in the direction of something called
*Capability Hardware Enhanced RISC* or [CHERI] which looks to add metadata to pointers
that control what instructions are able to operate on the data.

This is pretty cool piece of technology because it embeds it directly in the hardware of
CPUs, allowing already existing software to benefit from it however, it would still be of
value to have some control over it in a high level language like C, Zig or C++.

[CHERI]: https://en.wikipedia.org/wiki/Capability_Hardware_Enhanced_RISC_Instructions
