# 1 Introduction to JavaScript
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

// Arrays
var a = [1, 2, 3];   // define an array
a.push(4, 5, 6);     // add elements to array a
a.revers();          // reverse the order of elements in array a
```

**Expressions** compute a value but don't alter the program state.  
**Statements** don't have a value, but alter the state.

#### *var*, *let*, and *const*
```javascript
function example() {
    var a = 1;
    var b = 2;
    var c = 3;

    if (true) {
        var a = 4; // reassigns the value of a to 4
        let b = 5; // creates a new block-scoped variable b
        const c = 6; // creates a new block-scoped constant c

        console.log(a, b, c); // output: 4 5 6
    }

    console.log(a, b, c); // output: 4 2 3
}
example();
```

It's generally recommended to use `let` and `const` instead of `var` in modern
JavaScript code.

| [Contents](./Contents.md) | [Next](./Chapter-2.md) |
| --- | --- |