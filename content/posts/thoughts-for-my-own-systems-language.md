+++
title = "Thoughts for my own Systems Language"
date = 2020-12-14
updated = 2025-11-11
[taxonomies]
tags= ["thoughts", "language-design"]
+++

> **2026-04-11 - Note:**
>
> I have taken these notes and ideas from stuff I wrote in 2020/21. It is here for
> archival purposes and because the idea is not abandoned (I've been quite busy at these
> last couple years) but the concepts from the language need heavy revision from my end
> after many years of deliberation in my head.

## Preamble

I've been learning and writing C++ for a bit over a year and there have been so many
amazing things I've learnt about the language in recent weeks. The level of control you
can have over a program while being able to still abstract stuff in clever OOP systems is
truly unique experience, especially compared to programming in Python.

And I finally get templates! It is one of the coolest things I seen in a programming
language. I thought they were utterly useless before cause they never really worked when
I tried to use them myself.

There are some things about the language I don't like. There is no package manager which
makes it really hard to use external code. I spent multiple days a while back trying to
understand how to get a package called pdcurses working for a project and didn't
understand I need to be build it myself and then include it because, well I didn't know
about `make` or whatever I used on my Windows device, I think it was Visual Studio 2019.

Other then that, pointers are tricky to get right and I keep forgetting `delete` when I
use `new`. All this to say, I kinda wanna make my own language.

## Kraken

Don't know why I like this name. Probably mostly because it is not used, it is easy to
remember and I like the look of the extension `*.kr`.

The main idea is to have a language a lot like C++ but focused on good and simple
defaults and offers many building blocks over complete features.

### Necessary Features

* Structures
* Templates
* Custom operators
* Strongly typed
* Static types
* Closures/lambdas
* Threading support
* Atomic data types
* Basic data structures
* Modules/packages
* Basic algorithms lib

## Update :: 2022-03-12

I've started learning Rust and I love the idea of its Enums. I know C++ got
`std::optional<T>` which is great but its very hard to build your own Enums like Rust. I
don't like using `std::varient<Ts...>` very much.

* Tagged Unions
* Option
* Result

## Update :: 2024-10-04

* Monoidal IO (like Haskell)
* Functional programming

## Update :: 2025-10-11

* Fine-grained access control for data like a filesystem[^1][^2]

## Update :: 2026-04-11

* 

[^1]: https://en.wikipedia.org/wiki/Capability_Hardware_Enhanced_RISC_Instructions
[^2]: https://a-programming-odyssey.netlify.app/posts/fine-grained-memory-access
