# 6 Global replacement
Global replacement really uses two *ex* commands: `:g` (global) and `:s` (substitute).

## 6.1 The substitute command
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

## 6.2 Confirming substitutions
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

## 6.3 Doing things globally
General pattern of `:g` command:

```
:g/pattern/ command
```

Find all lines contain *pattern* and apply *command* to these lines.

## 6.4 Context-sensitive replacement
Context-sensitive actually means combine `:g` and `:s`, the general pattern of
context-sensitive replacement is as follow:

```
:g/pattern/s/old/new/g
```

First find and register all lines contain *pattern* and then replace all occurrences of
`old` with `new` in those lines

## 6.5 Pattern-matching rules
*vi* allows you to match *patterns* with not only fixed strings of characters, but also
variable patterns of words, which refered to as **regular expressions**. *Regular
expressions* are made up by combining *normal characters* with a number of special
chracters called *metacharacters*.
### 6,5.1 Metacharacters used in search patterns
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

### 6.5.2 POSIX bracket expressions
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

### 6.5.3 Metacharacters in replacement strings
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

### 6.5.4 More substitution tricks
`:s` = `:&` = `:s//~/` = `&`, these four commands all reapeat the last substitution.

`~` also repeats the last substitution, but the search pattern used is the last regular
expression used in *any* comman.

Besides slashes (`/`), you may use any *nonealphanumeric*, *nonespace* character as your
delimiter, except `\`, `"`, and `|`.

## 6.6 Pattern-matching examples
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

### More examples
```
:g/\(mg[ira]\)box/ s//\1sqare/g

:g/pattern1/ .,/pattern2/- m /pattern3/+

:%s/^[1-9][0-9]*\.[1-9][0-9.]* //
```

## 6.7 A final look at pattern matching
The pattern `.*` matches as many characters as posible. For example, in the text:

```
The greatest of times; the worst of times: moving
```

This command: `:s/ .* of/ best of/` will result: `The best of times: moving` instead of
`The best of times; the worst of times: moving`.

When using patterns to match certain text, it's better to work toward refining the
metacharacters, rather than using specific text.

Sometimes specifying exactly what you want is more difficult than specifying what you
don't want.

`[^,]*`: until the next comma (any number of any character other than a comma)

```
:1,10g/^/ 12,17t$
```

Appends 10 copies of line 12 through line 17 to the end of the file. The `:g` command
selects line 1, excutes the `:t` command, and then goes to line 2 and excutes the `:t`
command again, and so on. When line 10 is reached, `ex` will have made 10 copies.

```
Part 2
Capability Reference
.LP
Chapter 6
Introduction to the Capabilities
This and the next three chapers ...




:/^Part 2/,/^Part 3/ g/Chapter/ .+2w >> begain | +t$
```

For *Part 2*, copy all the first lines of each chapter to a file named *begain*, and then
copy all the titles of each chapter and append them to the end of the current file. 

[Previous](Chapter-5.md) | [Contents](../Contents.md) | [Next](./Chapter-7.md)
