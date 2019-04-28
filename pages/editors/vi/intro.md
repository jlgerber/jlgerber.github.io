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
G | move to the end of the file
gg | move to the top of the file

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
Press `p` to put the previously deleted text after the cursor. If you delete a line with `dd`, `p` will put the deleted line on the line below the one that the cursor is on now.
### Replace command 
Press `r<char>` to replace the character under the cursor with <char>. Eg `rx` replaces the character under the cursor with `x`. Note that you will be in nav mode after you type the replacement character; you will not be in edit mode.
 
 ### Change operator
  `c` is the change operator. It may be used with motion keys, `w`, `e`, `$`. For example `ce` deletes the characters under the cursor through the end of the word and puts you into edit mode. 
 
 In practice, to correct `thwf`, navigate to the `w`, press `ce` and then type `is` and <esc> to get out of edit mode.
 
 You may also use the `c` motion key with a count, just like `d`

## File Status
press `Ctrl-g` to get the current file's statuse
