+++
title = "Fine-Grained Access Semantics for Program Memory"
date = 2025-10-11
[taxonomies]
tags= ["thoughts", "language-design", "programming", "memory",
"Fine-Grained Access Semantics for Program Memory"]
+++

I was reading [thread #12283] on <ziggit.dev> earlier this week which was discussing the
idea of a traits-like system; akin to what Rust has, in Zig such that you can
programaticlly add methods and thus capabilities to structs.

The discussion itself was not what drew my attention but the tl;dr was that reaching into
the "guts" of structs is the expected practice of Zig programs as to allow you absolute
control over the entirety of the program.

No, what caught my attention was a [remark from floooh] describing a concept for
memory/data access control that any person who has used a UNIX-like machine is familiar
with but is not used in any programming language itself.

Why not used fine-grained filesystem-like semantics for controlling access and
permissions to data instead of the public/private (/protected) model which can be too
simplistic when precise control is required.

What would this even look like?

Would pointers have something like the three octets (3-bits each) UNIX filesystems[^1]
have to indicate different permissions. An imperfect mapping could have the *owner* octet
represent the current scope, the *group* being the containing module's or class's
permissions and the final octet of *other* being all other scope's permissions ie. other
classes/structs, modules, functions etc.. I'm not sure what the `x - execute` bit would
refer to, maybe whether the object can be used as an *callable*/*invokable*, with object
not of a *callable*/*invokable* type never having `x` set but those that are of the
correct type can have which scopes can invoke them be controlled more precisely.

While this mapping could work in theory... I think... there would need to be a
consideration of whether this is only *"hints"* for the compiler, with it verifying the
validity of these permissions during compilation but stripping them out in the final
binary or whether this data is able to be queried at runtime and thus has its own data
attached to this.

If it is the latter then begs the question of where and how to store this data to prevent
it from being corrupted itself from programmer errors, system crashes or security
vulnerabilities to ensure that permissions are upheld. There is some mechanisms that
exist in hardware for verifying pointer validity by means of "tagging" a pointer[^2].
This is enabled because most pointers only use the first 48-bits (by convention only)
thus freeing some bits for a pointer tag. Maybe this could store the necessary permission
bits can key the metadata a part of existing pipelines and not introduce larger data
loading for every pointer used in a program.

However, the former means that there is not ability for any kind of *"reflection"* system
from within a program to change its behaviour based on the available permissions, which
could provide solutions to potential really hard to solve problems unavailable otherwise
but could be queried at compile time by systems like C++ templates or Zig comptime.

* [Update #1](/posts/fine-grained-memory-access-update1)

[thread #12283]: https://ziggit.dev/t/can-we-simulate-rust-like-traits-in-zig-to-add-methods-to-a-struct/12283
[remark from floooh]: https://ziggit.dev/t/can-we-simulate-rust-like-traits-in-zig-to-add-methods-to-a-struct/12283/8?u=oraqlle

[^1]: https://en.wikipedia.org/wiki/File-system_permissions#UNIX_and_Unix-like_systems
[^2]: https://en.wikipedia.org/wiki/Tagged_pointer
