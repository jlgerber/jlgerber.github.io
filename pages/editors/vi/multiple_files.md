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
