---
layout: page
title: Pandoc
tagline: Convert all the things
description: Fully functional Haskell program to convert between different text formats.
---

# Using Pandoc to convert document formats

to convert a document from org to pdf:

export PATH=${PATH}:/Library/TeX/Root/bin/x86_64-darwin/
pandoc -s -S <DOC>.org -o <DOC>.pdf
