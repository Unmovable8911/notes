# 5 Introducing the *ex* editor
> A file is a sequence of numbered lines.
## 5.1 *ex* commands
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
## 5.2 Editing with *ex*
Introducing some *ex* commands
| Full command | Abbreviation | Description |
| ---          | ---          | ---         |
| delete       | d            | delete lines |
| move         | m            | move lines |
| copy         | co / t       | copy lines |

*You can use full commands or their abbreviation, the results are same*

You can seperate **line address patterns** and **commands** with spaces for beter
readability.
### 5.2.1 Line addresses
You can specify line addresses in several ways:
- with explicite line **numbers**
- with **symbols** that help you specify line numbers relative to your current position
- with **search** patterns
### 5.2.2 Defining a range of lines
Addresses use explicite numbers are called *absolute* line addresses
- `:3,18d`: delete line 3 through line 18
- `:2,23m0`: move line 2 through line 23 to the top of the file
- `:3,34co48`: copy line 3 through line 34 to line 48
- `:=`: print the total number of lines
- `:.=`: print the line number of the current line
- `:/pattern/=`: print the line number of the next occurrent of *pattern*
### 5.2.3 Line addressing symbols
- `:.`: current line
- `:$`: last line in the file
- `:%`: all the lines in the file
- `:/pattern/+3`: 3rd line under the line contains the next occurrence of *pattern*
- `:.-2`: 2nd line above the current line
- `:+++`: move to the 3rd line below the current line. a `+` represents `+1`
- `:-`: move to the line above the current line. a `-` represents `-1`
### 5.2.4 Search patterns
- `:/pattern/ d`: delete the next line contains *pattern*
- `:?pattern1?,/pattern2/ t $`: copy the lines from the line contains the previous
    occurrence of *pattern1* to the line contains the next occurrence of *pattern2*, and
    past them at the end of the file

A *pattern* is delimited by a `/` or a `?` both *before* and *after*. Any spaces or tabs
between slashes or question signs are interpreted as part of the *pattern*.
### 5.2.5 Redefining the current line
In a *address range*, substitute `,` to `;` will cause *ex* redefining the *first
address* as the current line.
- `:3;+4 p`: print out line 3 through the 4th line under line 3, which is line 7.
### 5.2.6 Global searches
A global command `g` lets you search for a pattern in all lines in the file. `g!` does the
opposite, which searches for all lines does not contain a given pattern.
- `:g/pattern`: find and move to the last occurrence of *pattern* in the file
- `:g/pattern/nu`: find and display all lines contaning *pattern* their respective line
    numbers
- `:20,34g/pattern/ d`: delete all lines between line 20 to line 34 which contain *pattern*
### 5.2.7 Combining *ex* commands
`|` allows you combine multiple commands. `|` keeps track of the line addresses you
defined, if previous commands affects the order of lines in the file, the next command
does its work using the new line position.
- `:1,3d | s/thier/their/`: delete line 1 through line 3 (leving you now on the top line
    of the file), and the n make a substitution on the current line (which was line 4
    before the first command maks effects)
## 5.3 Saving and exiting files
In addition to the *saving* and *exiting* commands introduced before, `:x` is another
command which saves the buffer and quit, equal to `:wq`.
### 5.3.1 Renaming the buffer
`:w new_file` saves the buffer as *new_file*.
### 5.3.2 Saving part of a file
`:.,$ w new_file` saves the current line through the last line in *new_file*.
### 5.3.3 Appending to a saved file
`:.,++ w >> new_file` appends the current line and the following two line to *new_file*.
### 5.3.4 Copying a file into another file
`:3r file` ~~reads~~copys contents in *file* and past it after the line after line 3.
## 5.4 Editing multiple files
`vim file1 file2` opens *file1* and *file2* into the same session, After you have
finished editing the first file, you can `:w` write the file, and `:n` call the next file
### 5.4.1 Using argument list
`:args` or `:ar` lists the files named **on the command line**, with the current file
enclosed in brackets.  
*vi*'s `:rewind` or `:rew` resets the current file to be the first file named on the
command line.  
*Vim* provides `:last` to move to the last file, and `:prev` to go back to the previous
file.
### 5.4.2 Calling in new files
If you want to edit another file, save the current file, and use `:e another` takes you
to file *another*, where *another* is the other file you want to edit.
### 5.4.3 Filename shortcuts
You can use `%` as a shorcut to refer to the current file name, and `` refers to the
alternate file name.
- `:+3r`: reads the content of the *alternate file* and past it after the line 3 line
    below the current line.
- `:w new_%`: if your current editing filename is practice, this command saves the
    buffer as *new_practice*

### 5.4.4 Switching files from command mode
`CTRL-^` in *vi* is equal to `:e ` in *ex*
### 5.4.5 Edits between files
In addition to *vi* commands `"<letter><command>`, *ex* also provides `:ya` (yank) and
`:pu` (put) which allow you to copy and past contents between files. Note that they are
used with *ex*'s line\-addressing capability and named registers.
- `:160,224ya a`: yanks line 160 through line 224 into register `a`
- `:pu a`: put register `a` after current line

[Previous](Chapter-4.md) | [Contents](../Contents.md) | [Next](./Chapter-6.md)
