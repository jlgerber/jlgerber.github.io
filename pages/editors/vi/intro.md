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
[number] G | move to the line number (number + G, eg 465G to go to line 465)

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
press `Ctrl-g` to get the current file's status

## Searching and Replacing 
### Search Command
In normal mode, press `/` to go into search forward mode. Your cursor will jump to the minieditor and you will be able to type a string to search for and type ENTER. 

To search for the next occurence of the string, type `n`.

To search backwards for the previous occurence of the string, type `N`.

To turn off the color highlight, press `:noh`

### Search Backwards Command
In normal mode, press `?` to enter search backwards mode. In backwards mode, `n` advances up the page, and `N` advances down the page - the opposite of '/' search mode.

To go back where you came from, press `<Ctrl-o>`. To go forward again, press `<Ctrl-i>`
### Search for matching (, [, or {
When programming, the `%` key is essential. It will bounce back and forth between matching `(`, `[` or `{`. 

### Substitution
Substitution should look familiar as it uses the sed form.

Key | Effect
--- | ---
`:s/old/new/` | replace first occurence of `old` on current line with `new`
`:s/old/new/g` | replace all occurences of `old` on current line with `new`
`:l1,l2s/old/new/g` | replace all occurences of `old` between line `l1` and line `l2` with `new`
`:%s/old/new/g` | replace all occurences of `old` with `new` in file
`"%s/old/new/gc` | replace all occurences of `old` with `new` in file but prompt for each replacement

## Executing a command
type `:!` plus the command to execute a command in a shell and return the results in a separate buffer.

## Saving a buffer to a file
Type `:w FILENAME` to save the buffer as FILENAME.
