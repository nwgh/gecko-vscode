# gecko-vscode
My Visual Studio Code config for hacking gecko.

This repository provides a few things that make hacking gecko in Visual Studio Code a bit easier (IMHO). **NB: This is designed assuming a multi-root workspace. Example is available (see below).**

## A Build Script
This is a "smart" build script (`build`) that does a few things:
 * Chooses whether to do a full build or a binaries build based on (1) what the last build type performed is, (2) whether or not the build was successful, (3) whether or not you appear to have switched branches/bookmarks since the last build.
 * Refreshes the compile_commands.json for your repo after a successful build (so that VS Code's C/C++ extension can have all the right info).
 * Tries to make this all work on windows, too, in conjunction with `mach_wrapper_win`.
 * On windows only, also creates a full Visual Studio build backend so you can use the full Visual Studio if you like.

## A C/C++ Extension Configuration
This is in `c_cpp_properties.json` and does its best to make code completion work on all the platforms. This depends on having the compile_commands.json created by the build script (or `mach build-backend -B CompileDB`) available.
To make use of this, `cd` into your `${TOPSRCDIR}/.vscode` and `ln -s /path/to/gecko-vscode/c_cpp_properties.json .`

## Some Extra VS Code Configuration
This lives in `.vscode` just like it would for any normal project, and includes:
 * Launch/debug configurations for Mac, Linux, and Windows (`.vscode/launch.json`)
 * Build configurations to be able to use the smart build script with VS Code's usual build keybindings (`.vscode/tasks.json`)
 
## An Example Multi-Root Workspace Setup
This is `example-code-workspace` and shows how you might want to set things up. Here's what I do:
 * `cd ~/src`
 * `git clone https://github.com/nwgh/gecko-vscode vscode-gecko` (name swapped so my tab completion works better)
 * `git clone https://gitlab.com/nwgh/gecko` (this is a git-cinnabar clone where I keep all my private WIP branches. See [the git-cinnabar documentation](https://github.com/glandium/git-cinnabar/wiki) for more info on git-cinnabar.)
 * `cp vscode-gecko/example-code-workspace gecko.code-workspace` (note the difference in `-` versus `.` before `code-workspace` - this is important!)
 * `vi gecko.code-workspace` (edit to make sure all the paths are right for your checkouts)
 * `code gecko.code-workspace`

**NB: If you want this all to work, do not change the values of the `"name"` keys in the `"folders"` section. You are, however, free to change the values of the `"path"` keys to suit your particular directory layout.**
