+++
title = "Sapling Game Engine :: Devlog #4"
date = 2026-03-11
[taxonomies]
tags= ["game-dev", "graphics", "memory", "Sapling Game Engine Devlogs"]
+++

Started organising the build process of the project to be modular while implemented the
baseline resource handler classes proposed by the Vulkan tutorial. This involved
massively changing the CMake scripts but in the end allowed for the specification of
different subsystems and separate modules of a given subsystem to be encoded directly
in the build process and also create different libraries that can be distributed
throughout the project and downstream games using the engine.

I greatly appreciate the simplicity of the proposed resource handler classes and makes
the later introduction of Vulkan specific types etc. more approachable and is much
better for understanding how Vulkan objects are used in a bigger application (at least
if feels this way so far).
