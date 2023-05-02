# Introduction to JavaScript
The official standard name for JavaScript is *ECMAScript* (European Computer
Manufacturer's Association), or *ES* for short.

There are three *keywords* for defining variables: `let`, `const`, and `var`.
And they have different *scoping rules* and behavior.

`var` is the original way to declare variables, it has *function scope*.

`let` and `const` were introduced in *ES6*, they have *block scope*. The
difference between `let` and `const` is that `let` allows you reasign values to
the variables, while `const` does not.

Some JavaScript syntax examples:
```javascript
'2' == 2;            // => true
'2' === 2;           // => false
true && true;        // => true
true && false;       // => false
true || false;       // => true
!(3==2);             // => true  ! inverts a boolean value

// one line functions
const plus1 = x => x + 1;
const square = x => x * x;
```

**Expressions** compute a value but don't alter the program state.
**Statements** don't have a value, but alter the state.
