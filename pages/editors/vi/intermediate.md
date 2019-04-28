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

## Switching Buffers
You can switch to a different buffer by typing `:b NUMBER` or ``:b NAME` (or `:buffer` but who likes to type more than they have to ?). You can use tab completion if you are attempting to switch by name.
## Deleting Buffers
You can delete a buffer using the `:bd[elete]` command (ie `:bd` or `:bdelete`). Without any arguments, it will
delete the current buffer. However, you can supply it with a number or name.

## Buffer Commands
Command | Effect
--- | ---
`:ls` | list buffers
`:ls!` | list all buffers, including unlisted buffers (help buffers are, for example, unlisted buffers)
`:bn` or `:bnext` | advance to the next buffer
`:bf` or `:bfirst` | rewind to the first buffer
`:bp` `:bprevious` | activate previous buffer
`:b NUMBER` | switch to the buffer with the number NUMBER (see `:ls`)
`:b NAME` | switch to the buffer with the supplied name (tab completion works here)
`:b#` | switch to the alternate buffer. (again see `:ls`)
`:bd NUMBER [NUMBER...]` | delete one or more buffers by number (as long as they have not been modified)
`:bd NAME [NAME...]` | delete one or more buffers by name (as long as they have not been modified)
`#s,#ebd` | delete a range of buffers (eg `1,5bd` deletes 1 through 5) (as long as they have not been modified)
`%db` | delete all the buffers (as long as they have not been modified)

**Note:** Add a `!` to the end of your `bd` command to force delete buffers that have modifications

# Dealing with Multiple Files

When you launch vim, you can supply it with a list of files, which will end up in its  `args` list. 

## Args List Commands

Command | Effect
--- | ---
`:n` | go to the next file
`:rewind` | go back to the start of the args list
`:ar` | print the files in the args list with the current file in [brackets]
`:wall` | write out all the files in the arg list to disk, saving any changes

## Bufdo
Perform a command or series of commands on all of the buffers

## Match text in all buffers buffers 
You can use the matching function `g` in combination with `bufdo` to achieve some powerful effects. Here is an example where we 
delete `" hithere` from every line of every buffer in vi.
```
:bufdo g/"\s*hithere/d
```
