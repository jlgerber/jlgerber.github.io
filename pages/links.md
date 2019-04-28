---
layout: page
title: links
---

Accounts
========

Databases
=========

Postgres
--------

### [postgrest - auto api generation](http://postgrest.com/examples/users/)

performat, automated, written in haskell. has a LOT going for it....

DevOps
======

Logging
-------

### [logstash vs Rsyslog](https://sematext.com/blog/2015/05/18/tuning-elasticsearch-indexing-pipeline-for-logs/)<span class="tag" data-tag-name="logging"></span><span class="tag" data-tag-name="logstash"></span><span class="tag" data-tag-name="rsyslog"></span><span class="tag" data-tag-name="syslog"></span><span class="tag" data-tag-name="ELK"></span>

-   cliff notes
    -   log in JSON
    -   parallelize when possible
    -   use time based indices
    -   use hot / cold nodes policy

### [centralize logging with ELK stack](http://www.freeipa.org/page/Howto/Centralised_Logging_with_Logstash/ElasticSearch/Kibana)<span class="tag" data-tag-name="logging"></span><span class="tag" data-tag-name="logstash"></span><span class="tag" data-tag-name="ELK"></span><span class="tag" data-tag-name="elasticsearch"></span>

### [scaling logstash](http://engineering.chartbeat.com/2015/05/26/logstash-deployment-and-scaling-tips/)<span class="tag" data-tag-name="logging"></span><span class="tag" data-tag-name="logstash"></span><span class="tag" data-tag-name="ELK"></span><span class="tag" data-tag-name="elasticsearch"></span>

### [rsyslog - rocket-fast system for log processing](http://www.rsyslog.com/)<span class="tag" data-tag-name="logging"></span><span class="tag" data-tag-name="rsyslog"></span><span class="tag" data-tag-name="syslog"></span>

### [rsyslog video tutorial](http://www.rsyslog.com/video-tutorials/)<span class="tag" data-tag-name="logging"></span><span class="tag" data-tag-name="rsyslog"></span><span class="tag" data-tag-name="tutorial"></span><span class="tag" data-tag-name="video"></span>

Process Management
------------------

### Looking up processes on ports

#### sudo lsof -i

lists on its standard output file information about files opened by
processes for unix. An open file may be a regular file, a directory, an
execting text reference, a network file ( socket, nfs file or unix
socket) etc.

-i selects the listing of files on all internet and x.25 network files.
you may also specificy a specific port using -i

#### lsof -i :22

see which process is bound to a particular port (in this case 22) then
you cna use \`whatis\` to look up what a particular process is.

#### sudo netstat -lptu

#### sudo netstat -tulpn

look up process and protocols

load generators
---------------

### Response time vs service time

we often just measure service time. we need to measure response time.

### wcsb on github ( avoid coordinated omission )

### wkrt2

Editors<span class="tag" data-tag-name="editors"></span><span class="tag" data-tag-name="editing"></span><span class="tag" data-tag-name="coding"></span>
=========================================================================================================================================================

Emacs
-----

### Basics

#### [emacs reference card](https://www.gnu.org/software/emacs/refcards/pdf/refcard.pdf)<span class="tag" data-tag-name="emacs"></span><span class="tag" data-tag-name="reference"></span><span class="tag" data-tag-name="navigation"></span>

#### [MELPA - package management for emacs](https://melpa.org/#/)<span class="tag" data-tag-name="emacs"></span><span class="tag" data-tag-name="packagemanagement"></span><span class="tag" data-tag-name="package"></span><span class="tag" data-tag-name="repo"></span>

### Modes

#### Org Mode<span class="tag" data-tag-name="emacs"></span><span class="tag" data-tag-name="mode"></span><span class="tag" data-tag-name="todo"></span><span class="tag" data-tag-name="markdown"></span><span class="tag" data-tag-name="planning"></span>

##### [org mode reference card](http://orgmode.org/orgcard.pdf)<span class="tag" data-tag-name="emacs"></span><span class="tag" data-tag-name="todo"></span><span class="tag" data-tag-name="markdown"></span><span class="tag" data-tag-name="planning"></span><span class="tag" data-tag-name="reference"></span>

##### *orgmode main page*<span class="tag" data-tag-name="emacs"></span><span class="tag" data-tag-name="mode"></span><span class="tag" data-tag-name="todo"></span><span class="tag" data-tag-name="markdown"></span><span class="tag" data-tag-name="planning"></span>

#### Helm Mode<span class="tag" data-tag-name="emacs"></span><span class="tag" data-tag-name="mode"></span>

##### [Helm Mode - EmacsWiki](https://www.emacswiki.org/emacs/Helm)<span class="tag" data-tag-name="emacs"></span><span class="tag" data-tag-name="mode"></span><span class="tag" data-tag-name="wiki"></span><span class="tag" data-tag-name="completion"></span>

Generic Framework for quicly accessing stuff with emacs.

##### [Helm setup tutorial](https://www.youtube.com/watch?v=k78f8NYYIto)<span class="tag" data-tag-name="emacs"></span><span class="tag" data-tag-name="mode"></span><span class="tag" data-tag-name="helm"></span><span class="tag" data-tag-name="setup"></span><span class="tag" data-tag-name="tutorial"></span><span class="tag" data-tag-name="video"></span>

##### [Helm Video](https://www.youtube.com/watch?v=k78f8NYYIto)<span class="tag" data-tag-name="emacs"></span><span class="tag" data-tag-name="mode"></span><span class="tag" data-tag-name="helm"></span><span class="tag" data-tag-name="video"></span><span class="tag" data-tag-name="tutoria"></span>

##### Configuring Helm

``` {.commonlisp}
;; for the code completion
(semantic-mode 1)

