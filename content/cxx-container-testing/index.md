+++
title = "Testing C++ Standard Library Containers"
description = ""
date = 2023-05-05
updated = 2026-07-01
draft = false

[taxonomies]
tags = ["cpp", "go", "plotting", "cpp-standard", "cpp-standard-library", "testing", "benchmarks"]

[extra]
images = ["TrivialSmallType.png", "TrivialMediumType.png", "TrivialHugeType.png", "TrivialMonsterType.png"]
status = "Complete"
source = "https://codeberg.org/oraqlle/cxx-container-testing"
+++

A simple program that runs a series of tests on C++ standard containers. The tests are
not designed to be rigorous but rather simple experimentation of the relative performance
of the standard C++ containers.

## Tests

Benchmark tests the following operations on the below containers with element types
varying in size and features.

### Operations

- Push Back
- Push Front
- Linear Search
- Random Insert w/ Linear Search
- Random Erase
- Random Remove + Erase
- Destruction
- Sort
- Random Sorted Insert

### Containers

- [`std::list`](https://en.cppreference.com/cpp/container/list)
- [`std::deque`](https://en.cppreference.com/cpp/container/deque)
- [`std::vector`](https://en.cppreference.com/cpp/container/vector)
- Preallocated `std::vector`

### Element Types

- Trivial [Sized Types]
  - 8 bytes
  - 64 bytes
  - 128 bytes
  - 1024 bytes
  - 4096 bytes
- Non Trivial and Movable
- Non Trivial, Movable and `noexcept`
- Non Trivial and Non Movable
- Non Trivial and Expensive to Copy and Move [32 bytes]

## Results

Some of the most interesting results from the benchmark. It showed that for smaller
element sizes, operations typically attested to perform better with a linked list
(`std::list`); like random insertion, actually performed better with a contiguous array
(`std::vector`). It was only once the element size got really big did the linked-list
perform significantly better. This most likely can be attributed to how modern CPUs cache
data based on temporal and spatial locality which `std::vector` is able to "leverage" due
to the fact its elements are close together making subsequent lookup faster due to
higher likelihood of already being cached as opposed to the detached nature of
`std::list` nodes. However, these benchmarks just ran on my machine and might only be
indicative of the particular CPU my device had.

> Note: I apologize for the inconsistent colour convention of each plot.

*Trivial 8 byte type comparison*
{{ image(src="TrivialSmallType.png", alt="Trivial 8 byte type comparison") }}

*Trivial 64 byte type comparison*
{{ image(src="TrivialMediumType.png", alt="Trivial 64 byte type comparison") }}

*Trivial 1024 byte type comparison*
{{ image(src="TrivialHugeType.png", alt="Trivial 1024 byte type comparison") }}

*Trivial 4096 byte type comparison*
{{ image(src="TrivialMonsterType.png", alt="Trivial 4096 byte type comparison") }}

<!-- Another interesting thing to note is how much of an all-rounder `std::deque` can be which
can support many benefits from both `std::list` and `std::vector` in a single container.
-->

Full directory of plots available with [source](https://codeberg.org/oraqlle/cxx-container-testing/src/branch/master/imgs)
to browse.

## Credit

Inspired by [C++ benchmark - std::vector VS std::list VS std::deque - Baptiste Wicht](https://baptiste-wicht.com/posts/2012/12/cpp-benchmark-vector-list-deque.html)
