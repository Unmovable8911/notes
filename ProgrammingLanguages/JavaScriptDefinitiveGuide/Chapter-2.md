# 2 Lexical Structure

JavaScript ignores spaces and **most** line breaks

```javascript
// This is a single line comment
/* This is also a comment */ // And here is another comment
/* 
This is a multi-lime comment.
You can expand it to as many line as you like. 
*/
```

A *literal* is a data value that appears directly in a program.

An *identifier* is simply a name, used to name *constants*, *variables*,
*classes*, and to provide names for certain *loops*. A JavaScript *identifier*
must begin with a **letter**, an **underscore**, for a **dollar sign**.

There are some *reserved words* in JavaScript which serve as keywords in
JavaScript, you should avoid using these words when declaring identifiers.

JavaScript allows *Unicode* letters, digits, and ideographs (except emojis) in
*identifiers*.
### 2.5.1 Unicode Escape Sequences
JavaScript defines escape sequences that allows us to write Unicode characters
using only ASCII chracters with `\u`. The `\u` is either followed by four
*hexdecimal* digits, or 1 to 6 *hexdecimal* digits enclosed within braces (this
braces-enclosed feature was introduced in ES6)
### 2.5.2 Unicode Normalization
Unicode allows more than one way of encoding the same character. You can use
utilities provided by your text editor or other tolls to perform Unicode
normalization of your code to prevent you from ending up with different but
visually indisdinguishable identifiers.

## 2.6 Optional Semicolons
Semicolons in JavaScript works as statement delimilaters, but you can omit the
semicolon between statements when the two statements are written on seperate
lines, or the statement is at the end of a program or the next token following
the statement is a closing brace.

JavaScript treats a line break as a semicolon if the next nonspace character
cannot be interpreted as a continuation of the current statement.

There are three special occasions when JavaScript cannot parse the socond line
as a continuation:
1. The line following a `return`, `throw`, `yield`, `break`, and `continue`.
2. The line before or after a `++` or `--`.
3. The line following a `=>` as the prameter of a function.

| [Previous](Chapter-1.md) |[Contents](./Contents.md) | [Next](./Chapter-3.md) |
| --- | --- | --- |