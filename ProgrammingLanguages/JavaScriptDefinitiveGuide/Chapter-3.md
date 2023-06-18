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

### 3.3.2 Escape Sequences in String Literals
- `\0`: The NUL character (`\u0000`)
- `\b`: Backspace (`\u0008`)
- `\t`: Horizontal tab (`\u0009`)
- `\n`: Newline (`\u000A`)
- `\v`: Vertical tab (`\u000B`)
- `\f`: Form feed. Abbreviate as `FF`, indicates end of a page (`\u000C`)
- `\r`: Carriage return (`\u000D`)
- `\xab`: Unicode character specified by two hexdecimal digits.
- `\uabcd`: Unicode character specified by four hexdecimal digits.
- `\u{n}`: Unicode character specified by 1 to 6 hexdecimal digits.

`"`, `'`, and `\` has special meanings in JavaScript, if you want to refer to
these actual characters, you need to prefix a `\` before them.

If `\` preceds any character other than the foremention ones, `\` is ignored.

### 3.3.3 Working with Strings
```javascript
// + can be used to concatinate strings
let msg = "Hello" + ", " + "World!";     // => "Hello, World"

// String Comparison is done by comparing the 16-bit value
"Example" === "Example";                 // => true
"b" > "a"                                // => false

// The length property stores the number of 16-bit values a string contains
let s = "123456";
s.length                                 // => 6

// String methods
let s = "Hello, world!";

// Obtaining portions of a string
s.substring(1,4);     // => "ell": characters from index 1 to 4
s.slice(1,4);         // => "ell": characters from index 1 to 4
s.slicc(-3);          // => "ld!": last three characters
s.split(",")          // => ["Hello", " world!"]: split at delimiter string

// Searching a string
s.indexOf("l");       // => 2: index of the first "l"
s.indexOf("l", 3);    // => 3: index of the first "l" at or after index 3
s.indexOf("zz");      // => -1: string not found
s.lastIndexOf("l");   // => 10: index of the last "l"
s.startsWith("H");    // => true
s.endsWith("d");      // => false
s.includes("or");     // => true

// Creating modified string
s.replace("llo", "ya"); // => "Heya, world!"
s.toLowerCase();      // => "hello, world!"
s.toUpperCase();      // => "HELLO, WORLD!"
s.normalize();        // Unicode NFC normalization
s.normalize("NFD");   // Unicode NFD normalization, Also "NFKC", "NFKD"

s.charAt(0);          // => "H"

// Bracket notation with strings
s[1,4]                // => "ell"
s[0]                  // => "H"

// Sting padding methods
"x".padStart(3);      // => "   x"
"x".padEnd(3);        // => "x   "
"x".padStart(2, "="); // => "==x"
"x".padEnd(3,"-");    // => "x---"

// Space trimming methods
"  a  ".trim();       // => "a"
"  a  ".trimStart();  // => "a  "
"  a  ".trimEnd();    // => "  a"

"ab".repeat(3);       // => "ababab"
```

Strings are **immutable**, methods like `replace` do not modify the string,
instead, they return a new string.

### 3.3.4 Template Literals
Template literals are enclosed between a pair of backticks, they work like
*f-strings* in Python.
```javascript
let name = "Alice";
let greeting = `Hello, ${name}!`
```

If you put a function name before a template literal, JavaScript will pass the
template literal to the function as its argument. This is called a tagged
template literal, and the return value of a tagged template literal is the
return value of the function.  
The following example illustrates this usage by using the JavaScript built-in
function `String.row()`, which return the text passed to it without proccessing
the backslash escapes:
```javascript
`\n`.length           // => 1
String.row`\n`.length // => 2
```

### 3.3.5 Pattern Matching
JavaScript defines a datatype as a *regular expression* (RegExp) for describing
and matching patterns in strings.  
Text between a pair of slashes constituts a RegExp. The second slash of the pair
can be followed by one or more letters to modify the meaning of the RegExp.
```javascript
let text = "testing: 1, 2, 3";
let pattern = /\d+/g;    // Match all instances of one or more digits
pattern.test(text);      // => true: a match exist
text.search(pattern);    // => 9: position of the first match
text.replace(pattern, "#"); // => "testing: #, #, #"
```

## 3.4 Boolean Values
`undefined`, `null`, `0`, `-0`, `NaN`, and `""` are falsy values in JavaScript,
all values other than these are truthy values.  
Any time JavaScript expects a boolean value, a falsy value works as false, and a
truthy value works as true.

Boolean oprators:
- `&&`: AND
- `||`: OR
- `!`: NOT

## 3.5 null and undifined
Both `null` and `undefined` indicates the absence of a value. The difference is
that, `null` is a **language keyword** while `undefined` is a **global
constant**.
- `null` indicates "no value" for numbers, strings, as well as objects.
- `undefined` is the value of:
    - variables not been initializedi;
    - an object property or array element that does not exist;
    - a function return which the function does not explicitly return a value;
    - a function parameter for which no argument is passed.

## 3.6 Symbols
Symbols were introduced in ES6 to serve as non-string **property names**. The
Symbol type does not have a literal syntax. You can use Symbols as property
names to avoid accidentally override the existing properties.

- `Symbol()` function creates a Symbol value, it never returns the same value
  twice, even when called with the same argument.
- `Symbol.for()` function returns a Symbol value associated to the string
  argument passed to it, or creates a Symbol value associated to the argument.

```javascript
let symbol1 = Symbol("test");
let symbol2 = Symbol("test");
symbol1.toString() == symbol2.toString();   // => true
symbol1 === symbol2                         // => false

