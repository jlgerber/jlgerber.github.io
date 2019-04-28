# One Vim to Rule Them All
How to use a single vim instance? 

# Buffers
Buffers may be tied to files, but they don't have to be. 
Wnen you list buffers (`:ls`) you get a table with the following columns:
- buffer number
- buffer code (see `:h ls` for full detail)
  - `%` buffer in current window
  - `#` The alternate buffer for `:e #` and CTRL-^
  - `a` active buffer. loaded and visible
  - `h` hidden buffer. loaded but not displayed in a window
  - `-` buffer with modifiable off
  - `_` a readonly buffer
  - `?` a terminal buffer without a job
  - `R` a terminal buffer with a running job
  - `F` a terminal buffer with a finished job
  - `+` a modified buffer
  - `x` a buffer with read errors
- buffer name
- current active line of buffer

## Buffer Commands
Command | Effect
--- | ---
:ls | list buffers
:bn or :bnext | advance to the next buffer
:bf or :bfirst | rewind to the first buffer
:bp :bprevious | activate previous buffer

# Dealing with Multiple Files

When you launch vim, you can supply it with a list of files, which will end up in its  `args` list. 

## Args List Commands

Command | Effect
--- | ---
:n | go to the next file
:rewind | go back to the start of the args list
:ar | print the files in the args list with the current file in [brackets]
:wall | write out all the files in the arg list to disk, saving any changes

## Bufdo
Perform a command or series of commands on all of the buffers

## Match text in all buffers buffers 
You can use the matching function `g` in combination with `bufdo` to achieve some powerful effects. Here is an example where we 
delete `" hithere` from every line of every buffer in vi.
```
:bufdo g/"\s*hithere/d
```
