+++
title = "Learning Programming from Slides Sucks"
date = 2026-07-02
[taxonomies]
tags= ["thoughts", "learning"]
+++

In my first year of university studying software engineering found it exceptionally
difficult to learn programming concepts from slides. Not only was it using rich text
format; which can be subtly problematic when pasting code or commands that expect pure
ASCII characters, but it is also formatted in the most abysmal manner possible (no
monospace, weird indentation etc.) making it hard to follow along in; idk, a whitespace
pedantic language like Python.

Given this was the status quo, students also showcased code in slide decks when teaching
in extra curricular circles. In the student team I joined (Monash DeepNeuron), the
usage of a `.` for current directory tripped me and a lot of my peers up when refering to
the training slides.

Given this I wanted to find a better approach. I had learnt officially what markdown was
and that GitHub rendered it in; an at least legible manner, so  when given the chance to
teach my peers some C++ I developed it in markdown and hosted on GitHub.

Mid-way through teaching I started learning Rust from '*the book*' which was built using
a tool called mdbook. I quickly took my hobble of markdown files and ported them to a
markdown build (thank god, now I could test how it displayed/rendered locally).

This was approach was much more receptive from my peers I was training so the team wanted
me to lead an effort to port the baseline HPC training to a online book format. I was
given some team members to help develop the content.

This book aimed to teach some basic C, parallel and distributed computing and how to use
the research cluster available to the student team. This book was very successful and
gave new recruits a concrete resource to refer back to whenever they got stuck so I was
very happy with the outcome. Since then it has been further developed by other HPC
training leads of MDN to include OS and cloud computing concepts which is a testament to
the open-content approach the book allows.

Anyone, just wanted to yap. I think mdbook is pretty cool.
