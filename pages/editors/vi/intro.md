# vi / vim / gvim / nvim

## Repeat
Repeat the last command by typing the `.` character.

## Basic Character  Navigation

Key | Effect
--- | ---
`h` | move left one character
`l` | move right one character
`j` | move down one character
`k` | move up one character

## Basic Word Navigation
Key | Effect
--- | ---
`w` | move right to the first char of the next word. Note that periods, commas, are considered words in their own right.
`W` | move right to the first char of the next WORD based on whitespace separation. Commas and periods be damned.
`e` | move right to the end of the word, taking commas and periods into account
`E` | move right to the end of the word, based on whitespace boundaries
`b` | move left to the beginning of the word, counting periods and commas, etc as words
`B` | move left to the beginning of the word, using whitespace as the sole delimiter
`ge` | move left to the end of the previous word, recognizing period, comma, and sundry other symbols as words
`gE` | move left to the end of the previous word, considering only whitespace as word delimiter

## Basic Page Navigation
Key | Effect
--- | ---
`Ctrl f` | move forward one whole page
`Ctrl b` | move back one whole page
`ctrl d` | move forward half page
`ctrl u` | move back half page

## Basic Screen Navigation
Key | Effect
--- | ---
`H` | First line of the screen
`M` | Middle line of the screen
`L` | Last line of the screen

### Advanced Navigation

Key | Effect
--- | ---
`fCHAR` | go to the next occurence of CHAR. `;` repeats the previous search (I know, why not `.`?)
`tCHAR` | go one left of the next occurence of CHAR. `;` repeats the previous search
$ | move to end of line
0 | move to start of line
^ | move to first character of first word on line
G | move to the end of the file
gg | move to the top of the file
NUMBER G | move to the line number (number + G, eg 465G to go to line 465)
:NUMBER | Also move to the line number (eg :456 ENTER goes to line 456)
`%` | When the cursor is over a { or a ( or a [, pressing `%` will jump to its pair
`[[` | go to the next outer `{`
`]]` | go to the previous outer `{`
`][` | go to next outer closing `}`
`[]` | go to previous outer closing `}`

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

### Insert && Append Modes
Key | Effect
--- | ---
i | (insert mode) insert text after the cursor
I | (insert mode) start insert mode at the beginning of the current line
a | (append mode) move one char to the right and go into edit mode. Typing will insert text.
A | (append mode) move to end of line and enter edit mode
o | open the line below the current line, inserting a newline, and enter edit mode
O | open the line above the current line, inserting a new line, and enter edit mode

`a`, `i`, `A`, `o`, and `O` all go into insert mode. The only difference is where the characters are inserted.

### Replace Mode
typing `R` enters **replace** mode. This allows you to write over characters rather than insert new characters.

### Visual Marking Mode
To enter into visual mode (like emacs marking mode), type `v`, then use your motion keys to highlight a block of text.
If you highlight lines and type `:w FILENAME`, vi will save the highlighted lines to `FILENAME`. (note that it will save complete lines, regardless of how much text you have highlighted on the first or last line)

In visual mode, you can, for instance, use `d`, to delete the selected text.
### Yank command
if you mark text in Visual Marking mode, you can copy it by pressing `y`. This will save it to the buffer. yank may be used as an operator. For example `yw` yanks one word.
### Put command
Press `p` to put the previously yanked or deleted text after the cursor. If you go into visual marking mode (v), select some text, and press `y`, then navigate elsewhere and press `p`, you will paste the yanked text at the new location. If you delete a line with `dd`, `p` will put the deleted line on the line below the one that the cursor is on now.

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
### Search for Current Word
With your cursor over a word, if you type `*`, you will start searching for the current word. It is as if you went `/` and typed the work name. Sweet.

Of course, if you want to search backwards from the get go, use `#` instead of `*` and `n` will take you backwards...

Both of these searches include word boundaries, so they are exact match searches. If you want to select partials, put a `g` in front of `*` or `#`. Eg `g*`

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

## More Commands
### Executing a command
type `:!` plus the command to execute a command in a shell and return the results in a separate buffer.

### Saving a buffer to a file
Type `:w FILENAME` to save the buffer as FILENAME.

### Insert contents of File at Cursor
To insert the contents of a file at the cursor, type `:r FILENAME`.

You can also read the output of an external command. For instance, `:r !ls` would read the contents of ls into the current 
file at the cursor.

## Set Command
You can configure vi behavior using the `:set` command. For instance, to make searches case insensitive, type `:set ic`. This turns on ignore case mode. To turn case sensitivity back on, type `:set noic`.

To toggle the value of a setting, prefix it with `inv`. For example `:set invic`.

NOTE: if you want to ignore case for one search, just append a `\c` to your search. For example `/foo\c`

### Set Commands

Command | Effect
--- | ---
ic or ignorecase | ignore case
noic | turn off ignore case
invic | toggle ignore case 
is or incsearch |  show partial matches for a search phrase
nois | turn off partial matches
invis | toggle partial searching
hls or hlsearch | highlight all matching phrases
nohls | turn off highlighting matching phrases
invhls | toggle highlighting
cpoptions+=$ | mark the end of an edit with $. (eg `cw` would put a `$` at the end of the current word)

# Command Completion
in the mini editor you can get a list of commands which match what you have typed so far by typing `Ctrl d`. You can complete your command by typing `<TAB>`.

# Marks
vim has bookmarks, although they are not visible without a plugin.

Command | Effect
--- | ---
`mCHAR` | set a bookmark on the current line (eg `ma`)
`'CHAR | go to a previously set bookmark (eg `'a`)
`:marks` | list bookmarks
`''` | go back to the last place you were (auto bookmark)

# Opening URLs in Web browser from Vim
Put your curor over a url and type `gx`

# plugins we want
- matchit.vim
