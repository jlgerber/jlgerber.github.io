
# Configuring Rez

Rez 2.0 has moved to python as a primary means of configuring its behavior. The default, supplied configuration script may be overridden ( see below ). Additionally, individual options may be overridden using environment variables, which may generally be constructed by uppercasing them and prefixing them with **REZ_**. One may assume that this is the case, even if the document fails to explicitly mention the name of the variable in the sections below.

# rezconfig.py & settings overrides

The base `rezconfig.py` file is part of the rez python package.

Settings for Rez are determined in the following way:
1. The setting is first read from the internal **rezconfig.py** file.
- The setting is then overridden if it is present in another settings file
   pointed at by the **$REZ_CONFIG_FILE** environment variable;
- The setting is further overriden if it is present in **$HOME/.rezconfig**
- The setting is overridden again if the environment variable **$REZ_XXX** is
   present, where XXX is the uppercase version of the setting key. For example,
   "image_viewer" will be overriden by $REZ_IMAGE_VIEWER. List values can be
   separated either with "," or blank space.
- This is a special case applied only during a package build or release. In
   this case, if the package definition file contains a "config" section,
   settings in this section will override all others.

Note that in the case of plugin settings (anything under the "plugins" section
of the config), (4) does not apply.

In other words, the order of precedence, from lowest to highest is:
- internal rezconfig.py
    - external rezconfig.py ( set via REZ_CONFIG_FILE env var)
        - $HOME/.rezconfig
            - Individual Environment variable ( REZ_ + uppercase property name)
                - During package build or release only - package.py config section settings

Variable expansion can be used in configuration settings. The following
expansions are supported:
- Any property of the system object: Eg "{system.platform}" (see system.py)
- Any environment variable: Eg "${HOME}"

The following variables are provided if you are using rezconfig.py files:
- 'rez_version': The current version of rez.

Paths should use the path separator appropriate for the operating system
(based on Python's os.path.sep).  So for Linux paths, / should be used. On
Windows \ (unescaped) should be used.

# rezconfig.py contents by section

## Paths

rezconfig.py defines a number of variables identifying resource locations and resource location search paths. Here are some important ones.

#### packages_path = [...]

Rez uses the `packages_path` key to define a list of places where it will look for packages at runtime. Paths earlier in the list have higher precedence than later ones; Rez will use the first package it finds  which satisfies its name and version specifications. ( At least according to the docs. Question: if a package earlier in the search paths does not have the required variant, and one later does, will the later package be used? Test. )

The `packages_path` key may be overridden in the environment using the **REZ_PACKAGES_PATH** environment variable, separating paths with spaces or commas.

In a vanilla install, the packages_path list is populated with 3 paths:

- ~/packages
    - locally installed packages, not yet deployed
- ~/.rez/packges/int
    - internally developed packages, deployed
    - should be replaced with a central location)
- ~/.rez/packages/ext
    - external (3rd party) packages, such as houdini or boost
    - should be redefined to point to a central location

#### local_packages_path = "~/packages"

This is the path that Rez will locally install packages to when `rez-build` is run. In a vanilla install, it is defined as `~/packages`.

At DD, we can define this as `${JS_CS}/packages `, which points to the work.user directory at the level last defined via `go`.

#### release_packages_path = "~/.rez/packages/int"

The path that Rez will deploy packages to when `rez-release` is used. In a vanilla install, this points to `~/.rez/packages/int`. However, in a production install, the comments suggest pointing this towards a central repo.

At DD, we have a number of repos. Since packages support explicit platform and os semantics in the form of variants,  `/tools/packages` ( which is symlinked to /dd/<os>/tools/packages) could be simplified to an actual `/dd/packages` mountpoint.

#### tmpdir = None

Where temporary files go. Defaults to appropriate path depending on the system. For example, *nix distributions will probably set this to /tmp. It is highly recommended that this be set to local storage, such as /tmp. In order to take advantage of default behavior, leave the setting at its default, which is `None`.

