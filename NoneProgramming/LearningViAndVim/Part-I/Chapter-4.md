# 4 Beyond the basics
## 4.1 Options when starting *vi* and *Vim*
You can open a *vi* or *vim* session with options. For example, `vi -R file` will open
### 4.1.1 Advancing to a specific place
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
### 4.1.2 Read-only mode
`vim -R file` or `view file` opens *file* in read-only mode

In read-only mode, you can still make edits, except you cannot save your edits. But if
you do want to save the changes, you can do it by adding an exclamation sign to `write`
command.
### 4.1.3 Recovering a buffer
#### Recovery in *vi*
`ex -r` or `vi -r` returns a list of files that are not saved because of unexpected
termination of sessions
`vi -r practice` recovers *practice* file.

You can force the system to preserve your buffer by `:pre` (or `:preserve`).You may find
it useful if you have edited a file but then discovered that you don't have write
permission.
#### Recovery in *Vim*
*Vim* keeps the *buffer* as a `.swap` file in the same directory of the file you're
editing. If your *buffer* were saved properly in the last session, this `.swap` file would
not exist. Otherwise, it will be there storing all your unsaved edits. If the `.swap` file
exists, when you try to open the corresponding file with *Vim*, *Vim* will ask you if you
want to recover your edits. Whether you do, or not, after that, you should quit
immediately and **manually** remove the `.swap` file.
## 4.2 Making use of registers
### 4.2.1 Recovering deletions
The last **nine** deletions are stored in *numbers registers*. But small deletions of only
**parts of lines** are not stored in the *numbered registers*. However, you can recover
these small deletions immediately after they are deleted

The last delete is saved in register `1`, the second-to-last in register `2`, and so on.

- `"2p` puts the second-to-last register (regester `2`) after the cursor.
- `"1pu.u.u.` puts register `1` and undo it, and puts register `2` and undo it, and so on.
### 4.2.2 Yanking to named registers
- `"dyy`: yank the current line and stores it in register `d`
- `"a3dd`: delete 3 lines and stores them in register `a`
- `"ap`: put contents in register `a` after the cursor
- `"Ayw`: yank the following word and **appends** it to register `a`
- `"Ddd`: delete the current line and **appends** it ot register `d`

The *numbered* and *named* registers are shared within the same *Vim* session.
## 4.3 Marking your place
- `mx`: mark the current position as `x`, `x` can be any letter (*vi* allows only lower
    case letters, *Vim* distinguishes between upper and lower case letters)
- <code>&#96;x</code>: move to the position marked `x`
- `'x`: move to the first character of the line marked `x`
- <code>&#96;&#96;</code>: return to the exact position of the previous mark or context
    after a move
- `''`: return to the beginning of the line of the previous mark or context

Place markers are set only during the current session, they are not stored in the file.

[Previous](Chapter-3.md) | [Contents](../Contents.md) | [Next](./Chapter-5.md)
