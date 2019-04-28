---
layout: page
title: OS X
tagline: Mac Crack
description: Favorite Os of the Elite Hipster Programmer with Sticker Festooned Laptop
---

[return to os specifics](../os_specifics.html)

# Finder

## Tags

If you drag file into a tag, it will apply a flag.
The last button on the top of the finder is the tag button.
You can add tags to an item in the finder by hitting the tag button.

## Search in the Terminal using **mdfind**

mdfind : 
	search using native spotlight capabilities

- Perform Boolean Query
- use AND, OR, NOT or - (and not)

### search for files with tags

mdfind -onlyin \~ '<tag:tagname1> AND <tag:tagname2> -<tag:tagname3>'

### Opening files in the Terminal with the Correct App using **open**

lets say I want to open a file with a registered application. I can just use *open APPNAME*.
