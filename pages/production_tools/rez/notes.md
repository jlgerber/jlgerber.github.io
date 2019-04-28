# rez 2.0 notes

## REX

rex is a plugin system for actions exposed in the commands() section of the **package.py** file of a package.

## ResolvedContext

The main Rez entry point for creating, saving, loading and executing
resolved environments is the ResolvedContex class. A ResolvedContext object can be saved to file and
loaded at a later date, and it can reconstruct the equivalent environment
at that time. It can spawn interactive and non-interactive shells, in any
supported shell plugin type, such as bash and tcsh. It can also run a
command within a configured python namespace, without spawning a child
shell.

## rezplugins

### Types
- build_process
    - local.py
    - remote.py
- build_system
    - bez.py
    - cmake.py
    - cmake_files
        - Colorize.cmake
        - FindStaticLibs.cmake
        - InstallDirs.cmake
        - InstallFiles.cmake
        - InstallPython.cmake
        - RezBuild.cmake
        - RezFindPackages.cmake
        - RezInstallCMake.cmake
        - RezInstallContext.cmake
        - RezInstallDoxygen.cmake
        - RezInstallPython.cmake
        - RezInstallWrappers.cmake
        - RezPipInstall.cmake
        - RezProject.cmake
        - RezRepository.cmake
        - Utils.cmake
    - make.py
    - rezconfig
    - rezconfig.py
    - template_files
        - Doxyfile
- package_repository
    - filesystem.py
    - memory.py
    - rezconfig
- release_hook
    - amqp.py
    - command.py
    - emailer.py
    - rezconfig
- release_vcs
    - git.py
    - hg.py
    - rezconfig
    - stub.py
    - svn.py
- shell
    - bash.py
    - .html.py
    - csh.py
    - rezconfig
    - sh.py
    - tcsh.py

TODO: figure out how to extend rez with a particular plugin. What does an additional plugin look like?

would it be like this?
```
build_process/
    __init__.py
    newprocess.py
```
