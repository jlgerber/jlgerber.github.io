Hotkeys
=======

Bookmarks
---------

bookmarks are set for a file and line in the file. They are persisted to
disk and you can jump between files and locations.

  Key       Description
  --------- ------------------
  C-x r m   Set bookmark
  C-x r b   Jump to bookmark
  C-x r l   List Bookmarks

Font Size
---------

  key       description
  --------- ---------------------------
  C-x C-+   increase buffer font size
  C-x C--   Decrease buffer font size
  C-x C-0   reset buffer font size

Imenu
-----

Imenu allows you to browse symbols. i have defined the following hotkeys

  key     description
  ------- ---------------------------------------------
  C-x c   bring up imenu helm menu for current buffer

Manipulating Word Case
----------------------

  key   description
  ----- -------------------------------
  M-c   Capitalize the following word
  M-l   Lower case the following word
  M-u   Uppercase the following word

Navigation
----------

  key      description
  -------- -----------------------------------------------
  C-f      Move **forward** one **character**
  C-b      Move **backward** one **character**
  M-f      Move **forward** one **word**
  M-b      Move **backward** one **word**
  C-e      Move to **end** of **line**
  C-a      Move to **start** of **line**
  C-n      Move **down** one **line**
  C-p      Move **up** one **line**
  C-v      Move **down** one **window's content**
  M-v      Move **up** one **window's content**
  M-&lt;   Move to the **top** of the **buffer**
  M-&gt;   Move to the **bottom** of the **buffer**
  C-M-f    Move forward to matching curly brace / parens
  C-M-b    Move back to matching curyly brace / parens
  M-}      Move forward to next paragraph
  M-{      Move backward to previous paragraph

### Move Cursor

-   C-p : up one line
-   C-n : down one line

### Scroll Screen

-   M-v : up one window's context
-   C-v : down one window's content

Registers
---------

you can store bits of information in registers. I particularly like
position registers.

  key         description
  ----------- ------------------------------------------------------------------------
  C-x r SPC   Store a position in a buffer with a single letter or number as a label
  C-x r j     Recall a position buffer with a particular label

Org Mode
--------

### Tables

-   C-x - insert a line under the current one

Elisp
=====

general
-------

### C-h f

get help for the function you are over

eldoc mode
----------

if you enable eldoc mode, you can press space on a function definition
and get its params in the minibuffer.

debugger
--------

debug-on-error t

### exit with q

### hit 'e' to evaluate any context

e-debug
-------

interactive debugging is fantastic. You can step through line at a time.
it is easier than printing

### C-u C-M-x

### ?

shows keys in debug

commands
--------

### are enclosed in parens

(message "this is a message")

### start with defun

( defun foo () (interactive) (message "hello"))

### (interactive) makes the command available in emacs via x-M

### on exception, you are thrown into the debugger

### c-x c

paredit.el
----------

good editing helper for elisp

### barfing and slurping

#### spit out an element from the lisp form

C-\[\]

#### slurping

C-(

Packages
========

yasnippet
---------
