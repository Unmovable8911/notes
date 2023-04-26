# Part I vi and Vim Fundamentals
## 1 Introducing vi and Vim
- The `ex` *line editor* is the underlying editor of *vi*
- Your edit is stored in a *buffer* lives in the memory before you save your file
- Discard all changes and reloads the last saved version of the file `:e ENTER`
- `vim file` or `vi file`: opens a file
- `:q`; `:wq`/`ZZ`; `:q!`: quit; save and quit; quit without saving
## 2 Simple editing
### 2.1 Moving around
| Keystroke        | Action                | Keystroke        | Action                |
| :---:            | ---                   | :---:            | ---                   |
| j / `CTRL-N` / - | move down             | k / `CTRL-P` / + | move up               |
| h / `BACKSPACE`  | move left             | l / `SPACE`      | move right            |
| w                | move one word forward | W                | ignore punctuations   |
| b                | move one word backward | B               | ignore punctuations   |
| e                | move to the end of the word | E          | ignore punctuations   |
| G                | Goes to last line     | gg               | go to the first line  |
| 0 (zero)         | beginning of the line | $                | end of the line       |

You can combine commands with *numbers* to repeate command *number* of times. For example,
if you type `5j`, the cursor will move five lines down.
### 2.2 Simple edits
- `i`: insert text before the cursor  
    `I`: Insert text before the first character of the current line  
- `a`: append text after the cursor  
    `A`: Append text at the end of the line
- `x`: delete the current character  
    `X`: delete the character before the cursor
- `c`: change text  
    `cc`: change the current entire line  
    `C`: Change the from the cursor to the end of the line
- `s`: substitute, delete the current character and starts *insert* mode  
    `S`: Substitute the current entire line
- `d`: delete text  
    `dd`: delete the current line  
    `D`: delete to the end of the line
- `r`: replace the current character  
    `R`: Replace text, start from the current character
- `p`: put the deleted or yanked text after the cursor,   
    `P`: Put the deleted or yanked text before the cursor  
    *if the text is an entire line, put the text under (or above) the current line, no*
    *matter the column of the cursor*
- `y`: yank (copy) text  
    `Y`: yank the current line  
    `yy`: equal to `Y`
- `~`: change case, for example, change capital letter to lower case letter, vice versa.
- `u`: undo. **vi** can only undo the last command, while *Vim* can undo infinite number
    of previous commands  
    `U`: Undo changes made to the current line
- `o`: open a new line beneath the current line and starts *insert* mode  
    `O`: Open a new line above the current line and starts *insert* mode
- `J`: Join two lines into one line
- `.`: d~~ot~~uplicate command, repeat the previous command
- `xp`: command combination, transposing two letters
- `+`: first character of the next line  
- `-`: first character of the previous line

In *Vim*, you can redo the undos with `CTRL-R`.

Except for `o` and `O`, the *insert* commands listed above can take *numeric* prefixes.

Some *number*, *command* combinations you may not expect:  
- `3i*`: insert three asterisks (`***`)
- `5A-`: Append 5 hyphens (`-----`) at the end of the line
- `4r=`: replace 4 characters, starts from the current one, with 4 equal signs (`====`)
### 2.3 General form of *vi* commands
*vi* and *Vim* commands consists of three parts:  
*\[command\]\[number\]\<text object\>* or *\[number\]\[command\]\<text object\>*

*command* and *number* are optional, without them, you are just moving around in the file
by *text object*.

Some *commands* don't require *text object*. For example, `r`, `i`, `~`, etc. But you can
combine them with *numbers* to repeat the command multiple times by prefixing a *number*
before them .
### Settings
- `wrapmargin` or `wm` option can automatically wrap a line when its length reaches the
    value you specified.Usage:  
    `:set wm=100`, `:set nowm`, `:set nowrapmargin`
- `number` or `nu` option can add a line number before a line. Usage:   
    `:set number`, `:set nu`, `:set nonumber`
