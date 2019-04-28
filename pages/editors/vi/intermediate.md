---
title: intermediate
layout: page
---

[return to editors](../../editors.html)

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

**Note:** Add a `!` to the end of your various `bd` commands to force delete buffers that have modifications

## Splitting Windows

Command | Effect
--- | ---
:sp [buffername] | split horizontally. If no buffername is supplied, the current buffer will be split
:vs [filename] | split vertically. if no filename is supplied, then the currenct buffer will be split
CTRL-w h j k l | switch to a different buffer dictated by the direction (h j k or l)
:q | close the currently active split
[NUM] CTRL-w _ | Set the height to NUM or Max out height of current split if NUM not supplied
[NUM] CTRL-w \| | Set the width to NUM or Max out width of current split if NUM not supplied
CTRL-w + | Grow Height by 1 (can be prefixed by count)
CTRL-w - | Shrink height by 1 (can be prefixed by count)
CTRL-w > | Shift width right by 1 (can be prefixed by count)
CTRL-w < | Shift width left by 1 (can be prefixed by count)
CTRL-w = | Normalize size of all splits
CTRL-w r | Swap Top / Bottom or Left / Right splits
CTRL-w T | Break out current  window into new tab
CTRL-w o | Close every window in the current tabview except this one

## Tabs

Command | Effect
--- | ---
:tabn | go to the next tab
:tabp | go to the previous tab
:tabfirst | go to the first tab
:tablast | go to the last tab

#### NOTE: If you use split windows a lot, then you may want to remap your keys to make bouncing between windows
a single command affair. Here is an example configuration you can put in your .vimrc.

```
nnoremap <C-J> <C-W><C-J> " Ctrl-j to move down a split
nnoremap <C-K> <C-W><C-K> " Ctrl-k to move up a split
nnoremap <C-H> <C-W><C-H> " Ctrl-h to move left a split
nnoremap <C-L> <C-W><C-L> " Ctrl-l to move right a split
```
 
## Registers
You can store data in named registers. By default, Vim uses an unnamed register when saving text. For instance, when you yank text, it goes into an unnamed register, and when you paste text, it comes from an unnamed register, unless you name it. And how do you do that? Prefix the relevant command with `"`.
For example, to yank the current word into register `a`, do `"ayw`
The unnamed register may be addressed explicitly as `"`. So to select it you would type `""`.

### The Yank Register
When we use `y{motion}`, the text is placed, not only in the unnamed register, but in the `yank register` as well. The yank register is named `"0`. The yank register is more reliable than the unnamed register when it comes to yank and paste.

### The Black Hole Register
You can send data into the void, never to return, by specifying the `"_` register. This is Vim's /dev/null.

### System Clipboard Register
Vim's plus register (`"+`) references the system clipboard. If we capture text externally into the system clipboard, we can paste it via the `"+p` command (or `CTRL-r +` when in insert mode). 

We can send text to the system clipboard by prefixing the appropriate command with `"+`. For example, to capture the current word and send it to the system clipboard, type `"+yw`.

In X11 there is a second clipboard, called the primary cliboard. It stores middle mouse highlighted selections. We can reference it using `"*`. In Windows and Mac, there is no Primary clipboard, and `+` and `*` may be used interchangably.

### The Expression Register
registers can be thought of simply as containers that hold a block of text. The expression register, referenced by the = symbol ( quote= ⓘ ), is an exception. When we fetch the contents of the expression register, Vim drops into Command-Line mode, showing an = prompt. We can enter a Vim script expression and then press <CR> to execute it. If the expression returns a string (or a value that can be easily coerced into a string), then Vim uses it. (from Practical Vim: Edit Text at the speed of Thought)

### Read Only Registers
Vim has a number of read only registers that it sets for us. They are set implicitly. Here is a list

Register | Description
--- | ---
`"%` | Name of the current file
`"#` | Name of the alternate file
`".` | Last inserted text
`":` | Last Ex command
`"/` | Last search command

### Replace a Visual Selection with a Register
When text is selected in Visual Mode, using `p` replaces the selection with the contents of the specified register (or the implicit register if none is specified).

### Viewing Register Contents
You can look at an individual register by typing `:reg [NAME]`. Eg `:reg"0`. You can look at all of the registers by typing `:reg`.

## Loading Multiple Files From the Command Line

When you launch vim, you can supply it with a list of files, which will end up in its  `args` list. The arg list
is like the buffer list, except that it doesnt support random access. It is effectively a linked list that you
may go back to the head of at any time.

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
