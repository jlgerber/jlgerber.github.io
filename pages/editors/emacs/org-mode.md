Org Mode Basic Usage
====================

visibility cycling
------------------

-   3 states
    -   completely visible
    -   headline only visible
    -   folded
-   hotkeys for folding
    -   tab cycles through modes for a tab
    -   shift-tab cycles through modes for the document

buffer narrowing
----------------

-   buffer narrowing limits the *view* of the current buffer (C-x n)
-   Org mode adds the ability to narrow to a subtree ( C - x n s )
-   widen back out C-x n w)

Lists
-----

-   lists come in two flavors: bullet lists and number lists
    -   unordered lists use \`-\`
    -   you can cycle through representations using Shift-right and
        Shift-left

Navigation
----------

-   move to the previous headline C-c C-p
-   move to next headline C-c C-n
-   move to the next higher headline C-c C-u
-   move to headlines at the current level with C-c C-f and C-c C-b

creating new headlines
----------------------

-   M-RETURN creates a new headline or list item
-   C-RETURN creates a new headline from whereever you are

Moving items
------------

-   M-RIGHT promote M-left demote
-   M-SHIFT-LEFT promote structure and children
-   M-SHIFT-RIGHT demote structure and children

Hack Emacs - Markup and Links
=============================

Fonts and color schemes
-----------------------

Depending on what color theme and font settings you have, the markup may
vary.

-   *italic*
-   **bold**
-   **underlined**
-   Links
    -   links are automatically inkified by typing <http://>
    -   links may me opened using C-c C-o
    -   Use the format

        ``` {.example}
        [[http://google.com][Google]]
        ```

    -   [Google](http://www.google.com)

-   Latex markup is supported
    -   Union and intersection: ∪∩
    -   Greek characters: *πΣαβ*
    -   Mathematical relations: x ≤y
    -   superscripts: x \* x = x^2^
    -   and may others. go search google

Links
-----

Links warran an entire episode bug

-   links cna be a tom of types
    -   <ftp://>
    -   <http://>
    -   file:
    -   mailto:
    -   elisp:
    -   shell:
-   these kinds of links give the ability to write Org Mode files that
    are programs that have "buttons" that invoke code

### examples

-   [elisp: (color-theme-subtle-hacker)](elisp: (color-theme-subtle-hacker))

    ``` {.example}
    [[elisp: (color-theme-subtle-hacker))]]
    ```

-   [Comidia](elisp:(color-theme-comidia))

    ``` {.example}
    [[elist: (color-theme-comidia)][comidia]] 
    ```

top
===

second level
------------

### 3rd level

some text

### 3rd level

-   \[ \] write more text
-   \[ \] and yet more text

Moving and Promoting
--------------------

-   You can move outline points around by holding the Meta key and using
    up and down
-   You can promote and demote levels by using the meta key and pressing
    the left or right key

metadata
--------

-   \[ \] you can use to indicate a checkbox. you can update checkbox
    state by C-c C-c
-   You can apply tags using :TAGNAME:

### TODO \[\#B\] Get new Laptop<span class="tag" data-tag-name="buy"></span><span class="tag" data-tag-name="Frys"></span><span class="tag" data-tag-name="work"></span>

DEADLINE: &lt;2016-07-03 Thu&gt;

Metadata progagates downward
----------------------------

### parenet tags can be inherited by by children

### parent properties can be inherited as well

upward propagation: accumulation
--------------------------------

### certain properties like number, itmes or status flags can be summed automatically

### special interfaces for Meta Data

#### one-key-per-tag interface

#### completion wherever useful

#### date/time reading funciton second to none

#### column view for fast tabular editing of meta data

remember.el by John Wiegley is the ultimate capture tool for emacs
==================================================================

org-mode allows to set up template for remember
-----------------------------------------------

templates define content and meta data like:
--------------------------------------------

-   a TODO headline
-   a link to context
-   a target locaiton for a note

