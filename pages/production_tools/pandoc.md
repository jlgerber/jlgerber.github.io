# Using Pandoc to convert document formats

to convert a document from org to pdf:

export PATH=${PATH}:/Library/TeX/Root/bin/x86_64-darwin/
pandoc -s -S <DOC>.org -o <DOC>.pdf