;; optionally (require 'helm)
(require 'helm-config)

;; the following line requires (require 'helm)
(define-key helm-map (kbd "<tab>") 'helm-execute-persistent-action)

;; use this to bring up a code symbol browser
(global-set-key (kbd "C-c h i") 'helm-semantic-or-imenu)
;; replace the standard buffer list with a helm buffer
(global-set-key (kbd "C-x b") 'helm-buffers-list)
;; access bookmarks
(global-set-key (kbd "C-x r b") 'helm-bookmarks)
;;call helm search 
(global-set-key (kbd "C-x m") 'helm-M-x)
;; show a helm style kill ring
(global-set-key (kbd "C-x y") 'helm-show-kill-ring)
;; use helm to find files
(global-set-key (kbd "C-x C-f") 'helm-find-files)
```

#### [Magit Mode](https://magit.vc) - Git From Emacs<span class="tag" data-tag-name="git"></span><span class="tag" data-tag-name="vc"></span><span class="tag" data-tag-name="versioncontrol"></span>

##### [Magit User Manual](https://magit.vc/manual/magit/#Top)

##### [Magit Reference Card](https://magit.vc/manual/magit-refcard.pdf)

#### [eshell](http://www.gnu.org/software/emacs/manual/html_node/eshell/) - a shell + lisp environment

#### [IELM](http://emacs-fu.blogspot.com/2011/03/ielm-repl-for-emacs.html) REPL for emacs lisp

### Packages

#### [Yasnippet](https://github.com/joaotavora/yasnippet)

##### [tutorial 1](http://cupfullofcode.com/blog/2013/02/26/snippet-expansion-with-yasnippet/index.html)<span class="tag" data-tag-name="tutorial"></span>

###### Basics

You provide a snippet file with the following info

###### Tab Stops

-   You can supply tabstops using \$N (eg \$1, \$2 )
-   \${N:default} : Default value for tabstop

<!-- -->

-   eg \${1:"foo"}
    -   \$0 is a special marker. it represents the final place to end
        the tabstops

###### Mirrored Fields

If you use the same \$N in multiple locations, it will be mirrored. You
should only define the default value in the first use of \$N. For
example: Vector(\${1:0}, \$1, \$1)

###### elisp

You can also embed lisp and have it execute automagically.

-   Inside a default value, you can place lisp in \$() and it will do
    its thing
-   You can place lisp in between \`()\` in the template, and it will
    execute as well.

Languages<span class="tag" data-tag-name="programming"></span><span class="tag" data-tag-name="docs"></span>
============================================================================================================

Python<span class="tag" data-tag-name="python"></span>
------------------------------------------------------

### Tools

#### vprof<span class="tag" data-tag-name="python"></span><span class="tag" data-tag-name="profiler"></span><span class="tag" data-tag-name="debugging"></span><span class="tag" data-tag-name="debugger"></span>

profiler which allows you to explore performance of a python script
providing insights into exection.

##### [github vprof](https://github.com/nvdv/vprof)<span class="tag" data-tag-name="link"></span>

Usage: vprof -c &lt;modes&gt; -s &lt;test~program~&gt; vprof -c cmh -s
"testscript.py --foo --bar" vprof -c cm -s testscript.py

Supported modes:

-   c : flame chart. Renders running time visualization for
    &lt;test~program~&gt;.
-   m : memory graph. Shows memory usage during execution of each line
    of &lt;test~program~&gt;.
-   h : code heatmap. Shows number of executions of each line of code.

vprof can also profile single functions. In order to do this, launch
vprof in remote mode:

-   vprof -r

And then to profile a function you can do:

``` {.python}
from vprof import profiler

def foo(arg1, arg2):
    ...

profiler.run(foo, 'cmh', args=(arg1, arg2), host='localhost', port=8000)
```

#### Rest Client Mode<span class="tag" data-tag-name="emacs"></span><span class="tag" data-tag-name="mode"></span>

interact wit hHTTP endpoints from an emacs buffer. Get it from MELPA.

##### [github restclient](https://github.com/pashky/restclient.el)<span class="tag" data-tag-name="emacs"></span><span class="tag" data-tag-name="mode"></span><span class="tag" data-tag-name="github"></span><span class="tag" data-tag-name="docs"></span><span class="tag" data-tag-name="coding"></span>

##### Hotkeys in Major Mode

-   C-c C-c : runs the query under the cursor and tries to prettyprint
    the result
-   C-c C-r : same but doesn't do anythnig with the response. Just shows
    the buffer
-   C-c C-v : same as C-c C-c but doesnt switch focus to other window
-   C-c C-p : jump to the previous query
-   C-c C-n : jump to the next query
-   C-c C-. : mark the query under the cursor
-   C-c C-u : copy query under the cursor as a curl command
-   C-c C-g : start a helm session iwth the sources for variables and
    requests

##### In buffer Variables

###### declaration

-   :myvar = the value
-   :myvar := (some (arbitrary 'elisp))

variables may be multi line as well

    :myvar = <<
    Authorization: :my-auth
    Content-Type: application/json
    User-Agent: SomeApp/1.00
    #

or

``` {.shell}
:myvar := <<
(some-long-elisp
  (code spanning many lines)
#
```

###### usage

After the var is declared, you can use it in the URL, the header values
or the body

``` {.shell}
:my-auth = adfadsfqwrqrwt
:my-headers = <<
Authorization :my-auth
Content-Type: application/json
User-Agent: SomeApp/1.0

#update a user's name
:user-id = 7
:the-name := (format "%s %s s" 'neo (md5 "the chosen") (+ 100 1))

PUT http://localhost:4000/users/:user-id/
:my-headers

{"name" : ":the-name"}
```

##### Query File Example

``` {.shell}
# -*- restclient -*-
#
# Gets  all Github APIs, formats JSON, shows response status and headers underneath.
# Also sends a User-Agent header, because the Github API requires this.
#
GET https://api.github.com
User-Agent: Emacs Restclient

#
# XML is supported - highlight, pretty-print
#
GET http://www.redmine.org/issues.xml?limit=10

#
# It can even show an image!
#
GET http://upload.wikimedia.org/wikipedia/commons/6/63/Wikipedia-logo.png
#
# A bit of json GET, you can pass headers too
#
GET http://jira.atlassian.com/rest/api/latest/issue/JRA-9
User-Agent: Emacs24
Accept-Encoding: compress, gzip

#
# Post works too, entity just goes after an empty line. Same is for PUT.
#
POST https://jira.atlassian.com/rest/api/2/search
Content-Type: application/json

{
        "jql": "project = HSP",
        "startAt": 0,
        "maxResults": 15,
        "fields": [
                "summary",
                "status",
                "assignee"
        ]
}
#
# And delete, will return not-found error...
#
DELETE https://jira.atlassian.com/rest/api/2/version/20
```

Rust<span class="tag" data-tag-name="rust"></span>
--------------------------------------------------

### Main Pages

#### [main rust page](https://www.rust-lang.org/documentation.html)

#### [rust std library](https://doc.rust-lang.org/std/)

#### [rust tutorials](http://hackr.io/tutorials/rust)<span class="tag" data-tag-name="tutorial"></span>

#### [Learning Rust](https://github.com/ctjhoa/rust-learning)

#### [rustup](https://github.com/rust-lang-nursery/rustup.rs) - working with multiple versions of rust

### Resources

#### [rust by example](http://rustbyexample.com/)

#### [rust design patterns](https://github.com/nrc/patterns)

#### [rust guidelines](http://aturon.github.io/)

#### [rustcamp 2015](http://confreaks.tv/events/rustcamp2015)

#### [rust learning](https://github.com/ctjhoa/rust-learning)

#### [rustlings](https://github.com/carols10cents/rustlings)

### Crates

#### [zeromq rust](https://github.com/erickt/rust-zmq)

#### [ssh2](https://github.com/alexcrichton/ssh2-rs)

#### [low level networking](https://github.com/libpnet/libpnet)

#### Compression

##### [zip](https://github.com/slackito/zip)

##### [tar](https://github.com/alexcrichton/tar-rs)

##### [bzip2](https://github.com/alexcrichton/bzip2-rs)

#### [docopt](https://github.com/docopt/docopt.rs)

#### [memcached](https://github.com/jaysonsantos/bmemcached-rs)

#### [rust cpython](https://github.com/dgrunwald/rust-cpython)

#### [rust container system](https://github.com/tailhook/vagga)

#### [fuse file system](https://github.com/zargony/rust-fuse)

#### Databases

##### [elasticsearch](https://github.com/benashford/rs-es)

##### [postgres](https://github.com/sfackler/rust-postgres)

##### [mongo](https://github.com/mongodb-labs/mongo-rust-driver-prototype)

#### [sharedlib](https://tyleo.github.io/sharedlib/doc/sharedlib/index.html) loading a library at runtime

#### Serialization

##### json

###### [serde~json~ crate documentation](https://serde-rs.github.io/json/serde_json/index.html)

####### [using serde JSON](https://github.com/serde-rs/json)

#### Parsers

##### [combine](https://marwes.github.io/combine/combine/index.html)<span class="tag" data-tag-name="5star"></span>

#### Formatting

##### [prettytable-rs console table printing](https://github.com/phsym/prettytable-rs)

### podcasts

#### [new rustacean](http://www.newrustacean.com/)

#### [rusty radio](https://soundcloud.com/posix4e/sets/rustyradio)

### Blogs

#### [baby steps](http://smallcultfollowing.com/babysteps/)

##### [using the option type effectively](https://blog.8thlight.com/uku-taht/2015/04/29/using-the-option-type-effectively.html)

#### [Non Lexical Lifetimes (babysteps)](http://smallcultfollowing.com/babysteps/blog/2016/04/27/non-lexical-lifetimes-introduction/)

### Books

#### [24 days of rust](http://zsiciarz.github.io/24daysofrust/) (list of neat crates)

#### [Too Many Lists](http://cglab.ca/~abeinges/blah/too-many-lists/book/README.html)<span class="tag" data-tag-name="tutorial"></span><span class="tag" data-tag-name="book"></span><span class="tag" data-tag-name="5"></span><span class="tag" data-tag-name="star"></span>

great tutorial on rust. Goes through the exercise of iterating on a
linked list. Along the way, you explore rust. Probably the best learning
tutorial out there.

#### [glium - opengl in rust](https://tomaka.github.io/glium/book/index.html)

### Tutorials<span class="tag" data-tag-name="tutorial"></span>

#### [ArcadeRs - learn rust by writing a game](https://jadpole.github.io/arcaders/arcaders-1-0)

#### [building a web server in rust](https://dfockler.github.io/2016/05/20/web-server.html)<span class="tag" data-tag-name="tutorial"></span><span class="tag" data-tag-name="web"></span>

#### [controlling executable size](https://lifthrasiir.github.io/rustlog/why-is-a-rust-executable-large.html)

#### [creating a basic web service in rust](http://hermanradtke.com/2016/05/16/creating-a-basic-webservice-in-rust.html)<span class="tag" data-tag-name="tutorial"></span><span class="tag" data-tag-name="web"></span>

#### [creating a rust function that accepts string or str](http://hermanradtke.com/2015/05/06/creating-a-rust-function-that-accepts-string-or-str.html)

#### [error handling in rust](https://doc.rust-lang.org/book/error-handling.html#the-try-macro)<span class="tag" data-tag-name="5star"></span>

#### [rust error handling](http://blog.burntsushi.net/rust-error-handling/)<span class="tag" data-tag-name="5star"></span>

#### [set up emacs for rust development](http://julienblanchard.com/2016/fancy-rust-development-with-emacs/)

#### [setting up local crates](https://gillesleblanc.wordpress.com/2014/10/10/using-a-local-crate-with-cargo/)

#### [string vs str in functions](http://hermanradtke.com/2015/05/03/string-vs-str-in-rust-functions.html)

#### [using Copy on Write for optional allocation](http://hermanradtke.com/2015/05/29/creating-a-rust-function-that-returns-string-or-str.html)<span class="tag" data-tag-name="5star"></span>

#### [The Guide to Rust Strings](http://www.steveklabnik.com/rust-issue-17340/)

#### [writing an OS in rust](http://os.phil-opp.com/multiboot-kernel.html)<span class="tag" data-tag-name="tutorial"></span><span class="tag" data-tag-name="rust"></span><span class="tag" data-tag-name="os"></span>

#### [more on borrowing, lifetimes, and moves](https://medium.com/@bugaevc/understanding-rust-ownership-borrowing-lifetimes-ff9ee9f79a9c#.s9rmhcok1)

Lisp
----

### eLisp<span class="tag" data-tag-name="emacs"></span>

### *SBCL* - A very performant Lisp implementation

### [clasp](https://github.com/drmeister/clasp) - Common Lisp Environment - LLVM back end<span class="tag" data-tag-name="llvm"></span>

#### [Clasp author's blog](https://drmeister.wordpress.com/)

Javascript
----------

### Libraries and Frameworks

#### React

##### Starting React Project

###### Install Dependencies

-   npm init
-   sudo npm install babel webpack webpack-dev-server -g (i use ansible)
-   npm install react react-dom --save
-   npm install babel-loader babel-core babel-preset-2015
    babel-preset-react

###### create webpack.config.js

``` {.javascript}
module.exports = {
    entry: './main.js', // entry point.
    output: :{
     path: './', // could be ./build
     filename: 'index.js' // name of js file generated by webpack
   },
   devServer: {
     inline: true,
     port: 3333
   },
   module: {
      loaders: [
        {
          test: /\.js$/,
          loader: 'babel',
          query: {
           presets: ['es2015', 'react']
          }
        }
      ]
  }
}
```

###### create minimum files

####### index.html

Create a placeholder div with an id into which we are going to render
our app

####### App.js

main components

####### main.js

imports react and renders the app to the document

``` {.javacsript}
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';
ReactDOM.render('<App />', document.getElementById('app'))
```

####### update package.json

under scripts, add a start tag which point at webpack-dev-server

###### Run npm start

##### Components

###### React Ui Kits

####### [zippy ui](http://zippyui.com/)

carefully crafted ui components for React. Very cool

####### [react-bootstrap](https://react-bootstrap.github.io/components.html#buttons)

###### Handling Images

####### [react-smart-image](https://github.com/gwendall/react-smart-image)

Enhanced IMG component which provides a placeholder while image is
loading as well as a fallback image.

####### [legit-react](https://github.com/legitcode/image)

handles lazy loading. fades in once image loads.

###### Node Graphs

####### [react-node-graph](https://github.com/lightsinthesky/react-node-graph)

sweet looking node graph. Could be used to graph depndencies

###### Tables

####### [react-virtualized](https://github.com/bvaughn/react-virtualized)

react components for efficiently rendering large lists of tabular data

####### [react-pivot](https://github.com/davidguttman/react-pivot)

React-Pivot is a data-grid with a pivot-table-like functionality for
data display, filtering, and exploration.

####### [react-datagrid](http://zippyui.com/react-datagrid/#/)

high performance, production ready, customizable. seems very nice. makes
it easy to add remote data. shows 1milliion recorrd table.

### node

#### [installation gist](https://gist.github.com/isaacs/579814)

#### React

##### \[\[

Programming
===========

[replace recursion with stack](http://haacked.com/archive/2007/03/04/Replacing_Recursion_With_a_Stack.aspx/)
------------------------------------------------------------------------------------------------------------

Shells
======

[fish shell](http://fishshell.com/) - New Shell<span class="tag" data-tag-name="5star"></span>
----------------------------------------------------------------------------------------------

CVS
===

[Git](https://git-scm.com/)
---------------------------

### [Documentation](https://git-scm.com/doc)

### [git workflows](https://www.atlassian.com/git/tutorials/comparing-workflows/)

Tools
=====

Pandoc
------

### [creating an ebook from markdonw with pandoc](http://pandoc.org/epub.html)<span class="tag" data-tag-name="tutorial"></span>

Shotgun
-------

### [developer docs](http://developer.shotgunsoftware.com/python-api/)

woodworking
===========

shops
-----

### [highland woodworking](http://www.highlandwoodworking.com/)
