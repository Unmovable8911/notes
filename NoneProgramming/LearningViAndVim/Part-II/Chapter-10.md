# 10 Multiple Windows in Vim
## 10.1 Initiating Multiwindow Editing
Open files with horizontally split windows. 

```bash
$ vim -o[n] file1 [file2] [file3] [...]
```

If the optional *n* is provided, Vim will create *n* of windows. If Vim cannot
split the screen into enough windows, the first files provided as aguments will
get the windows, while the remaining files are loaded into buffers not visible.

The `laststatus` option controls when the last window will have a status line,
it takes a integer value from '0' to '2': 
- '0'--never
- '1'--only if there are multiple windows
- '2'--always.

`winheight` and `winwidth` options define the minimal window size of the
**current** window. In practice, that means, the current window would expand to
the specified value of `winheight` and `winwidth` if its original size is
smaller than the values.

## 10.2 Opening Windows
The `:split` and `:vsplit` command:

```
:[n]split [++opt] [+cmd] [file]
:[n]vsplit [++opt] [+cmd] [file]
```

`:split` command opens a new horizontally split window while `:vsplit` opens a
vertically split window. The optional arguments has the following purpose:
- *n*: Tells Vim how many lines the new window displays.
- *opt*: Passes Vim option information to the new window. Must be preceeded by
  two plus sign.
- *cmd*: The command to execute in the new window. Must be preceeded by a plus
  sign.
- *file*: The file to open in the new window. If this omited, Vim will open the
  currently editing file in the new window.

> The `equalalways` option causes Vim to always create split windows in the same
> size. Add `eadirection` option and assign it a value of `hor`, `ver`, or
> `both` (horizontal, vertical, or both) to control which direction splits
> equally.

In addition to `:split`, the `:new` command (with its sibling `:vnew`) also
opens a new split window, it also takes the same arguments as `:split` does. The
difference is that `:new` automatically execute `WinLeave`, `WinEnter`,
`BufLeave`, and `BufEnter`. 

Following are two horizontal split commands without vertical sibling:

```
" opens the split window in read-only mode
:sview filename

" works like ':split', except that if 'filename' does not exist, Vim will not
" split the window.
:sfind [++opt] [+cmd] filename
```

## 10.3 Moving Around Windows
- `CTRL-W``CTRL-W`: Cycle through all the windows.


[Previous](./Chapter-9.md) | [Contents](../Contents.md) |
[Next](./Chapter-11.md)
