---
layout: page
title: OS X
tagline: Mac Crack
description: Favorite Os of the Elite Hipster Programmer with Sticker Festooned Laptop
---

[return to editors](../../os_specifics.html)

# Finder

## Tags

If you drag file into a tag, it will apply a flag.
The last button on the top of the finder is the tag button.
You can add tags to an item in the finder by hitting the tag button.

## Search

- Perform Boolean Query
- use AND, OR, NOT or - (and not)

## In the Terminal

mdfind : 
	search using native spotlight capabilities

### search for files with tags

mdfind -onlyin \~ '<tag:tagname1> AND <tag:tagname2> -<tag:tagname3>'
