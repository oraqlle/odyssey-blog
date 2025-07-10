+++
title = "Cortex Library"
description = "A library of general purpose types, classes, functions, algorithms, utilities and other components for C++."
date = 2023-03-26T14:12:00+00:00
updated = 2023-06-22T16:00:00+00:00
draft = false
sort_by = "title"
template = "docs/page.html"

[extra]
lead = "A library of general purpose types, classes, functions, algorithms, utilities and other components for C++."
toc = true
top = false
+++

<a href="CODE_OF_CONDUCT.md"><img src="https://img.shields.io/badge/Contributor%20Covenant-2.1-4baaaa.svg" alt="Contributor Covenant"></a>
<a href="LICENSE"><img src="https://img.shields.io/github/license/oraqlle/cortexlib" alt="License"></a>
<img src="https://img.shields.io/github/v/release/oraqlle/cortexlib?include_prereleases" alt="Current Release">

## Development Status

Cortex is pretty unstable and currently undergoes regular changes. It also has limited testing meaning that meaning it could break. There is no promise for long-term support and may evolve without regard to backwards compatibility. That being said I do plan to stabilise the library more with time.

- [GitHub](https://github.com/oraqlle/cortelib)
- [Docs](https://oraqlle.github.io/cortexlib/)

## Features

Cortex is a library with a bunch of random components from different data structures and algorithms to concurrency tools and general utilities. Everything in Cortex is kept within the `cxl` namespace and is broken down into three separate sub-libraries. These being:

- **containers/. . .**
- **iterators/. . .**
- **utilities/. . .**

### Containers

Cortex features a few unique, general purpose container types designed to compliment the existing C++ standard library containers and algorithms.

#### List of Containers

- `cxl::matrix` - Generic, owning 2D array.
- `cxl::tensor` - Generic, owning multidimensional array (in dev).

### Iterators

Iterators library is designed to compliment the containers library and offer different traversal patterns across various data structures.

#### List of Iterators

- `cxl::normal_iterator` - A iterator that adapts non-object iterators (pointers) into object iterators while keeping the original entities semantics.

### Utilities

Cortex's utilities library contains various general purpose utility objects and types.

#### List of Utilities

- `cxl::utils::match` - A visitor object type allowing for conditional access to a `std::variant`.
- `cxl::utils::match_any` - A type which acts as the base fallthrough match for a match expression.

