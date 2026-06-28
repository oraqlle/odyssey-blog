+++
title = "ymalloc"
description = "Custom malloc function using free-list technique"
date = 2025-11-27
updated = 2026-06-28
draft = false

[taxonomies]
tags = ["c", "posix", "memory", "library"]

[extra]
images = ["free-list-example.png"]
source = "https://codeberg.org/oraqlle/ymalloc"
+++

A very simple implementation of a malloc-like function in C using POSIX `sbrk()` and a
free list. Mostly built to briefly explore how memory allocations work.

{{ image(src="free-list-example.png", alt="Layout of Freelist in Heap for ymalloc") }}
*Layout of Freelist in Heap for ymalloc*

## Credit

Credit to Marwan Burelle for the implementation details:
[A Malloc Tutorial](https://wiki-prog.infoprepa.epita.fr/images/0/04/Malloc_tutorial.pdf).
