Python<span class="tag" data-tag-name="python"></span>
======================================================

### Tools

#### vprof<span class="tag" data-tag-name="python"></span><span class="tag" data-tag-name="profiler"></span><span class="tag" data-tag-name="debugging"></span><span class="tag" data-tag-name="debugger"></span>

profiler which allows you to explore performance of a python script
providing insights into exection.

##### [github vprof](https://github.com/nvdv/vprof)<span class="tag" data-tag-name="link"></span>

Usage: vprof -c &lt;modes&gt; -s &lt;test~program~&gt; vprof -c cmh -s
“testscript.py –foo –bar” vprof -c cm -s testscript.py

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
