+++
title = "Python Voxel Engine"
description = "A simple voxel Minecraft engine"
date = 2025-06-07
updated = 2026-06-29
draft = false

[taxonomies]
tags = ["python", "glsl", "opengl", "games", "graphics", "pygame", "moderngl-py"]

[extra]
images = ["view-on-ground.png", "view-from-sky.png", "wandering.png", "cave.png"]
status = "Complete"
source = "https://codeberg.org/oraqlle/py-voxel-engine"
+++

A simple voxel rendering engine written in Pygame and ModernGL Python libraries used to
make Minecraft like terrian and biomes. Generates terrain using a perling noise height
map and features full Ambient Occlusion, face culling and render chunking. Player can fly
around the world and can break and place blocks.

{{ image(src="view-on-ground.png", alt="View on the ground") }}

{{ image(src="view-from-sky.png", alt="View from the sky") }}

{{ image(src="wandering.png", alt="Wandering through the wilderness") }}

{{ image(src="cave.png", alt="Exploring caves") }}

## Credit

This engine was created following a [tutorial](https://www.youtube.com/watch?v=Ab8TOSFfNp4)
from Coder Space. Documentation is based on the techniques demonstrated in the video, for
my own record and for others further understanding. This project is licensed the same as
the source created by the original author.
