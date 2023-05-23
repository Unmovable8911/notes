# 3 Types, Values, and Variables
## 3.1 Overview and Definitions
JavaScript data types mainly consists of two types: 
1. *Primitive types*: Includes *numbers*, *strings*, *booleans*, `null`,
   `undefined` (`null` and `undefined` are special types, they are the only
   member of their type), and *Symbols* (ES6)
2. *Object types*: Everythin that do not belong to *primitive types*. An object
   is a collection of *properties* where each property has a **name** and a
   **value**.

When a program no longer has a way to reach a value, for example, the identifier
for the value is deleted, the *interpreter* automatically reclaims the memory it
was occupying.

Technically, only JavaScript objects have methods, but primitive types behaves
as if they have methods. Object types are **mutable** and primitive types are
**imutable**.

`null` and `undifined` are the only values that methods cannot be involked on.

JavaScript liberally converts values from one type to another.

## 3.2 Numbers
JavaScript presents numbers using the 64-bit floating point format. But certain
operations in JavaScript (such as array indexing and bitwise operators) are
performed with 32-bit integers.

When a number appears directly in a JavaScript program, it's called a *numeric
literal*.
### 3.2.1 Integer Literals
Base-10 integer literals are represented as normal numbers.  
Base-16 or hexdecimal integer literals begin with a `0x` or `0X`.  
Base-2 or binary integer literals begin with a `0b` or `0B`.  
Base-8 or octal integer literals begin with a `0o` or `0O`.  
### 3.2.2 Floating-Point Literals
Floating-point literals can be represented using exponential notation, the
following natation represents the number multiplied by 10 to the power of the
exponent.

```
[digits][.digits][(e|E)[+|-]digits]
```

You can use underscores within numeric literals to break long literals up into
chunks that are easier to read.

```javascript
let billion = 1_000_000_000;       // Underscore as a thousand separator.
let bytes = 0x89_ab_cd_ef;         // As a bytes separator.
let bits = 0b0001_1101_0111;       // As a nibble separator.
let fraction = 0.123_456_789;      // Works in the fractional part, too.
```
### 3.2.3 Arithmetic in JavaScript
```javascript
Math.pow(2,53);            // 2 to the power 53
Math.round(.6);            // => 1.0: round to the nearest integer
Math.ceil(.6);             // => 1.0: round up to an integer
Math.floor(.6);            // => 0.0: round down to an integer
Math.abs(-5);              // => 5: absolute value
Math.max(x, y, z);         // Return the largest argument
Math.min(x, y, z);         // Return the smallest argument
Math.random();             // Generate a number x where 0 <= x < 1.0
Math.PI;                   // circuference of a circle / diameter
Math.E;                    // e: the base of the natual logarithm
Math.sqrt(3);              // The squre root of 3
Math.cbrt(9);              // Cube root of 9
Math.sin(0);               // => 0
Math.log(10);              // Natural logarithm of 10
Math.log(100)/Math.LN10;   // Base 10 logarithm of 100
Math.log(256)/Math.LN2;    // Base 2 logarithm of 256
Math.exp(3);               // Math.E cubed
```

JavaScript does not raise errors in cases of overflow, underflow, or divided by
zero.  
- Overflow: the absolute value ecceded the largest representable number.
  Returns `Infinity` or `-Infinity`.
- Underflow: the absolute value is closer to zero than the smallest
  representable number. Returns `0` or *negative zero* which looks indentical to
  `0`.
- Division by zero: returns `Infinity` or `-Infinity`. In addition, zero divided
  by zero results a special *not-a-number* value `NaN`.

The `Number` object:
```javascript
Number.POSITIVE_INFINITY     // => Infinity
Number.NEGATIVE_INFINITY     // => -Infinity
Number.NaN                   // => NaN
Infinity/Infinity            // => NaN

// The following Number properties are defined in ES6
Number.parseInt()            // Same as global parseInt() Function
Number.parseFloat()          // Same as global parseFloat() Function
Number.isNaN(x)              // Is x the NaN value
Number.isFinite(x)           // Is x a number and finite
Number.isSafeInteger(x)      // Is x an integer where -(2**53) < x < 2**53
```

`NaN` does not compare equal to any value, including itself.

The global function `isNaN()` returns `true` if the argument passed to it is
`NaN` just as `Number.isNaN()` does, and it also returns `true` if the argument
is a non-numeric value that cannot be converted to a number.

Global function `isFinite()` works as `Number.isFinite()` does, but it also
returns true when its argument can be converted to a finite number.

*Negative zero* compares equal to normal zero, the difference raises when they
are used as divisors. For example, `1/0 === 1/-0` returns false.
### 3.2.4 Binary Floating-Point and Rounding Errors
JavaScript (and almost all other languages) uses binary values to represent
floating-point values, which can represent fractions like 1/2, 1/4, 1/8, and so
on, decently. But cannot represent a decimal fraction exactly. As a
demonstration, see the following example:

```javascript
let x = .3 - .2;
let y = .2 - .1;
x === y;                 // => false
x === .1;                // => false
y === .1;                // => true
```

Because of rounding error, `.3 - .2` doesn't equal to `.2 - .1`.

### 3.2.5 Arbitrary Precision Integers with `BigInt`
`BigInt` literals are written as a string of digits followed by a lowercase
letter `n`. Note that mixing operands of `BigInt` with regular number operands
is not allowed.  
Comparison operators do work with mixed types.

```javascript
1 < 2n    // => true
2 > 1n    // => true
0 == 0n   // => true
0 === 0n  // => false
```

The `BitInt()` function converts number literals or number-looking strings into
`BitInt` values

None of the methods of `Math` object accept `BitInt` operands.

### 3.2.6 Dates and Times
The JavaScript `Date` class represents and manipulates the number that
represents dates and times. It has a numeric value represents the *timestamp*
which specifies the number of elapsed milliseconds since Jan 1 1970.

```javascript
let timestamp = Date.now();       // The current time as a timestamp
let now = new Date();             // The current time as a Date object
let ms = now.getTime();           // Convert to a millisecond timestamp
let iso = now.toISOString();      // Convert to a string in standard format
```

## 3.3 Text
A string is an immutable ordered sequence of 16-bit values, each of which
typically represents a Unicode character. The length of a string is the number
of 16-bit values it contains.

JavaScript uses the UTF-16 encoding. Unicode characters do not fit in 16-bits
are encoded using the rules of UTF-16 as a sequence  (known as a *surrogate
pair*) of two 16-bit values.

Most string-manipulation methods defined by JavaScript works on the 16-bit
value, which means, when they encounter a Unicode character that is represented
using a *surrogate pair*, they will treat that character as two seperate
characters.  
In ES6, however, strings are iterable, and the `for/of` loop or `...` operator
will iterate the actrual character, not the 16-bit value.
### 3.3.1 String Literals
A string literal in JavaScript is enclosed between a pair of single or double
quotes or backticks (`'`, `"`, or `` ` `` ).

You can break a string literal across multiple lines by ending each line except
the last with a bachslash (`\`).

```javascript
// A one-line string written on 3 lines
"one\
long\
line"

// A two line string written on 2 lines
`the newline character at the end of this line
is included literally in this string`
```

| [Previous](Chapter-2.md) |[Contents](./Contents.md) | [Next](./Chapter-4.md) |
| --- | --- | --- |
```
:so ~/Documents/exrc/js_exrc
```