## 3 Moving around in a hurry
### 3.1 Movement by screens
#### 3.1.1 Scrolling the screen
- `CTRL-F`: Scroll *forward* one screen
- `CTRL-B`: scroll *backward* one screen
- `CTRL-D`: scroll *down* half screen
- `CTRL-U`: scroll *up* half screen
- `CTRL-Y`: scroll down one line
- `CTRL-E`: scroll up one line

`CTRL-Y` and `CTRL-E` do not send the cursor to the beginning of the line
#### 3.1.2 Repositioning the screen with `z`
- `z-ENTER`: Move the current line to the top of the screen
- `z.`: Move the current line to the center of the screen
- `z-`: Move the current line to the bottom of the screen

`[number]z-ENTER` will move *number*th line to the top of the screen, similar with `z-`.

**Note** | Some GNU/Linux distributions come with an /etc/vimrc file tha sets `scrolloff`
to a nonezero value, which will provide that number of lines above or below the cursor
when you use `z-ENTER` or `z-`. However, you can set that value to zero in you personal
`.vimrc` file in your `home` directory.
#### 3.1.3 Redrawing the screen
You can redraw the screen with `CTRL-L`, you will find this usefull if some system
messages appears on the screen.For example, when you are working with a remote server.
#### 3.1.4 Movement within a screen
- `H`: move to *Home* -- the first character of the first line on the screen
- `M`: move to *middle* -- the first character of the middle line on the screen
- `L`: move to *last* -- the first character of the last line on the screen
- `nH`: move to the first character of the *nth* line below the top line
- `nL`: move to the first character of the *nth* line above the last line
#### 3.1.5 Movement within the current line
- `^`: move to the first noneblank character of the current line
- `n|`: move to the *nth* column of the current line
### 3.2 Movement by text blocks
- `(`: beginning of the *current* sentence
- `)`: beginning of the *next* sentence
- `{`: beginning of the *current* paragraph
- `}`: beginning of the *next* paragraph
- `[[`: beginning of the *current* section
- `]]`: beginning of the *next* section

In *vi*, the *end of a sentence* is indicated by a `?`, `.`, or `!` and following **two
spaces** or a line break (`ENTER`). However, *Vim* only looks for these punctuations
followed by **a single space** or a line break indeed.

A paragraph is defined as text up to the next *blank line*, or up to one of the default
**paragraph macros** (`.IP`, `.PP`, `.LP`, or `.QP`) from the *`troff` MS macro package*.

A *section* is defined as text up to the next default **section macro** (`.NH`, `SH`,
`.H1`, or `.HU`).
### 3.3 Movement by searches
- `/pattern`: search for the next occurrence of *pattern*
- `?pattern`: search for the previous occurrence of *pattern*
#### 3.3.1 Repeating searches
- `n`: repeat the search in the same direction
- `N`: repeat the search in the opposite direction
- `/-ENTER`: repeat the search forward
- `?-ENTER`: repeat the search backward

When your search reaches the first or last match of *pattern*, if you continue to search
in the same direction, *vi* or *Vim* wrap up to the last or first match of *pattern* if
`wrapscan` is set, and by default, it is. Otherwise, if `nowrapscan` is set, the search
stops and a warning message appears in the status line, which goes something like this:
> `Address search hit BOTTOM without matching pattern`

#### 3.3.2 Changing through search
`/pattern` and `?pattern` can work as *text objects* which you can combine them with
*commands*. For example, with `d` and `c`.
#### 3.3.3 Current line searches
- `fx`: find and move to the *next* occurrence of *x* in the line, where *x* strands for
    any *character*
- `Fx`: Find and move to the previous occurrence of *x* in the line
- `tx`: to the column before the next occurrence of *x*
- `Tx`: To the column after the previous occurrence of *x*
- `;`: repeat the find (or to) command in the same direction
- `,`: repeat the find (or to) command in the opposite direction
### 3.4 Movement by line number
`CTRL-G` causes *vi* to display **the current line number**, **the total number of lines
in the file**, and **the percentage of the current line number to the total number of
lines in the file**.  
*Vim* displays more information than *vi* does, for example, the current column.

`nG` takes you to the beginning of *nth* line.  
Two back ticks (\`\`) returns you to your original position, the position where you issued
the last `G` command.
