# 7 Advanced editing
## 7.1 Customizing *vi* and *Vim*
You can change the *vi* or *Vim* behavior from within the editor or in a *.exrc* file in
your home directory by using the *ex* command `:set`.
### 7.1.1 The `:set` command
There are two types of options that can be changed with the `:set` command:

- Toggle options
- Options that take a **numeric** or **string** value

Many options have both *complete names* and *abbreviations*, both have the same effect.

To enable or disable toggle options:

```
 Enable
:set option
 Disable
:set nooption
 Toggle
:set option!
```

More usage of `:set`:

```
 Display the complete list of options
:set all
 Display only the options which user has specifically changed
:set
 Show the current value of any individual option
:set option?
 Set the value of an option
:set option=value
```

### 7.1.2 The `.exrc` file
The `.exrc` file controls your *ex*, *vi*, and *Vim* editing environment, put this file in
your home directory. You can edit this file just as how to edit any other text files.
After you have edited the file, you can restart *vi* or use `:source` command to make the
changes take effect.

You don't need to preceed the commands in your `.exrc` file with a *ex* prompt `:`.

### 7.1.3 Alternate environments
You can create a `.exrc` file in the directory of your editing files. Settings in this
`.exrc` file will only affect the editing environment in the current directory. But you
have to set the `exrc` option in the `.exrc` file in your home directory (`:set exrc`).

You can also define alternate editing environment by saving option settings in a file
other than `.exrc` and reading in that file with the `:so` (short for `:source`) command.

### 7.1.4 Some useful options
#### Options take values
- `wrapmargin` (`wm`): specifies the size of the right margin that is used to auto wrap
  text as you type. A typical value is 7 to 15.
- `textwidth`: specifies the length of a line and automatically wrap the line as you type.
  A typical value is 80.
- `shiftwidth` (`sw`): define the number of spaces in backward tabs when using the
  `autoindent` option, and for the `<<` and `>>` commands.
- `tabstop` (`ts`): number of spaces that a tab indents.
#### Toggle options
- `ignorecase`: ignore case in search patterns
- `wrapscan`: search loop back to the first occurrence when it reaches the last one.
- `magic`: recognizes wildcard characters when pattern matching.
- `autoindent`: automatically indent blocks while entering a new line, use with
  `shiftwidth` option.
- `expandtab`: render tabs into spaces
- `list`: print tabs as `^I`; mark ends of lines with `$`.
- `number` (`nu`): display the line number.
- `showmatch` (`sm`): when ) or } is entered, move the cursor briefly to the matching ( or
  {. (if no match, ring the error message bell)
- `autowrite`: automatically write the buffer when you issue the command `:n` and before
  running a shell command with `:!`.

For a brief description of each option, refer to *Appendix B* in *Learning the vi And the
Vim Editor*.

## 7.2 Executing Unix commands
Prefix the Unix commands with a exclamation mark (!): `:!command`

If you want to give a several Unix commands in a row without returning to the editing
session, you can create a shell with: `:sh`. Quit the shell with `CTRL-D` or `exitENTER`.

You can combine *ex* commands with Unix commands. For example, `:r !pwd` reads in the
output of Unix command `pwd`

### Filtering text through a command
Note that the sorted output replaces the original text in the buffer.
#### 1) Fieltering text with *ex*
```
:39,90 !sort
```

Sort line 39 through line 90
#### 2) Fieltering text with *vi*
```
!4jsort
4!jsort
```

Sort current and the following 4 lines of text

There are a few obscure behavior of the way *vi* acts when you use this feature:
- The exclamation mark doesn't appear right away when you enter it. It appears when you
  enter the keystroke(s) for the text object, but the character(s) you entered to
  reference the object won't appear.
- Text blocks must be more than one line. A number may precede either the exclamation mark
  or the text object to repeat mutiply the text object referenced by the character.
- Entire lines are affected
- You can specify the current line by entering a second exclamation mark

## 7.3 Saving commands
### 7.3.1 Word abbreviation
You can define an abbreviation in *vi* and whenever you enter the abbreviation in insert
mode as a **full word**, the abbreviation expand to the phrase you defined as soon as you
press a *nonalphanumeric* character (e.g. punctuations), a space, a carriage return, or
`ESC`.

```
:ab js JavaScript
```

TO disable the abbreviation:

```
:unab js
```

To list your defined abbreviations:

```
:ab
```

Note that the charaters that compose your abbreviation cannot also appear at the end of
your phrase.

### 7.3.2 Using the map command
The `map` command acts a lot like `ab` command except that you define a macro for command
mode instead of insert mode.
- `:map x sequence`: define character *x* as a sequence of editing *sequence*
- `:unmap x`: disable the *sequence* defined for *x*
- `:map`: list the characters that are currently mapped

Keys *vi* doesn't use: `g`, `K`, `q`, `v`, `V`, `^A`, `^K`, `^O`, `^W`, `^X`, `_`, `*`,
`\`, and `=`.  
*Vim* uses all of these characters except for `^K` and `\`

> The `=` is used by *vi* if Lisp mode is set. In many modern versions of *vi*, `_` is
> equal to `^`. *Vim* has a visual mode that uses `v` and `V`.

You can also map keys that the command mode already uses.

### 7.3.3 Mapping with a leader
Defining a map with mapleader:

```
:map <leader>q :wq
```

The default key of mapleader is backslash (\\), you can set the mapleader to the key you
prefer:

```
:let mapleader="x"
```

### 7.3.4 Escaping special keys
Special keys such as `ENTER`, `ESC`, `BACKSPACE`, and `DELETE` can be escaped when you try
to type them in insert mode or in a sequence of *ex* command by preceding them with
`CTRL-V`.  
In addition, there are 3 control characters that must be escaped with `CTRL-V`: `CTRL-T`,
`CTRL-W`, and `CTRL-T`.

There's one character always has a special meaning in *ex* commands -- `|`, even if you
try to escape it with `CTRL-V`. And you cannot use a vertical bar in insert mode maps.

Mapped sequences are allowed to contain other mapped commands, and this ability is
controled by the `remap` option.

### 7.3.5 Mapping keys for insert mode
General syntax:
- `:map! x sequence`: mapping *x* for command *sequence*
- `:unmap! x`: unmapping key *x*
- `:map!`: list mapped keys for insert mode

### 7.3.6 Mapping function keys
- `map 1 sequence`: mapping `F1` to the command *sequence* in **command mode**
- `map! 1 sequence`: mapping `F1` to the command *sequence* in **insert mode**

### 7.3.7 Mapping other special keys
You can also map other special keys, such as `HOME`, `END`, `PAGEUP`, `PAGEDOWN`, etc. by
preceding them with a `CTRL-V`.

***Vim*** provides a portable way to describe special keys by enclosing the key name
between a pair of angle brackes. For example, `<Home>` representes `HOME`, `<ESC>`
representes `ESC`. It also provides a shortcut for combination of special keys and
character, for example, `<C-i>` representes `CTRL-I`, `<A-s>` represents `ALT-S`.

### 7.3.8 Mapping multiple keys
You can conbine multiple keys to reference a command sequence. For example, you may find
`:map! ii <ESC>` very helpful for it saves a lot of strech.

### 7.3.9 @-Functions
Named registers provide yet another way to create macros. By saving a line of either *ex*
command or *vi* command (as text) into a named register, for example register *a*, and you
can refer to the  command by typing `@a`.

`@@` repeats the last `@`.

*Vim* provides another way to save text into a named regiter. `qatextq` saves *text* into
register *a*.

### 7.3.10 Excuting registers from *ex*
You can also use *ex* `:@` command to excute commands saved in registers. For example,
`:@g` excute the command saved in register `g`.

## 7.4 Using *ex* scripts
```
$ ex -s document < exscript
```

Execute the commands in `exscript` to alter `document`. `-s` option suppress the terminal
message (stands for either "script mode" or "silent mode").

### 7.4.1 Looping in a shell script
```shell
for file in "$@"
do
    ex -s "$file" < exscript
done
```

`"$@"` represents all arguments given to the *shell script* when invoking it. Quoting the
expansion of the `file` variable ($file) allows the script to work event when the filename
have spaces in their names.

```shell
" Assign variable to a, b, c, and d.
for variable in a b c d

" Assign variable in turn to the name of each file in which grep finds the string Alcuin.
" -l prints the filenames whose contents match the pattern, without printing the actual
" matching lines.
for variable in $(grep -l "Alcuin" *)

" equal to for variable in "$@"
for variable
```

### 7.4.2 Here documents
You can include your editing commands (*ex* commands) in the shell script.

```shell
for file in "$@"
do
    ex -s "$file" << end-of-script
    g/thier/s//their/g
    g/writeable/s//writable/g
    wq
    end-of-script
done
```

The string `end-of-script` is entirly arbitrary as long as the editing commands doesn't
include it, because it can be used by the shell to recognize when *here document* is
finished. By convention, many users the string `EOF`, or `E_O_F` to indicate the end of
the editing commands.

### 7.4.3 Sorting text blocks
> `g/^<glossaryitem>/,/^<\/glossaryitem>/-1 s/$/@@/`  *Append `@@` to each line*
>
> `g/^<glossaryitem>/,/^<\/glossaryitem>/j`           *Join the entries*
>
> `%!sort`                                            *Sort the lines*
>
> `%s/@@ /g`                                       *Break the lines apart*
>
> `wq`                                                *Save and quit*

The `j` command in the second command joints two lines together and converts the *newline*
into a *space*.

## 7.5 Editing program source code
### 7.5.1 Indentation control
With `autoindent` option enabled, you can type `CTRL-T` at the start of a line gives you a
level of indentation, and `CTRL-D` to removes a level of indentation in **insert mode**.

`<<` shifts a line left one indentation level and `>>` shifts a line right one indentation
level. You can also prefix a number with these two commands to indent multiple lines.

It is convenient to have a `shiftwidth` that is the same size as `tabstop`.

`expandtab` converts tabs into a number of spaces based on the value of `tabstop` option.
And you can use the `expand` utility to convert the already existing tabs into spaces.

`:set list` alters your display so that a tab appears as the control character `^I` and
an end of line appears as a `$`.

`:5.20l` displays line 5 through 20, showing tab characters and end-of-line characters.

### 7.5.2 A special search command
`%` moves the cursor to the corresponding prenthesis or bracket if the cursor is above a
prenthesis or bracket, if not, *vi* searches forward on the current line to the first
openning prenthesis or bracket, and then moves the cursor to its matching prenthesis or
bracket.

*Vim* can highlight the matching prenthesis or bracket using the *matchparen* plug-in,
which is loaded by default.

### 7.5.3 Using tags
With a *tags* file created by *ctags* program, you can determine which files define which
functions while editing source code.

- `$ ctags *.c` creates a *tags* file describing the C source files in the directory.
- `:tag name` looks at the *tags* file to find out which file contains the definition of
  *name*
- `CTRL-]` performs tag look up for the identifier that is under the cursor, the editor
  uses the "word" under the cursor starting at the current cursor position, not the entire
  word containing the cursor.
- `:tag` displays the tag stack in *Vim*

In *Vim*, each time you look up for a idertifier using `:tag` or `CTRL-]`, the editor
saves the current location before searching for the *tag*, you may then return to a saved
location using `CTRL-T`.

[Previous](Chapter-6.md) | [Contents](../Contents.md) | [Next](../Part-II/Chapter-8.md)
