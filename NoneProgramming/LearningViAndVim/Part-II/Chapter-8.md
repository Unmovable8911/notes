# Part II Vim
# 8 Overview and Improvements Over *vi*
## 8.1 Buit-In Help
`:help` invokes *Vim*'s built-in help documentation, `:help` followed by a keyword shows
the help documentation of the keyword. `:help` command supports autocompletion when you
hit `TAB` key.

## 8.2 Setup and Initialization Options
### 8.2.1 Command-Line Options
Single letter options begin with `-`, and word-length options begin with `--`.

A command-line argument of two hyphens by themselves tells *Vim* that the rest of the
command line contains no options.

A partial list of Vim command-line options:
- `-b`: Edit in binary mode.
- `-c command`: Same with *vi*, but *Vim* allows up to 10 `-c` instances.
- `-C`: Run *Vim* in compatible mode.
- `-d`: Uses the native file difference program `diff` to make inspection of file
  differences.
- `-E`: Start in improved *ex* mode.
- `-F` or `-A`: Farsi or Arabic modes, respectively.
- `-g`: Start *gvim* (GUI).
- `-M`: Turn off write mode, you can't write the buffer even using `:w!`.
- `o[n]`: Open files in seperate windows. If the optional `n` is provided, *Vim* will open
  *n* of windows.
- `-O[n]`: Like `-o`, except this split windows vertically.
- `-y`: Easy mode
- `-Z`: Restricted mode. Turn off all external interfaces.

### 8.2.2 Behaviors Associated to Command Name
- `gvim`: Start *Vim* in graphical mode.
- `view`, `gview`: Start *vim* or *gvim* in read-only mode.
- `rvim`, `rview`, `rgview`, `rgvim`: Restricted mode
- `evim`, `eview`: Easy vim.
- `vimdiff`, `gvimdiff`: Same as starting *Vim* with a `-d` option.

### 8.2.3 System and User Configuration Files
*Vim* looks for initialization cues in the following list, the first element *Vim*
encounters is the only element it executes.
- `VIMINIT`: This is an environment variable. If it's nonempty, *Vim* executes its content
  as an *ex* command.
- User *vimrc* file: On Unix and MacOS, the path of this file is `$HOME/.vimrc`. On
  MS-Windows system, the path of this file is `$HOME/_vimrc` or `$VIM/_vimrc`.
- Local *exrc* or *exrc* file: If `exrc` option is set, *Vim* looks for three additional
  configuration files: `.vimrc`, `.gvimrc` and `.exrc`.

In a *vimrc* file, comments starts with a double quote (").

### 8.2.4 Enviroment Variables
Many environment variables affect *Vim*'s startup behavior and even some editing session
behavior.

[Previous](../Part-I/Chapter-7.md) | [Contents](../Contents.md) | [Next](./Chapter-9.md)
```
:set textwidth=90
```