let s = Symbol.for("shared");
let t = Symbol.for("shared");
s === t                                     // => true
Symbol.keyFor(t);                           // => "shared"
s.toString() == t.toString();               // => true
```

## 3.7 The Global Object
The global object has properties that are globally defined indentifies that are
available to JavaScript program. When a JavaScript interpreter starts (or
whenever a web browser loads a new page), it creates a new global object and
gives it a set of initial properties:
- Global constants like `undefined`, `NaN`, and `Infinity`;
- Global functions like `isNaN()`, `parseInt()`, etc.
- Constructor functions like `Symbol()`,  `String()`, etc.
- Global objects like `Math` and `JSON`.

- In node, the global object has a property `global` refers to itself.
- In web browsers, the Window object serves as the global object, and it has a
  property `window` refers to itself.
- ES2020 defined `globalThis` as the standard way to refer to global object in
  any context.

## 3.8 Immutable Primitives Vs. Mutable Objects
When you compare two primitive, they are compared by their actual value.
However, when you compare two objects, they are compared by **reference**, not
the actual property-value pair stored in them.

## 3.9 Type Conversion
Examples
```javascript
10 + " objects"        // => "10 objects"
"7" * "4"              // => 28
let n = 1 - "x"        // => n == NaN
n + " objects"         // => "NaN objects"
```

JavaScript Type Conversions
| value | to String | to Number | to Boolean |
| ----- | --------- | --------- | ---------- |
| `undefined` | "undefined" | 0 | `false` |
| `null` | "null" | 0 | `false` |
| `true` | "true" | 1 | |
| `false` | "false" | 0 | |
| "" | | 0 | `false` |
| "1.2" | | 1.2 | `true` |
| "one" | | `NaN` | `true` |
| 0 | "0" | | `false` |
| -0 | "0" | | `false` |
| 1 | "1" | | `true` |
| `Infinity` | "Infinity" | | `true` |
| `-Infinity` | "-Infinity"| | `true` |
| `NaN` | "NaN" | | `false` |

> Strings that can be parsed as numbers convert to those numbers. Leading and
> trailing spaces are allowed. But any nonspace characters that are not the part
> of a numeric lieral causes the conversion to produce `NaN`.

### 3.9.1 Conversion and Equality
`==` is flexible equality sign which allows type conversions.  
`===` is strict equality sign which do not allow type conversions.

### 3.9.2 Explicit Conversion
> Global functions `Number()`, `String()`, and `Boolean()` trys to convert their
> first argument into the data type their name indicates.

> If one operand of the `+` operator is a string, it converts the other one to a
> String and concatenates them. The unary `+` operator converts its operand to a
> number

> Unary `!` operator converts its operand to a boolean and negates it, `!!`
> simply converts its operand to a boolean.

> `Number()` only works for base-10 integers and does not allow trailing
> charactors that are not the part of the literal.

> The `parseInt()` parses only integers. `parseInt()` supports parsing
> hexdecimal leterals when it's prefixed by "0x" or "0X". `parseInt()` accepts
> an optional second argument specifying the radix of the number to be parsed,
> legal values are between 2 and 36.
>
> The `parseFloat()` function parses both integer and floating point numbers.
> `parseFloat()` does not accept hexdecimal literals.
>
> Both `parseInt()` and `parseFloat()` ignore the leading spaces, parse as many
> numeric characters as possible and ignore the following non-numeric
> characters. If the first nonspace character is not a valid numeric character,
> the invocation returns `NaN`. They both return numbers in base-10.

```javascript
parseInt("3 blind mice")     // => 3
parseFloat(" 3.14 meters")   // => 3.14
parseInt("0xff")             // => 255
parseFloat("0xff")           // => 0
parseFloat(".1")             // => 0.1
parseInt("0.1")              // => 0
parseInt(".1")               // => NaN

