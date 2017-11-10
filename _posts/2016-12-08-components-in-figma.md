---
layout: post
title: "Components in Figma"
description: "By bringing concepts like composition, inheritance and unlimited overrides from engineering to design, Components move Figma closer to a world where we are able to easily reason about design systems as we go about our day to day work."
---



If you're reading this chances are you're already familiar with WASM to some extent. If you aren't, this would be a good time to check out [webassembly.org](http://webassembly.org/). At the time of publishing this article, WebAssembly has just reached the [Browser Preview milestone](http://webassembly.org/roadmap/), meaning that version 1 of WebAssembly will very likely be what the current draft describes. The specifics of this article are for version mvp-13.

There are
[some](https://users.rust-lang.org/t/compiling-to-the-web-with-rust-and-emscripten/7627)
[existing](http://llvm.org/docs/doxygen/html/WebAssembly_8h.html)
[compilers](http://webassembly.org/getting-started/developers-guide/)

The <a name="element_section">**element section**</a> allows a module to initialize the contents of a table imported from the outside or defined in the [table section](#table_section).

The <a name="code_section">**code section**</a> is probably the bulk of most WebAssembly modules as it defines all code for all functions in the module. The function bodies are defined in the same order as their respective function indexes in the [function section](#function_section), not including imports of course. Our "half" function's body could be defined like this:
