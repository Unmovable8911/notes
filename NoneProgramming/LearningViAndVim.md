# Part I vi and Vim Fundamentals
## 1 Introducing vi and Vim
- The *ex* *line editor* is the underlying editor of *vi*
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
combine them with *numbers* to repeat the command multiple times by prefixing a *number*.
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
> Address search hit BOTTOM without matching pattern

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
If you have **made an edit** and then moved the cursor using some command **other than
`G`**, \`\` returns the cursor to the site of your last edit.  
If you have issued a **search command**, \`\` returns the cursor to the position where
you started the search.  
`''` (a pair of apostrophes) works much like \`\`, except it returns the cursor to the
beginning of the line.

## 4 Beyond the basics
### 4.1 Options when starting *vi* and *Vim*
You can open a *vi* or *vim* session with options. For example, `vi -R file` will open
#### 4.1.1 Advancing to a specific place
`file` in read-only mode.
- `-c command`: run command immediately after open *file*
- `-c n`: open *file* at *nth* line
- `-c /pattern`: open *file* at first occurrence of *pattern*  
    You can continue search for *pattern* when the file is opened  
    If there's spaces in the *pattern*, should enclose the *whole pattern* with single
    or double quotes (`'` or `"`), or *escape* spaces with backslashes (`\`)
- `+n`: open *file* at line *n* (traditional *vi* session)
- `+/pattern`: open *file* at the first occurance of *pattern* (traditional *vi* session
- `+`: open *file* at last line

If `wrapscan` is disabled, you might not be able to use `-c /pattern`. If you try to open
a file this way, the editor opens a file at the last line and displays the message:
> Address search hit BoTTOM without matching pattern

**TIP** | You can mark your place by inserting a pattern such as `ZZZ` or `HERE`, then
when you return to the file, all you have to remember is `/ZZZ` or `/HERE`
#### 4.1.2 Read-only mode
`vim -R file` or `view file` opens *file* in read-only mode

In read-only mode, you can still make edits, except you cannot save your edits. But if
you do want to save the changes, you can do it by adding an exclamation sign to `write`
command.
#### 4.1.3 Recovering a buffer
##### Recovery in *vi*
`ex -r` or `vi -r` returns a list of files that are not saved because of unexpected
termination of sessions
`vi -r practice` recovers *practice* file.

You can force the system to preserve your buffer by `:pre` (or `:preserve`).You may find
it useful if you have edited a file but then discovered that you don't have write
permission.
##### Recovery in *Vim*
*Vim* keeps the *buffer* as a `.swap` file in the same directory of the file you're
editing. If your *buffer* were saved properly in the last session, this `.swap` file would
not exist. Otherwise, it will be there storing all your unsaved edits. If the `.swap` file
exists, when you try to open the corresponding file with *Vim*, *Vim* will ask you if you
want to recover your edits. Whether you do, or not, after that, you should quit
immediately and **manually** remove the `.swap` file.
### 4.2 Making use of registers
#### 4.2.1 Recovering deletions
The last **nine** deletions are stored in *numbers registers*. But small deletions of only
**parts of lines** are not stored in the *numbered registers*. However, you can recover
these small deletions immediately after they are deleted

The last delete is saved in register `1`, the second-to-last in register `2`, and so on.

- `"2p` puts the second-to-last register (regester `2`) after the cursor.
- `"1pu.u.u.` puts register `1` and undo it, and puts register `2` and undo it, and so on.
#### 4.2.2 Yanking to named registers
- `"dyy`: yank the current line and stores it in register `d`
- `"a3dd`: delete 3 lines and stores them in register `a`
- `"ap`: put contents in register `a` after the cursor
- `"Ayw`: yank the following word and **appends** it to register `a`
- `"Ddd`: delete the current line and **appends** it ot register `d`

The *numbered* and *named* registers are shared within the same *Vim* session.
### 4.3 Marking your place
- `mx`: mark the current position as `x`, `x` can be any letter (*vi* allows only lower
    case letters, *Vim* distinguishes between upper and lower case letters)
- <code>&#96;x</code>: move to the position marked `x`
- `'x`: move to the first character of the line marked `x`
- <code>&#96;&#96;</code>: return to the exact position of the previous mark or context
    after a move
- `''`: return to the beginning of the line of the previous mark or context

Place markers are set only during the current session, they are not stored in the file.

## 5 Introducing the *ex* editor
> A file is a sequence of numbered lines.
### 5.1 *ex* commands
Opening a file with *ex*:
```shell
$ ex file
"file" 4L, 235
Entering Ex mode. Type "visual" to go to Normal mode.
:
```

The last colon (`:`) indicates the *ex* prompt, you can input your command after it.

*ex* commands consists of **a line address** plus a **command**, they are finished by
hitting `ENTER`.

- `:2p`: print out the 2nd line and set the 2nd line as the current line  
    *You can ommit `p`, *ex* will still print out the scond line*
- `:1,3`: print out 1st line to 3rd line and set the 3rd line as the current line
- `:p`: print out the current line  
    *commands without line address are aplied to the current line*
- `:vi`: get to *vi*

To invoke *ex* commands, start with a colon (`:`) in *command mode*, a `Q` in command
mode of *vi* or *Vim* invoke *ex*.
### 5.2 Editing with *ex*
Introducing some *ex* commands
| Full command | Abbreviation | Description |
| ---          | ---          | ---         |
| delete       | d            | delete lines |
| move         | m            | move lines |
| copy         | co / t       | copy lines |

*You can use full commands or their abbreviation, the results are same*

You can seperate **line address patterns** and **commands** with spaces for beter
readability.
#### 5.2.1 Line addresses
You can specify line addresses in several ways:
- with explicite line **numbers**
- with **symbols** that help you specify line numbers relative to your current position
- with **search** patterns
#### 5.2.2 Defining a range of lines
Addresses use explicite numbers are called *absolute* line addresses
- `:3,18d`: delete line 3 through line 18
- `:2,23m0`: move line 2 through line 23 to the top of the file
- `:3,34co48`: copy line 3 through line 34 to line 48
- `:=`: print the total number of lines
- `:.=`: print the line number of the current line
- `:/pattern/=`: print the line number of the next occurrent of *pattern*
#### 5.2.3 Line addressing symbols
- `:.`: current line
- `:$`: last line in the file
- `:%`: all the lines in the file
- `:/pattern/+3`: 3rd line under the line contains the next occurrence of *pattern*
- `:.-2`: 2nd line above the current line
- `:+++`: move to the 3rd line below the current line. a `+` represents `+1`
- `:-`: move to the line above the current line. a `-` represents `-1`
#### 5.2.4 Search patterns
- `:/pattern/ d`: delete the next line contains *pattern*
- `:?pattern1?,/pattern2/ t $`: copy the lines from the line contains the previous
    occurrence of *pattern1* to the line contains the next occurrence of *pattern2*, and
    past them at the end of the file

A *pattern* is delimited by a `/` or a `?` both *before* and *after*. Any spaces or tabs
between slashes or question signs are interpreted as part of the *pattern*.
#### 5.2.5 Redefining the current line
In a *address range*, substitute `,` to `;` will cause *ex* redefining the *first
address* as the current line.
- `:3;+4 p`: print out line 3 through the 4th line under line 3, which is line 7.
#### 5.2.6 Global searches
A global command `g` lets you search for a pattern in all lines in the file. `g!` does the
opposite, which searches for all lines does not contain a given pattern.
- `:g/pattern`: find and move to the last occurrence of *pattern* in the file
- `:g/pattern/nu`: find and display all lines contaning *pattern* their respective line
    numbers
- `:20,34g/pattern/ d`: delete all lines between line 20 to line 34 which contain *pattern*
#### 5.2.7 Combining *ex* commands
`|` allows you combine multiple commands. `|` keeps track of the line addresses you
defined, if previous commands affects the order of lines in the file, the next command
does its work using the new line position.
- `:1,3d | s/thier/their/`: delete line 1 through line 3 (leving you now on the top line
    of the file), and the n make a substitution on the current line (which was line 4
    before the first command maks effects)
### 5.3 Saving and exiting files
In addition to the *saving* and *exiting* commands introduced before, `:x` is another
command which saves the buffer and quit, equal to `:wq`.
#### 5.3.1 Renaming the buffer
`:w new_file` saves the buffer as *new_file*.
#### 5.3.2 Saving part of a file
`:.,$ w new_file` saves the current line through the last line in *new_file*.
#### 5.3.3 Appending to a saved file
`:.,++ w >> new_file` appends the current line and the following two line to *new_file*.
#### 5.3.4 Copying a file into another file
`:3r file` ~~reads~~copys contents in *file* and past it after the line after line 3.
### 5.4 Editing multiple files
`vim file1 file2` opens *file1* and *file2* into the same session, After you have
finished editing the first file, you can `:w` write the file, and `:n` call the next file
#### 5.4.1 Using argument list
`:args` or `:ar` lists the files named **on the command line**, with the current file
enclosed in brackets.  
*vi*'s `:rewind` or `:rew` resets the current file to be the first file named on the
command line.  
*Vim* provides `:last` to move to the last file, and `:prev` to go back to the previous
file.
#### 5.4.2 Calling in new files
If you want to edit another file, save the current file, and use `:e another` takes you
to file *another*, where *another* is the other file you want to edit.
#### 5.4.3 Filename shortcuts
You can use `%` as a shorcut to refer to the current file name, and `#` refers to the
alternate file name.
- `:+3r#`: reads the content of the *alternate file* and past it after the line 3 line
    below the current line.
- `:w new_%`: if your current editing filename is practice, this command saves the
    buffer as *new_practice*

#### 5.4.4 Switching files from command mode
`CTRL-^` in *vi* is equal to `:e #` in *ex*
#### 5.4.5 Edits between files
In addition to *vi* commands `"<letter><command>`, *ex* also provides `:ya` (yank) and
`:pu` (put) which allow you to copy and past contents between files. Note that they are
used with *ex*'s line\-addressing capability and named registers.
- `:160,224ya a`: yanks line 160 through line 224 into register `a`
- `:pu a`: put register `a` after current line

## 6 Global replacement
Global replacement really uses two *ex* commands: `:g` (global) and `:s` (substitute).

### 6.1 The substitute command
The basic substitute command syntax: 

```
:s/old/new/
```

The `/` is the delimiter between parts of the *substitute command* and is optional when it
is the last character on the line.

`:s/old/new/g` substitutes all occurrences of `old` with `new` on a **line** (Don't
confuse the `g` option of `:s` command with `:g` command).

`:50,100s/old/new/g` substitutes all occurrences of `old` with `new` between line 50 to
line 100.

`:1,$s/old/new/g` and `:%s/old/new/g` substitutes all occurrences of `old` with `new` in
the entire file.

### 6.2 Confirming substitutions
Add a `c` option at end of substitute command gives you privilege to confirm each
substitution.

```
:%s/old/new/gc
```

Options in confirmation prompt:
- `y`:       substitute this match
- `n`:       skip this match
- `a`:       substitute this and all the following matches
- `q`:       quit substitution
- `l`:       substitute this match and quit (last)
- `CTRL-E`:  scroll the screen up
- `CTRL-Y`:  scroll the screen down
- `ESC`:     quit substitution

### 6.3 Doing things globally
General pattern of `:g` command:

```
:g/pattern/ command
```

Find all lines contain *pattern* and apply *command* to these lines.

### 6.4 Context-sensitive replacement
Context-sensitive actually means combine `:g` and `:s`, the general pattern of
context-sensitive replacement is as follow:

```
:g/pattern/s/old/new/g
```

First find and register all lines contain *pattern* and then replace all occurrences of
`old` with `new` in those lines

### 6.5 Pattern-matching rules
*vi* allows you to match *patterns* with not only fixed strings of characters, but also
variable patterns of words, which refered to as **regular expressions**. *Regular
expressions* are made up by combining *normal characters* with a number of special
chracters called *metacharacters*.
#### 6,5.1 Metacharacters used in search patterns
- `.`: any **single** character except a new line
- `*`: any number of (includes zero) the preceeding **one** character
- `^`: used at the **beginning** of a regular expression, indicates that the pattern
  should be found at the **beginning** of a **line**
- `$`: used at the **end** of a regular expression, indicates that the pattern should be
  found at the **end** of a **line** 
  > both `^` and `$` behaves as *normal* characters when they are placed in the middle of
  a regular expression
- `[]`: match any **one** of the character enclosed between brackets, you should not
  seperate each characters with any special characters, just list them as a string of
  characters, for example, `[d;', ]` lists `d`, `;`, `'`, `,`, and ` ` to choose from.  
  you can specify a range of consecutive characters by separating the first and last
  character in the range with a hyphen (`-`). for example, `[2-6]` lists all numbers
  between 2 and 6, `[b-z]` lists all lowercase letters between b and z  
  `\`, `-`, and `]` have special meaning inside brackets, you need to escape them if you
  want to refer to these actual characters  
  `^` has special meaning only when it's the **first** character inside brackets which
  reverses the sense, wit match any **one** character *not* in the list
- `\( \)`: save the subpattern enclosed between `\(` and `\)` into *hold buffer*, up
  to **9** subpatterns can be saved on a **single line**  
  you can later refer to the saved subpatterns with `\n` (where `n` is a number), `n` is
  related to the order in which the subpattern is created from left to right. the first
  subpattern can be accessed with `\1`, the second is `\2`, and so on
- `\<` and `\>`: match characters at the *beginning* (`\<`) or at the *end* (`\>`) of a
  **word**

#### 6.5.2 POSIX bracket expressions
Groups of characters within brackets are called *<u>bracket expressions</u>* in the POSIX
standard. Additional components provided by POSIX standard:

- *character classes*: consists of keywords bracketed by `[:` and `:]`
    - `[:alnum:]`: alphanumeric characters
    - `[:alpha:]`: alphabetic characters
    - `[:blank:]`: spaces and tab characters only
    - `[:cntrl:]`: control characters
    - `[:digit:]`: numbers
    - `[:graph:]`: printable and visible (nonespace) characters
    - `[:lower:]`: lowercase letters
    - `[:print:]`: printable characters (includes whitespaces)
    - `[:punct:]`: punctuation characters
    - `[:space:]`: all whitespace characters (space, tab, newline, vertical tab, etc)
    - `[:upper:]`: uppercase letters
    - `[:xdigit:]`: hexdecimal digits
- *callating symbols*: a multicharacter sequence that should be treated as a unit,
  bracketed by `[.` and `.]`
- *equivalence classes*: lists a set of characters that should be considered equivalent in
  a particualar locale, for example, `[[=e=]]` might match any of `e`, `è`, or `é` in
  French locale

All three constructs must appear inside the squre brackets of a bracket expression, that
is to say, they are considered as an individual unit in a bracket expression.

#### 6.5.3 Metacharacters in replacement strings
The metacharacters discussed earlier carry out their special meaning only within the
**search** pattern. For replacement pattern, there are some different metacharacters
behaves differently:

- `\n`: replace the `\n` with the text matching the *nth* subpattern previously saved by
  `\(` and `\)`
- `\`: escape the following special character
- `&`: the entire text matched by search pattern
- `~`: replacement text specified in the *last* substitute command
- `\u` and `\l`: changes the following letter to uppercase or lowercase
- `\U`, `\L`, `\e`, and `\E`: change all letters up to `\e` or `\E` change into uppercase
  or lowercase letters

#### 6.5.4 More substitution tricks
`:s` = `:&` = `:s//~/` = `&`, these four commands all reapeat the last substitution.

`~` also repeats the last substitution, but the search pattern used is the last regular
expression used in *any* comman.

Besides slashes (`/`), you may use any *nonealphanumeric*, *nonespace* character as your
delimiter, except `\`, `"`, and `|`.

### 6.6 Pattern-matching examples
Suppose you want to substitute the word *child* with *children* in a large file. Here's
some commands that can achieve your goal, each command gets more specific, accurate than
the previous one.

```
:%s;child;children;g
```

This command may affect words like *childish*, *children*, and gives you some unexpected
results

```
:%s/child /children /g
```

With this command, *childish* and *children* will not be affected, but *child* followed by
punctuations will not be substituted

```
:%s/child\([ [:punct:]]\)/children\1/g
```

This would solve the problem, but the command is too lengthy. And what if you are editing
a program code, and the word *child* is in the middle of a variabl name, you may not want
to change the variable name.

Here's the final command that is apter for this situation:

```
:%s/\<child\>/children/g
```

or you can make it even simpler:

```
:%s/\<child\>/&ren/g
```

#### More examples
```
:g/\(mg[ira]\)box/ s//\1sqare/g

:g/pattern1/ .,/pattern2/- m /pattern3/+

:%s/^[1-9][0-9]*\.[1-9][0-9.]* //
```

> `:set textwidth=90`, 
