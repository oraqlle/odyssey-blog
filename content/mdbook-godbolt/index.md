+++
title = "mdbook-godbolt"
description = "Runnable codeblocks in mdbook via godbolt.org"
date = 2025-11-12
updated = 2026-07-01
draft = false

[taxonomies]
tags = ["markdown", "mdbook", "javascript", "rust", "book"]

[extra]
images = ["example-clear.png"]
status = "Complete"
source = "https://codeberg.org/oraqlle/mdbook-godbolt"
+++

A preprocessor for mdbook to add runnable code snippets via
[godbolt.org](https://godbolt.org), similar to Rust *playground* snippets. `v0.1.1`
published on [crates.io](https://crates.io/crates/mdbook-godbolt).

## Usage

You can make a code snippet executable by adding the `godbolt` *attribute* to the opening
code fence. This will wrap the resulting HTML element in a `<div>` with the `godbolt`
class. There must be a language specified, both for godbolt and so your codeblock gets
syntax highlighting from the main book processor.

Adding `godbolt` does not do anything to make the code runnable as you at a minimun need
to specify the compiler code. This is done by specify the `godbolt-compiler:<code>`
attribute. There is also the `godbolt-flags:<flags>` attribute (optional) which allows
you to specify a string of flags in any format you need up until the next comma or the
end of the line.

Every attribute must be seperated by a comma with no spaces. A lookup table for compiler
codes can be found below.

The entire codeblock will be submitted to to the [godbolt API](https://github.com/compiler-explorer/compiler-explorer/blob/main/docs/API.md)
as it appears in the markdown source (bar line-snipping designators if active). The other
features such as code copy, line-snipping etc. from mdBook are available as only wrapper
HTML elements are added and code fences are stripped of the godbolt metadata before being
passed back to the regular processor.

### Example

````markdown
```cpp,godbolt,godbolt-compiler:g151,godbolt-flags:-std=c++17
#include <cmath>
#include <iostream>

auto magnitude(auto const x, auto const y, auto const z) -> double {
    return std::sqrt(x * x + y * y + z * z);
}

auto main() -> int {
    auto const x = 2.;
    auto const y = 3.;
    auto const z = 5.;

    std::cout << "The magnitude of the vector is "
              << magnitude(x, y, z)
              << "units.\n";

    return 0;
}
```
````

Here is what the example renders as.

{{ image(src="example-marked-up.png", alt="Example mdbook with new 'run in godbolt' button") }}