#### context_tmpdir = None

Where temporary files for contexts go. Defaults to os appropriate location. This is distinct from `tmpdir`, as it is probably desirable to set this to an NFS location.

## Extensions

Rez provides a number of different extension opportunities.

#### plugin_path = []

This is the search path for plugins. It defaults to an empty list, as Rez ships with a number of predefined plugins. However, one may add a list of locations to search for plugins.

Furthermore, one may add an environment variable - `REZ_PLUGIN_PATH`, and provide a space separated list of locations as well.

#### bind_module_path = []

This is the search path for bind modules. It also defaults to an empty list, as Rez ships with a number of bind modules. However, one may extend this just like the plugin path.

## Caching

#### resolve_caching = True

Use available caching mechanisms to speed up resolves when applicable.

#### cache_package_files = True

Cache package file reads

#### cache_listdir = True

Cache directory traversals

#### resource_caching_maxsize = -1

The size of the local (in-process) resource cache. This number refers to count, not bytesize. Resources including package families, packages, and variants. Valid settings:

- 0  -  disable caching
- -1 - unlimited cache size
- \# - the max number of entries

#### memcached_uri = []

Uris of running memcahced server(s) to use as file and resolve cache. For example, "127.0.0.1:11211" points to a memcached running on localhost on its default port. Must be either an empty list of a list of strings.

#### memcached_package_file_min_compress_len = 16384

Bytecount beyond which memcached entries are compressed, for cached package files. (such as package.yaml, package.py ) Zero means never compress.

#### memcached_context_file_min_compress_len = 1

Bytecount beyond which memcached entries are compressed, for cached context files ( aka .rxt files). Zero means never compress.

#### memcached_listdir_min_compress_len = 16384

Bytecount beyond which memcached entries are compressed, for directory listings. zero means never compress.

#### memcached_resolve_min_compress_len = 1

Bytecount beyond which memcached entries are compressed, for resolves. Zero means never compress.

## Package Resolution

#### implicit_packages = [...]

Packages that are implicitly added to all package resolves, unless the **--no--implicit** flag is used.

The default list of implicit packages is as follows:

```
implicit_packages = [
    "~platform=={system.platform}",
    "~arch=={system.arch}",
    "~os=={system.os}",
]
```

#### prune_failed_graph = True

If true, then when a resolve graph is generated during a failed solve, packages unrelated to the failure are purned from the graph. An "unrelated" package is one that is not a dependency ancestor of any packages directly involved in the failure.

#### variant_select_mode = "version_priority"

This determines which variants in a package are preferred during a solve. Valid options are:

- version_priority - prefer variants that contain higher versions of packages present in the request
- intersection_priority - prefer variants that contain the must number of packages that are present in the request

#### package_filter = None

One or more filters may be listed, each with a list of exclusion and inclusion rues. These filters are applied to each package during a resolve, and if any filter excludes a package, that package is not included in the resolve. For example:

```
package_filter:
    excludes:
        - glob(*.beta)
    includes:
        - glob(foo-*)
```

This is an example of a single filter with one exclusion rule and one inclusion rule. The filter will ignore all packages with versions ending in '.beta', except for package **foo** ( which it will accept all versions of ). A filter will only exclude a package iff that package matches at least one exclusion rule, and does not match any inclusion rule.

Here is another exmple , which excludes all beta packages, and all packages except 'foo' that are released after a certain date. Note that in order to use multiple filters, you need to supply a list of dicts, rather than just a dict:

```
package_filter:
- excludes:
    - glob(*.beta)
- excludes:
    - after(1429830188)
    includes:
    - foo # same as range(foo), or glob(foo-*)
```
This example shows why multiple filters are supported - whit only one filter, it would not be possible to exclude all beta packages (includng foo), but also exclude all packages after a certain date, except for foo.

