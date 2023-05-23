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

Editing environment variables in Linux Bash shell:
```bash
VARABC=somevalue VARXYZ=someothervalue MYVIMRC=/path/to/my/vimrc/file
export VARABC VARXYZ MYVIMRC
```

The value of exported variables can be set before or after exporting them.

The `-u` command-line option override *Vim*'s enviroment variables and goes directly to
the specified initialization file.

- `VIMINIT` and `EXINIT`: Specifies *ex* commands to execute when *Vim* starts. Define
  multiple commands by separating them with vertical bars (|). `EXINIT` is used when
  `VIMINIT` does not exist.
- `MYVIMRC`: The path of an initialization file, if the file exists, *Vim* takes initial
  settings from it and no other file is consulted.
- `SHELL`: Specifies which shell or **external command-line interpreter**  *Vim* uses.
- `VIMRUNTIME`: Points to *Vim* support files, such as online documentation, syntax
  definitions, and plug-in directories. Vim typically figures this out on its own. If you
  set the variable -- for example, in the `.bashrc` file -- it can cause errors if a newer
  version of Vim is installed because your personal *VIMRUNTIME* variable may point to an
  old, nonexistent, or invalid location.

## 8.3 New Motion Commands
- `count%`: Go to the line *count* percent into the file.
- `:go n` or `:n go`: Go to the *n*th byte in the buffer.

### 8.3.1 Visual Mode Motion
`v` toggles visual mode on and off. Here the following is some visual mode motion commands
in Vim:
- `n aw`, `n aW`: Select n words, whitespaces included. Lowercase `w` looks for
  punctuation-delimited words, whereas uppercase `W` looks for whitespace-delimited words.
- `n iw`, `n iW`: Select n words, do not include whitespaces.
- `as`, `is`: Add sentence, or inner sentence.
- `ap`, `ip`: Add paragraph, or inner paragraph.

## 8.4 Extented Regular Expressions
- `\|`: Indicates alternation. `a\|b` matches either `a` or `b`. `house|\home` matches
  either `house` or `home`.
- `\&`: Matches the part after the last `\&`, but only if all the previous parts matched.
- `\+`: Match **one** or more of the preceding regular expression.
- `\=`, `\?`: Match **zero** or **one** of the preceding regular expression.
- `\{...}`: Counted number of repetitions.
    - `\{n,m}`: Match *n* to *m* of the preceding regular expression. As much as possible.
    - `\{n}`, `\{-n}`: Match exactly *n* repetitions of the preceding regular expression.
    - `\{n,}`: Match **at least** n of the preceding regular expression. As much as
      possible.
    - `\{,m}`: Match zero to *m* of the preceding regular expression. As much as possible.
    - `\{}: Same as *.
    - `{-n,m}: Matches *n* to *m* of the preceding regular expression. As few as possible.
    - `{-n,}: Matches at least *n* of the preceding regular expression. As few as
      possible.
    - `{-,m}: Matches zero to *m* of the preceding regular expression. As few as possible.

## 8.5 Extented Undo
The `undolevels` option decides how many undos you can recall in a session.

`:redo` and `CTRL-R` redos the undos. `CTRL-R` accepts a numeric prefix to redo several
changes at once.

When all possible undos are undone, Vim resets the file's *modified* status.

## 8.6 Incremental Searching
`incsearch` option highlights the text that matches what you've typed so far

## 8.7 Left-Right Scrolling
`sidescroll` option controls how much to scroll the screen, and `wrap` controls whether
lines wrap or disappear off the edge of the screen. Thus, to get side scrolling, you'd use
`:set nowrap` and give `sidescroll` a reasonable value.

[Previous](../Part-I/Chapter-7.md) | [Contents](../Contents.md) | [Next](./Chapter-9.md)
