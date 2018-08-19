Formats
=======

Markdown
--------

### How do you convert Org mode to Markdown?

#### Org mode can export to markdown

-   C-c C-e m m (org-md-export-to-markdown)
    -   Export as a text file written in Markdown syntax. For an Org
        file, myfile.org, the resulting file will be myfile.md. The file
        will be overwritten without warning.
-   C-c C-e m M (org-md-export-as-markdown)
    -   Export to a temporary buffer. Do not create a file.
-   C-c C-e m o
    -   Export as a text file with Markdown syntax, then open it.

Org ( emacs mode )
------------------

### How do you convert from Mardown to Org?

#### Use Pandoc

##### Single Document

``` {.shell}
     pandoc -f markdown -t org -o newfile.org original-file.markdown
```

##### Bulk Conversion

``` {.shell}
 find . -name \*.txt -type f -exec pandoc  -f markdown -t org -o {}.org {} \; 
```

Shell Foo
=========

sed
---

### How to manipulate file names and rename files using a single line of sed

-   Steps
    -   pipe names into sed
    -   used p;s to return the original and modified string
    -   pipe to xargs, use -n2 followed by command
-   Example ( take all filename.md.org files and get rid of .md )

    ``` {.sed}
    ls -1 *md.org | sed 'p;s/\.md//' | xargs -n2 mv
    ```