QUESTION - the filters appear to be yaml. Now that the filter is a python file, how does this change? Is the documentation just not up to date? What about the environment variable override form?

More rule examples:
```
glob(*.beta)          Matches packages matching the glob pattern.
regex(.*-\\.beta)     Matches packages matching re-style regex.
requirement(foo-5+)   Matches packages within the given requirement.
before(1429830188)    Matches packages released before the given date.
after(1429830188)     Matches packages released after the given date.
*.beta                Same as glob(*.beta)
foo-5+                Same as range(foo-5+)
```

## Environment Resolution

Rez's default behavior is to overwrite variables on first reference. This prevents unconfigured software from being used within the resolved environment. For exmample, if PYTHONPATH were to be appended to and not overwritten, then python modules from the parent environment would be (incorrectly) accessible within the Rez environment. Rez provides the notion of **parent variables** to override this behavior. Parent variables are appended/prepended to, rather than being overridden.

#### parent_variables = []

This is a list of variables to treat as parent variables. Parent variables will not be overwritten when a process is initialized by Rez. It should be noted that if one makes variables such as PATH, PYTHONPATH, or app plugin paths parent variables, one may experience incorrect behavior within a resolved environment.

This setting is ignored if the following setting is set to true ( all_parent_variables.)

#### all_parent_variables = False

If set to true, Rez appends/prepends existing environment variables when a new process is initialized, as opposed to replacing them. This setting should be left at False under normal circumstances, as problems are likely to arise when inheriting environment variables.

#### default_shell = ""

The default shell type to use when creating resolved environments ( eg wne using rez-env or calling ResolvedContext.execute_shell). If empty or null, the current shell is used (eg "bash")

#### terminal_emulator_command = None

