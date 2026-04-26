+++
title = "Sapling Game Engine :: Devlog #5"
date = 2026-03-18
[taxonomies]
tags= ["game-dev", "graphics", "memory", "Sapling Game Engine Devlogs"]
+++

> Completed writing the resource handler types but quickly noticed the inconsistencies in
> the example code and between code snippets. Resource types are also using dynamic
> polymorphism which might not scale well so I want to explore using Curiously Recurring
> Template Pattern (CRTP) to enable static polymorphism.
>
> I am considering how to distribute subsystems to best suit the need of downstream games.
> Current considerations are either;
>
> * Distribute modules of a subsystem as standalone libraries (shared by default), with
>   optional modules not being built, bundled, linked etc.
> * Distribute subsystems as libraries and only build in optional dependencies if
>   requested. This makes it easier to add or remove optional subsystems.
>
> Currently using the module as libraries approach with the naming convention of 
> sapling-<subsystem>-<module>.