parseInt("11", 2)            // => 3
parseInt("ff", 16)           // => 255
```

#### Number object methods:
- `toString()` accepts an optional argument as the *radix*(number base)
  to convert the number into that *radix* number.
- `toFixed()` converts a number to a string with a specified number of digits
  after the decimal point.
- `toExponential()` converts a number to a string using exponential notation.
- `toPrecision()` converts a number to a string with the number of significant
  digits you specify. It uses exponential notation if the number of significant
  digits is not large enough to display the entire **integer portion** of the
  number.

The last three methods automatically round the trailing digits or pad with zeros
as appropriate.


## 3.10 Variable Declaration and Assignment
Before you can use a variable or constant in JavaScript, you must declare it
using one of the following keywords: `let`, `const`, or `var`.

### 3.10.1 Declarations with `let` and `const`
```javascript
let i;                  // i == undefined
let i, sum;

// declaring with initial values
let message = "hello";
let i = 0, j = 0, k = 0;

// constants must be declared with an initial value
const DIR = "templates/registration/login.html";
```

> There's a common convention to declare constants with all letters
> capitallized.
>
> It is possible to declare the loop variables as constants for `for/in` and
> `for/of` loops, and that value is constant for the duration of on loop
> iteration.

> Variables and constants declared with `let` and `const` are *block scoped*.
> That rughly means they're accessible between the pair of curly braces they're
> defined.
>
> Variables and constants defined outside of any block are global. In node, the
> scope of a global variable is the file in which it's declared. In browsers,
> the scope of a global variable is the HTML document in which it is declared.
>
> Declaring a variable with the same name using `let` or `const` within the same
> scope is a syntax error.

### 3.10.2 Variable Declarations with `var`
Variables declared with `var` are *function scoped*
- They're scoped to the body of the containing function no matter how deeply
  nested they are inside  that function.
- Globals declared with `var` are implemented as **properties** of the **global
  object**.
- It's legal to declare the same variable multiple times with `var`. And it's
  common to declare a iterate variable with `var`
- Evaluation to a variable declared with `var` can be placed before the
  declaration within the enclosing function.

### 3.10.3 Destructuring Assignment
Array style destructuring assignment.
```javascript
let [x, y] = [1, 2];           // x == 1, y == 2
[x, y] = [++x, ++y];           // x == 2, y == 3

// Destructuring assignment in loops.
let o = {x: 1, y: 2};
for (const [name, value] of object.entries(o) {
    console.log(name, value);
}

// Extra variables on the left are assigned undefined, and extra values on the
// right are ignored.
let [x, y] = [1];             // y == undefined
[x, y] = [1, 2, 3];           // x == 1, y == 2
[, x, , y] = [1, 2, 3, 4];    // x == 2, y == 4

// With three dots prefixing the last variable, the remaining values will be
// collected by the last variable.
let [x, ...y] = [1, 2, 3, 4];         // y == [2, 3, 4]

// Destructuring assignment can be nested
let [a, [b, c]] = [1, [2, 3], 4];     //  a == 1, b == 2, c == 3

// Any interable value can be put on the right hand side of equal sign.
let [first, ...rest] = ["hello"];    // first == "h", rest == "ello"
```

Object style destructuring assignment.
```javascript
let transparent = {r: 0.0, g: 0.0, b: 0.0, a: 0.0};
let {r, g, b} = transparent;
// Variable names should match the property names of the object. Variable names
// not matching any property names within the object are assigned `undefined`.

// Identifiers on the lefthand side can also be a colon-separated pair of
// identifiers, where the first is the name of the property, and the second is
// the name of the variable to be assigned it to.
const {cos: cosine, tan: tangent} = Math;
// cosine == Math.cos, tangent == Math.tan

let points = [{x: 1, y: 2}, {x: 3, y: 4}];
let [{x: x1, y: y1}, {x: x2, y: y2}] = points
```

[Previous](Chapter-2.md) |[Contents](./Contents.md) | [Next](./Chapter-4.md)