The command to use to launch a new Rez environment in a separate terminal ( this is enabled using rez-env's "detached" option). If None, it is detected.

#### new_session_popen_args = None

subprocess.Popen arguments to use in order to execute a shell in a new process group. ( See ResolvedContext.execute_shell, 'start_new_session') Dict with {Popen Arg name: value,...}

#### env_var_separators = {...}

Dict which can be used to override the separator used for environment variables that represent a list of items. By default, the value of os.pathsep will be used, unless the environment variable is list here, in which case the configured separator will be used.

```
env_var_separators = {
    "CMAKE_MODULE_PATH": ";",
    "DOXYGEN_TAGFILES": " ",
}
```

#### suite_visiblity = "always"

Defines what suites on $PATH stay visible when a new rez environment is resolved.

Possible values are:
- "never" - Don't attempt to keep any suites visible in a new env
- "always" - Keep suites visible in any new env
- "parent" - Keep only the parent suite of a tool visible
- "parent_priority" - keep all suites visible and the parent takes precedence

#### rez_tools_visibility = "append"

Defines how Rez's command line tools are added back to PATH within a resovled environment. Valid values are:
- "append" - Rez tools are appended to PATH (default)
- "prepend" - Rez tools are prepended to PATH
- "never" - Rez tools are not added back to PATH - Rez will not be available within resolved shells in this case.

#### package_commands_sourced_first = True

Defines when package commands are sourced during the startup sequence of an interactive shell. If True, package commands rare sourced before startup scripts ( such as .bashrc ). If False, package commands are sourced after.

## Debugging

#### warn_shell_startup = False

If true, print warnings associated with shell startup sequence, when using tools such as rez-env. For example, if the target shell type is "sh", and the "rcfile" param is used, you would get a warning, because the sh shell does not support rcfile.

#### warn_untimestamped = False

If true, print a warning when an untimestamped package is found.

#### warn_all = False

Turn on all warnings

#### warn_none = False

Turn off all warnings. This overrides warn_all.

#### debug_file_loads = False

Print info whenever a file is loaded from disk.

#### debug_plugins = False
Print debugging info when loading plugins

#### debug_package_release = False
Print debugging info such as VCS commands during package release

#### debug_bind_modules = False

Print debugging info in binding modules. Binding modules should print using
the bind_utils.log() function - it is controlled with this setting

#### debug_resources = False

Print debugging info when searching and loading resources.

#### debug_package_exclusions = False

Print packages that are excluded from the resolve, and the filter rule responsible.

#### debug_resolve_memcache = False

Print debugging info related to use of memcached during a resolve

#### debug_memcache = False

Debug memcache usage. As well as printing debugging info to stdout,it also
sends human-readable strings as memcached keys (that you can read by running
"memcached -vv" as the server)

#### debug_all = False
Turn on all debugging messages

#### debug_none = False
Turn off all debugging messages. This overrides debug_all.

#### catch_rex_errors = True

When an error is encountered in rex code, rez catches the error and processes
it, removing internal info (such as the stacktrace inside rez itself) that is
generally not interesting to the package author. If set to False, rex errors
are left uncaught, which can be useful for debugging purposes.

## Build

#### build_directory = "build"

The default working directory for a package build, relative to the package source directory ( this is typically where temporary build files are written).

#### build_thread_count = "physical_cores"
The number of threads a build system should use, eg the make '-j' options. If the string values "logical_cores" or "physical_cores" are set, Rez will attempt to detect the number of logical or physical cores on the host system.

Note: "logical cores" are the number of cores reported to the OS, whereas "physical_cores" are the number of actual hardware processor cores. They may differ if, for example, the CPU's support hyperthreading, which case logical_cores == 2* physical_cores.

This setting is exposed as the environment variable `$REZ_BUILD_THREAD_COUNT` during builds.

## Release

#### release_hooks = []
The release hooks to run when a release occurs. Release hooks are plugins. If a plugin listed here is not present, a warning message is printed. Note that a release hook plugin being loaded does not mean it will run - it needs to be listed here as well. Several built-in release hooks are available. See **rezplugins/release_hook**.

## Suites

#### suite_alias_prefix_char = "+"
The prefix character used to pass rez-specific commandline arguments to alias scripts in a suite. This must be a character other than "-", so that it doesn't clash with the wrapped tools own commandline arguments.

## Appearance

#### quiet = False
Suppress all extraneous output - warnings, debug messages, progress indicators and so on. Overrides all warm_xxx and debug_xxx settings.

#### show_progress = True
Show progress bars where applicable.

#### editor = None
The editor used to get user input in some cases.
On osx, set this to "open -a <your-app>" if you want to use a specific app.

#### image_viewer = None
The program used to view images by tools such as "rez-context -g"
On osx, set this to "open -a <your-app>" if you want to use a specific app.

#### browser = None
The browser used to view documentation; the rez-help tool uses this
On osx, set this to "open -a <your-app>" if you want to use a specific app.

#### difftool = None
The viewer used to view file diffs. On osx, set this to "open -a <your-app>"
if you want to use a specific app.

#### dot_image_format = "png"
The default image format that dot-graphs are rendered to.

#### set_prompt = True
If true, tools such as rez-env will update the prompt when moving into a new
resolved shell. Prompt nerds might do fancy things with their prompt that Rez
can't deal with (but it can deal with a lot - colors etc - so try it first).
By setting this to false, Rez will not change the prompt. Instead, you will
probably want to set it yourself in your startup script (.bashrc etc). You will
probably want to use the environment variable $REZ_ENV_PROMPT, which contains
the set of characters that are normally prefixed/suffixed to the prompt, ie
'>', '>>' etc.

#### prefix_prompt = True
If true, prefixes the prompt, suffixes if false. Ignored if 'set_prompt' is
false.

## Misc

#### max_package_changelog_chars = 65536
If not zero, truncates all package changelog entries to this maximum length.
You should set this value - changelogs can theoretically be very large, and
this adversely impacts package load times.

#### rxt_as_yaml = True
If this is true, rxt files are written in yaml format. If false, they are
written in json, which is a LOT faster. You would only set to true for
backwards compatibility reasons. Note that rez will detect either format on
rxt file load.

Note for DD: we should set this to False ( ie we want JSON)

## Colorization
The following settings provide styling information for output to the console,
and is based on the capabilities of the Colorama module
(https://pypi.python.org/pypi/colorama).

*_fore and *_back colors are based on the colors supported by this module and
the console. One or more styles can be applied using the *_styles
configuration. These settings will also affect the logger used by rez.

At the time of writing, valid values are:
- fore/back:
    - black
    - red
    - green
    - yellow
    - blue
    - magenta
    - cyan
    - white
- style:
    - dim
    - normal
    - bright

#### color_enabled = (os.name == "posix")
Enables / disables colorization globally
Note: Turned off for Windows currently as there seems to be a problem with the Colorama module on windows.

Color configuration:
```

#------------------------------------------------------------------------------
# Logging colors
#------------------------------------------------------------------------------
critical_fore = "red"
critical_back = None
critical_styles = ["bright"]

error_fore = "red"
error_back = None
error_styles = None

warning_fore = "yellow"
warning_back = None
warning_styles = None

info_fore = None
info_back = None
info_styles = None

debug_fore = "blue"
debug_back = None
debug_styles = None

#------------------------------------------------------------------------------
# Context-sensitive colors
#------------------------------------------------------------------------------
# Heading
heading_fore = None
heading_back = None
heading_styles = ["bright"]

# Local packages
local_fore = "green"
local_back = None
local_styles = None

# Implicit packages
implicit_fore = "cyan"
implicit_back = None
implicit_styles = None

# Tool aliases in suites
alias_fore = "cyan"
alias_back = None
alias_styles = None
```
## Plugin Settings
Settings specific to certain plugin implementations can be found in the rezconfig file accompanying that plugin. The settings listed here are common to all plugins of a particular type
```

plugins = {
    "release_vcs": {
        # Format string used to determine the VCS tag name when releasing. This
        # will be formatted using the package being released - any package
        # attribute can be referenced in this string, eg "{name}".
        #
        # It is not recommended to write only '{version}' to the tag. This will
        # cause problems if you ever store multiple packages within a single
        # repository - versions will clash and this will cause several problems.
        "tag_name": "{qualified_name}",

        # A list of branches that a user is allowed to rez-release from. This
        # can be used to block releases from development or feature branches,
        # and support a workflow such as "gitflow".  Each branch name should be
        # a regular expression that can be used with re.match(), for example
        # "^master$".
        "releasable_branches": [],

        # If True, a release will be cancelled if the repository has already been
        # tagged at the current package's version. Generally this is not needed,
        # because Rez won't re-release over the top of an already-released
        # package anyway (or more specifically, an already-released variant).
        #
        # However, it is useful to set this to True when packages are being
        # released in a multi-site scenario. Site A may have released package
        # foo-1.4, and for whatever reason this package hasn't been released at
        # site B. Site B may then make some changes to the foo project, and then
        # attempt to release a foo-1.4 that is now different to site A's foo-1.4.
        # By setting this check to True, this situation can be avoided (assuming
        # that both sites are sharing the same code repository).
        #
        # Bear in mind that even in the above scenario, there are still cases
        # where you may NOT want to check the tag. For example, an automated
        # service may be running that detects when a package is released at
        # site A, which then checks out the code at site B, and performs a
        # release there. In this case we know that the package is already released
        # at A, but that's ok because the package hasn't changed and we just want
        # to release it at B also. For this reason, you can set tag checking to
        # False both in the API and via an option on the rez-release tool.
        "check_tag": False
    }
}
```

## GUI

All of the settings listed here onwards apply only to the GUIs available in the "rezgui" part of the module.


#### use_pyside = False
#### use_pyqt = False
Setting either of these options to true will force rez to select that qt
binding. If both are false, the qt binding is detected. Setting both to true
will cause an error.
