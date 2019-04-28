---
title: patterns
layout: page
---

# Patterns
This section is devoted to Vim pattern matching. 

## Case Sensitivity
We can disable case sensitivity for a pattern match using `:set ignorecase`. This also influences keyword autocompletion, so beware. We can override Vim's case sensitivity on a per search basis using `\c` and `\C`. `\c` tacked on the end of a pattern match forces Vim to ignore case. Whereas, `\C` forces Vim to be case sensitive. They each have their use depending upon the current state of Vim.

Vim has a setting called **smartcase** which attempts to predict our intentions. When active, Vim will treat searches as case insensitive unless a capital letter appears in the pattern; then it treats the search as case sensitive. You can set smartcase using `:set smartcase`. Note that both **smartcase** and **ignorecase** may be active. How they interact may be summed up in the following table

Pattern | **ignorecase** | **smartcase** | Matches
--- | --- | --- | ---
foo | off | - | foo
foo | on | - | foo Foo FOO
foo | on | on | foo Foo FOO
Foo | on | on | Foo
Foo | on | off | foo Foo FOO
\cfoo | - | - | foo Foo FOO
foo\C | - | - | foo


