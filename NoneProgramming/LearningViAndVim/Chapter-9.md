# 9 Graphical Vim (gvim)
## 9.1 General Introduction to gvim
*gvim* brings all the functinaligy, power, and features of Vim while adding the
convenience and intuitive nature of a GUI enviroment.
### 9.1.1 Starting gvim
*gvim* reads and executes two startup files: `.vimrc` followed by `.gvimrc`.
It's better to define gvim-specific options in `.gvimrc`, this provides a nice
separation and also assures proper behavior on startup.

System administrators can use a system *gvimrc* file (usually in `$VIM/gvimrc`)
to set common options for their users.

*gvim* looks for configuration in the following order, and stops looking after
any one of these is found.
1. `exrc` command stored in `$GVIMINIT1` environment variable.
2. A user *gvimrc* file, usually `$HOME/.gvimrc`.
3. In a Windows environment, if `$HOME` is not set, *gvim* looks in
   `$VIM/_gvimrc`.
4. If `_gvimrc` is not found, *gvim* looks for `.gvimrc`.

### 9.1.2 Using the Mouse
The mouse behavior is controled by the `mouse` option in which you can add or
remove flags from it to enable or disable mouse functionality in three different
modes -- ex command mode, insert mode, and vi command mode.  
In all three modes, mouse can be used to reposition the cursor. However, you
click hold and slide to the top or bottom to sroll up and down in vi command
mode.

The flags controlling enable and disable mouse function in three modes:
- ex command mode: `c`
- Insert mode: `i`
- vi command mode: `n`

Any combination of flags can be specified in the `mouse` option.
```
" Disable all mouse behavior.
:set mouse=""

" Enable all mouse behavior (default)
:set mouse=a

" Enable visual mode. Uses the += syntax to add a flag to the current mouse
" setting.
:set mouse+=v

" Disable mouse behavior in ex command mode
:set mouse-=c
```

Other mouse options: `mousefocus`, `mousehide`, `mousemodel`, `selectmode`. For
the detail of each option, refer to the Vim built-in help.

Scroll wheel scrolls the window up and down, regardless of the value of `mouse`
option.

## 9.2 Customizing Scrollbars, Menus, and Toolbars
### 9.2.1 Scrollbars
Toggle the left and right scrollbar by including or excluding `l` and `r` in the
`guioption` option. `L` and `R` tell gvim to show left or right scrollbars only
when there is a vertically split window.  
`b` flag controls horizontal scrollbar.

### 9.2.2 Menus
In gvim, menu entries are merely wrappers for Vim commands

You can click the dashed line containing a scissor to create a self-standing
window of the menu.

#### Basic menu customization
gvim stores menu definitions in a file named `$VIMRUNTIME/menu.vim`.

```
:amenu File.HTML :set filetype
```

[Previous](Chapter-8.md) | [Contents](Contents.md) | [Next](Chapter-10.md)
