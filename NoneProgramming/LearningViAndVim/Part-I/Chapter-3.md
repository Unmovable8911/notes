# 3 Moving around in a hurry
## 3.1 Movement by screens
### 3.1.1 Scrolling the screen
- `CTRL-F`: Scroll *forward* one screen
- `CTRL-B`: scroll *backward* one screen
- `CTRL-D`: scroll *down* half screen
- `CTRL-U`: scroll *up* half screen
- `CTRL-Y`: scroll down one line
- `CTRL-E`: scroll up one line

`CTRL-Y` and `CTRL-E` do not send the cursor to the beginning of the line
### 3.1.2 Repositioning the screen with `z`
- `z-ENTER`: Move the current line to the top of the screen
- `z.`: Move the current line to the center of the screen
- `z-`: Move the current line to the bottom of the screen

`[number]z-ENTER` will move *number*th line to the top of the screen, similar with `z-`.

**Note** | Some GNU/Linux distributions come with an /etc/vimrc file tha sets `scrolloff`
to a nonezero value, which will provide that number of lines above or below the cursor
when you use `z-ENTER` or `z-`. However, you can set that value to zero in you personal
`.vimrc` file in your `home` directory.
### 3.1.3 Redrawing the screen
You can redraw the screen with `CTRL-L`, you will find this usefull if some system
messages appears on the screen.For example, when you are working with a remote server.
### 3.1.4 Movement within a screen
- `H`: move to *Home* -- the first character of the first line on the screen
- `M`: move to *middle* -- the first character of the middle line on the screen
- `L`: move to *last* -- the first character of the last line on the screen
- `nH`: move to the first character of the *nth* line below the top line
- `nL`: move to the first character of the *nth* line above the last line
### 3.1.5 Movement within the current line
- `^`: move to the first noneblank character of the current line
- `n|`: move to the *nth* column of the current line
## 3.2 Movement by text blocks
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
## 3.3 Movement by searches
- `/pattern`: search for the next occurrence of *pattern*
- `?pattern`: search for the previous occurrence of *pattern*
### 3.3.1 Repeating searches
- `n`: repeat the search in the same direction
- `N`: repeat the search in the opposite direction
- `/-ENTER`: repeat the search forward
- `?-ENTER`: repeat the search backward

When your search reaches the first or last match of *pattern*, if you continue to search
in the same direction, *vi* or *Vim* wrap up to the last or first match of *pattern* if
`wrapscan` is set, and by default, it is. Otherwise, if `nowrapscan` is set, the search
stops and a warning message appears in the status line, which goes something like this:
> Address search hit BOTTOM without matching pattern

### 3.3.2 Changing through search
`/pattern` and `?pattern` can work as *text objects* which you can combine them with
*commands*. For example, with `d` and `c`.
### 3.3.3 Current line searches
- `fx`: find and move to the *next* occurrence of *x* in the line, where *x* strands for
    any *character*
- `Fx`: Find and move to the previous occurrence of *x* in the line
- `tx`: to the column before the next occurrence of *x*
- `Tx`: To the column after the previous occurrence of *x*
- `;`: repeat the find (or to) command in the same direction
- `,`: repeat the find (or to) command in the opposite direction
## 3.4 Movement by line number
`CTRL-G` causes *vi* to display **the current line number**, **the total number of lines
in the file**, and **the percentage of the current line number to the total number of
lines in the file**.  
*Vim* displays more information than *vi* does, for example, the current column.

`nG` takes you to the beginning of *nth* line.  
Two back ticks (\`\`) returns you to your original position, the position where you issued
the last `G` command.  
If you have **made an edit** and then moved the cursor using some command **other than
`G`**, \`\` returns the cursor to the site of your last edit.  
If you have issued a **search command**, \`\` returns the cursor to the position where
you started the search.  
`''` (a pair of apostrophes) works much like \`\`, except it returns the cursor to the
beginning of the line.

[Back to Contents](../Contents.md)