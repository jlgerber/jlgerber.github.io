# vi
## Basic navigation

Key | Effect
--- | ---
h | move left one character
l | move right one character
j | move down one character
k | move up one character

### Advanced Navigation

Key | Effect
--- | ---
w | move to start of next word
e | move to end of current word
$ | move to end of line
0 | move to start of line
^ | move to first character of first word on line

### Deleting

Key | Effect
--- | ---
x | delete character under cursor
dw | delete from the cursor to the start the next word
de | delete from the cursor to the end of the current word
d$ | delete fro the cursor to the end of the line
dd | delete the current line

## Motions
Many commands consist of a operator, and a motion.
In the previous section on deleting, the `d` key is the operator, and the `w`,`e` and `$` are motion keys. The motion keys by themselves produce motion, which is why they are called motion keys. Motion keys may also be prefixed by a `count`, which executes the motion key `count` times. 

For example `2w` moves forward 2 words. You can insert a `count` before a `motion` to modify the effect. 

```sh
# delete 2 words 
d2w
# delete two lines
2dd
```
## Undo & redo
Key | Effect
--- | ---
u | undo last edit
U | return current line to prior state
ctrl r | redo 

## Basic Editing

Key | Effect
--- | ---
i | insert text after the cursor
A | move to end of line and enter edit mode

### Put command
Press `p` to put the previously deleted text after the cursor
### Replace command 
Press `r<char>` to replace the character under the cursor with <char>. Eg `rx` replaces the character under the cursor with `x`.
