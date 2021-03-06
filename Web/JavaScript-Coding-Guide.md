JavaScript Coding Guidelines
===========================

## Spacing
- Indentation with tabs.
- No whitespace at the end of line or on blank lines.
- Lines should be no longer than 80 characters, and must not exceed 100 (counting tabs as 4 spaces). There are 2 exceptions, both allowing the line to exceed 100 characters:
  - If the line contains a comment with a long URL.
  - If the line contains a regex literal. This prevents having to use the regex constructor which requires otherwise unnecessary string escaping.
- if/else/for/while/try always have braces and always go on multiple lines.
- Unary special-character operators (e.g., !, ++) must not have space next to their operand.
- Any , and ; must not have preceding space.
- Any ; used as a statement terminator must be at the end of the line.
- Any : after a property name in an object definition must not have preceding space.
- The ? and : in a ternary conditional must have space on both sides.
- No filler spaces in empty constructs (e.g., {}, [], fn()).
- There should be a new line at the end of each file.
- Any `!` negation operator should have a following space.
- All function bodies are indented by one tab, even if the entire file is wrapped in a closure.
- Spaces may align code within documentation blocks or within a line, but only tabs should be used at the start of a line.

Good Spacing Example:
```js
var i;

if ( condition ) {
    doSomething( 'with a string' );
} else if ( otherCondition ) {
    otherThing( {
        key: value,
        otherKey: otherValue
    } );
} else {
    somethingElse( true );
}

// Unlike jQuery, WordPress prefers a space after the ! negation operator.
// This is also done to conform to our PHP standards.
while ( ! condition ) {
    iterating++;
}

for ( i = 0; i < 100; i++ ) {
    object[ array[ i ] ] = someFn( i );
    $( '.container' ).val( array[ i ] );
}

try {
    // Expressions
} catch ( e ) {
    // Expressions
}
```

## Object

Object and array expressions can be on one line if they are short (remember the line length limits). 
When an expression is too long to fit on one line, there must be one property or element per line, 
with the opening and closing braces each on their own lines. 
Property names only need to be quoted if they are reserved words or contain special characters:
```js
// Bad
map = { ready: 9,
    when: 4, "you are": 15 };
 
array = [ 9,
    4,
    15 ];
 
array = [ {
    key: val
} ];
 
array = [ {
    key: val
}, {
    key2: val2
} ];
 
// Good
map = { ready: 9, when: 4, "you are": 15 };
 
array = [ 9, 4, 15 ];
 
array = [ { key: val } ];
 
array = [ { key: val }, { key2: val2 } ];
 
array = [
    { key: val },
    { key2: val2 }
];
 
// Good as well
map = {
    ready: 9,
    when: 4,
    "you are": 15
};
 
array = [
    9,
    4,
    15
];
 
array = [
    {
        key: val
    }
];
 
array = [
    {
        key: val
    },
    {
        key2: val2
    }
];
```

## link Multi-line Statements

When a statement is too long to fit on one line, line breaks must occur after an operator.
```js
// Bad
var html = "<p>The sum of " + a + " and " + b + " plus " + c
    + " is " + ( a + b + c );
 
// Good
var html = "<p>The sum of " + a + " and " + b + " plus " + c +
    " is " + ( a + b + c );
```
Lines should be broken into logical groups if it improves readability, 
such as splitting each expression of a ternary operator onto its own line even if both will fit on a single line.
```js
// Acceptable
var baz = ( true === conditionalStatement() ) ? 'thing 1' : 'thing 2';

// Better
var baz = firstCondition( foo ) && secondCondition( bar )
    ? qux( foo, bar )
    : foo;
```
When a conditional is too long to fit on one line, 
successive lines must be indented one extra level to distinguish them from the body.
```js
    if ( firstCondition() && secondCondition() &&
            thirdCondition() ) {
        doStuff();
    }
```

## link Chained Method Calls

When a chain of method calls is too long to fit on one line, 
there must be one call per line, with the first call on a separate line from the object the methods are called on. If the method changes the context, an extra level of indentation must be used.
```js
elements
    .addClass( "foo" )
    .children()
        .html( "hello" )
    .end()
    .appendTo( "body" );
 ```
## Blocks and Curly Braces

if, else, for, while, and try blocks should always use braces, and always go on multiple lines. The opening brace should be on the same line as the function definition, the conditional, or the loop. The closing brace should be on the line directly following the last statement of the block.

```js
var a, b, c;

if ( myFunction() ) {
    // Expressions
} else if ( ( a && b ) || c ) {
    // Expressions
} else {
    // Expressions
}
```
## Assignments and Globals

### Variable Declarations

When possible, variables should be declared using a `const` declaration. Use 
`let` only when you anticipate that the variable value will be reassigned 
during runtime. `var` should not be used in any new code.

Note that `const` does not protect against mutations to an object, so do not
use it as an indicator of immutability.

```javascript
const foo = {};
foo.bar = true;

let counter = 0;
counter++;
```

### Globals

Globals should almost never be used. If they are used or you need to reference a pre-existing global do so via `window`.

```js
var userId = window.currentUser.ID;
```

## Naming Conventions

Variable and function names should be full words, using camel case with a lowercase first letter. This also applies to abbreviations and acronyms.

```js
// Good
var userIdToDelete, siteUrl;

// Bad
var userIDToDelete, siteURL;
```

Names should be descriptive, but not excessively so. Exceptions are allowed for iterators, such as the use of `i` to represent the index in a loop.

Constructors intended for use with new should have a capital first letter (UpperCamelCase).

Variables intended to be used as a [constant](https://en.wikipedia.org/wiki/Constant_(computer_programming)) can be defined with the [SCREAMING_SNAKE_CASE naming convention](https://en.wikipedia.org/wiki/Snake_case). Note that while any variable declared using `const` could be considered a constant, in the context of our application this usage should usually be limited to top-level or exported module values.

```js
const DUMMY_VALUE = 10;

function getIncrementedDummyValue() {
	const incrementedValue = DUMMY_VALUE + 1;
	return incrementedValue;
}
```

## Comments

Comments come before the code to which they refer, and should always be preceded by a blank line unless inserted as the first line in a block statement. Capitalize the first letter of the comment, and include a period at the end when writing full sentences. There must be a single space between the comment token (//) and the comment text.

Single line comments:

```js
someStatement();

// Explanation of something complex on the next line
$( 'p' ).doSomething();
```

When adding documentation, use the [jsdoc](http://usejsdoc.org/) format.

```js
/**
 * Represents a book.
 * @constructor
 * @param {string} title - The title of the book.
 * @param {string} author - The author of the book.
 */
function Book(title, author) {

}

```

Multi-line comments that are not a jsdoc comment should use `//`:

```js
//
// This is a comment that is long enough to warrant being stretched
// over the span of multiple lines.
```

Inline comments are allowed as an exception when used to annotate special arguments in formal parameter lists:

```js
function foo( types, selector, data, fn, /* INTERNAL */ one ) {
    // Do stuff
}
```

## Equality

Strict equality checks (===) must be used in favor of abstract equality checks (==). The only exception is when checking for both undefined and null by way of null.

```js
// Check for both undefined and null values, for some important reason.
if ( undefOrNull == null ) {
    // Expressions
}
```

## Type Checks

These are the preferred ways of checking the type of an object:

- String: `typeof object === 'string'`
- Number: `typeof object === 'number'`
- Boolean: `typeof object === 'boolean'`
- Object: `typeof object === 'object'`
- null: `object === null`
- undefined: `object === undefined` or for globals `typeof window.someGlobal === 'undefined'`

However, you don't generally have to know the type of an object. Prefer testing
the object's existence and shape over its type.

## Existence and Shape Checks

Prefer using the [power](http://www.ecma-international.org/ecma-262/5.1/#sec-9.2)
of "truthy" in JavaScript boolean expressions to validate the existence and shape
of an object to using `typeof`.

The following are all false in boolean expressions:

- `null`
- `undefined`
- `''` the empty string
- `0` the number

But be careful, because these are all true:

- `'0'` the string
- `[]` the empty array
- `{}` the empty object
- `-1` the number


To test the existence of an object (including arrays):
```js
if ( object ) { ... }
```

To test if a property exists on an object, regardless of value, including `undefined`:
```js
if ( 'desired' in object ) { ... }
```

To test if a property is present and has a truthy value:
```js
if ( object.desired ) { ... }
```

To test if an object exists and has a property:
```js
if ( object && 'desired' in object ) { ... }
if ( object && object.desired ) { ... }
```

## Strings

Use single-quotes for string literals:

```js
var myStr = 'strings should be contained in single quotes';
```

When a string contains single quotes, they need to be escaped with a backslash (\\):

Double quotes can be used in cases where there is a single quote in the string or in JSX attributes.

```js
var myStr = "You're amazing just the way you are.";

var component = <div className="post"></div>;
```

## Switch Statements

Switch statements can be useful when there are a large number of cases – especially when multiple cases can be handled by the same block (using fall-through), or the default case can be leveraged.

When using switch statements:

- Note intentional cases of fall-through explicitly, as it is a common error to omit a break by accident.
- Indent case statements one tab within the switch.

```js
switch ( event.keyCode ) {

    // ENTER and SPACE both trigger x()
    case constants.keyCode.ENTER:
    case constants.keyCode.SPACE:
        x();
        break;
    case constants.keyCode.ESCAPE:
        y();
        break;
    default:
        z();
}
```

## Best Practices

### Variable names

The first word of a variable name should be a noun or adjective (not a verb) to avoid confusion with functions.

You can prefix a variable with verb only for boolean values when it makes code easier to read.

```js
// bad
const play = false;

// good
const name = 'John';
const blueCar = new Car( 'blue' );
const shouldFlop = true;
```

### Function names

The first word of a function name should be a verb (not a noun) to avoid confusion with variables.

```js
// bad
function name() {
    return 'John';
}

// good
function getName() {
    return 'John';
}

// good
function isValid() {
    return true;
}
```

### Arrays

Creating arrays in JavaScript should be done using the shorthand `[]` constructor rather than the `new Array()` notation.

```js
var myArray = [];
```

You can initialize an array during construction:

```js
var myArray = [ 1, 'WordPress', 2, 'Blog' ];
```

In JavaScript, associative arrays are defined as objects.

### Objects

There are many ways to create objects in JavaScript. Object literal notation, `{}`, is both the most performant, and also the easiest to read.

```js
var myObj = {};
```

Object literal notation should be used unless the object requires a specific prototype, in which case the object should be created by calling a constructor function with new.

```js
var myObj = new ConstructorMethod();
```

Object properties should be accessed via dot notation, unless the key is a variable, a reserved word, or a string that would not be a valid identifier:

```js
prop = object.propertyName;
prop = object[ variableKey ];
prop = object['default'];
prop = object['key-with-hyphens'];
```

## “Yoda” Conditions #

Since we require strict equality checks, we are not going to enforce "Yoda" conditionals. You're welcome to use them, but the most important consideration should be readability of the conditional.

## Iteration

The functional programming inspired methods that were added in ECMA5 improve readability. Use them throughout.

```js
var posts = postList.map( function( post ) {
	return <Post post={ post } key={ post.global_ID } />;
} );
```

When iterating over a large collection using a for loop, it is recommended to store the loop’s max value as a variable rather than re-computing the maximum every time:

```js
// Good & Efficient
var i, max;

// getItemCount() gets called once
for ( i = 0, max = getItemCount(); i < max; i++ ) {
    // Do stuff
}

// Bad & Potentially Inefficient:
// getItemCount() gets called every time
for ( i = 0; i < getItemCount(); i++ ) {
    // Do stuff
}
```

### React method names

* Methods that return JSX partial code should start with `render`.
* Methods that navigate to a different page should start with `go`.
* Methods that are bound to event handlers should have descriptive names. Don't name methods after event handlers like `onClick`, `onSubmit`, etc. You can use fat arrow functions if it makes handling the event cleaner.
* Avoid prefixing method names with `_`.

```js
// good
const CartComponent = React.createClass( {
    ...

    renderActionButton() {
        if ( this.isLoading() ) {
            return null;
        }

        if ( this.canGoToCheckout() ) {
            return <Button onClick={ this.goToCheckout }>{ this.translate( 'Go to checkout' ) }</Button>;
        } else {
            return <Button onClick={ this.confirmCart }>{ this.translate( 'Confirm cart' ) }</Button>;
        }
    },

    confirmCart() {
        actions.confirmCart( () => {
             this.enableCheckoutStep();
        } );
    },

    goToCheckout() {
        page( this.getCheckoutPath() );
    }
} );
```

JSX is pretty easy to read as standard HTML. That's why separate meta `renderSomething` methods should only be used if there is meaningful functionality happening within the code, not just to break up content.
If you find that you have many separate rendering functions, or rendering functions that include more than a basic amount of logic, it might be a sign that there’s room for splitting off separate new components.

## ES6

We support and encourage ES6 features thanks to [Babel](https://babeljs.io/) transpilation and the accompanying polyfill. There are still a couple of minor caveats to be aware of [regarding Classes](https://babeljs.io/docs/learn-es2015/#subclassable-built-ins).

### Let and Const

You can use `const` for all of your references.

> Why? This ensures that you can't reassign your references (mutation), which can lead to bugs and difficult to comprehend code.

```javascript
// good
var a = 1,
    b = 2;

// better
const a = 1,
    b = 2;
```

If you must mutate references, you can use `let`. Otherwise `const` is preferred.

> Why? `let` is block-scoped rather than function-scoped like `var`.

```javascript
// good
var count = 1;
if ( true ) {
    count += 1;
}

// better
let count = 1;
if ( true ) {
    count += 1;
}
```

Note that both `let` and `const` are block-scoped.

```javascript
// const and let only exist in the blocks they are defined in.
{
    let a = 1;
    const b = 1;
}
console.log( a ); // ReferenceError
console.log( b ); // ReferenceError
```

### Arrow Functions

When you must use function expressions (as when passing an anonymous function), you can use arrow function notation.

> Why? It creates a version of the function that executes in the context of `this`, which is usually what you want, and is a more concise syntax.

> Why not? If you have a fairly complicated function, you might move that logic out into its own function declaration.

```javascript
// good
function Person() {
    this.age = 0;

    setInterval( function() {
        this.age++;
    }.bind( this ), 1000 );
}
let p = new Person();

// better
function Person() {
    this.age = 0;

    setInterval( () => {
        this.age++;
    }, 1000 );
}
let p = new Person();
```

If the function body fits on one line and there is only a single argument, feel free to omit the braces and parentheses, and use the implicit return. Otherwise, add the parentheses, braces, and use a `return` statement.

> Why? Syntactic sugar. It reads well when multiple functions are chained together.

> Why not? If you plan on returning an object.

```javascript
// bad
// it's too magic, it works only with parentheses, otherwise it returns collection of undefine values
[ 1, 2, 3 ].map( x => ( { value: x * x } ) );

// good
[ 1, 2, 3 ].map( x => x * x );

// good
[ 1, 2, 3 ].reduce( ( total, n ) => {
    return total + n;
}, 0 );

```

### Object Property Shorthand

You can use property value shorthand.

> Why? It is shorter to write and descriptive.

```javascript
const lukeSkywalker = 'Luke Skywalker';

// good
const obj = {
    lukeSkywalker: lukeSkywalker,
};

// better
const obj = {
    lukeSkywalker,
};
```

Group your shorthand properties at the beginning of your object declaration.

> Why? It's easier to tell which properties are using the shorthand.

```javascript
const anakinSkywalker = 'Anakin Skywalker',
    lukeSkywalker = 'Luke Skywalker';

// bad
const obj = {
    episodeOne: 1,
    twoJediWalkIntoACantina: 2,
    lukeSkywalker,
    episodeThree: 3,
    mayTheFourth: 4,
    anakinSkywalker,
};

// good
const obj = {
    lukeSkywalker,
    anakinSkywalker,
    episodeOne: 1,
    twoJediWalkIntoACantina: 2,
    episodeThree: 3,
    mayTheFourth: 4,
};
```

### Object Method Shorthand

You can use object method shorthand.

```javascript
// good
const atom = {
    value: 1,

    addValue: function ( value ) {
        return atom.value + value;
    },
};

// better
const atom = {
    value: 1,

    addValue( value ) {
        return atom.value + value;
    },
};
```

### Object Computed Properties

You can use computed property names when creating objects with dynamic property names.

> Why? They allow you to define all the properties of an object in one place.

```javascript

function getKey( k ) {
    return `a key named ${k}`;
}

// good
const obj = {
    id: 5,
    name: 'San Francisco',
};
obj[ getKey( 'enabled' ) ] = true;

// better
const obj = {
    id: 5,
    name: 'San Francisco',
    [ getKey( 'enabled' ) ]: true,
};
```

### Template Strings

When programmatically building up strings, you can use template strings instead of concatenation.

> Why? Template strings give you a readable, concise syntax with proper newlines and string interpolation features.

```javascript
// good
function sayHi( name ) {
    return 'How are you, ' + name + '?';
}

// better
function sayHi( name ) {
    return `How are you, ${ name }?`;
}
```

### Destructuring

You can use object destructuring when accessing and using multiple properties of an object.

> Why? Destructuring saves you from creating temporary references for those properties.

```javascript
// good
function getFullName( user ) {
    const firstName = user.firstName,
        lastName = user.lastName;

    return `${ firstName } ${ lastName }`;
}

// better
function getFullName( user ) {
    const { firstName, lastName } = user;

    return `${ firstName } ${ lastName }`;
}

// best
function getFullName( { firstName, lastName } ) {
    return `${ firstName } ${ lastName }`;
}
```

You can use array destructuring.

```javascript
const arr = [ 1, 2, 3, 4 ];

// good
const first = arr[0],
    second = arr[1];

// better
const [ first, second ] = arr;
```

Use object destructuring for multiple return values, not array destructuring.

> Why? You can add new properties over time or change the order of things without breaking call sites.

```javascript
// bad
// the caller needs to think about the order of return data
function processInput( input ) {
    let left, right, top, bottom;
    // some assignment happens
    return [ left, right, top, bottom ];
}
const [ left, , top ] = processInput( input );

// good
// the caller selects only the data they need
function processInput( input ) {
    let left, right, top, bottom;
    // some assignment happens
    return { left, right, top, bottom };
}
const { left, top } = processInput( input );
```

### Default Parameters

You can use default parameter syntax rather than mutating function arguments.

```javascript
// really bad
function handleThings( opts ) {
    // No! We shouldn't mutate function arguments.
    // Double bad: if opts is falsy it'll be set to an object which may
    // be what you want but it can introduce subtle bugs.
    opts = opts || {};
    // ...
}

// good
function handleThings( opts ) {
    if ( opts === void 0 ) {
        opts = {};
    }
    // ...
}

// better
function handleThings( opts = {} ) {
    // ...
}
```

### Rest

Never use `arguments`, opt to use rest syntax `...` instead.

> Why? `...` is explicit about which arguments you want pulled. Plus rest arguments are a real Array and not Array-like like `arguments`.

```javascript
// bad
function concatenateAll() {
    const args = Array.prototype.slice.call( arguments );

    return args.join( '' );
}

// good
function concatenateAll( ...args ) {
    return args.join( '' );
}
```

### Array Spreads

You can use array spreads `...` to copy arrays.

```javascript
// good
const itemsCopy = items.slice();

// better
const itemsCopy = [ ...items ];
```

### Promises

It's ok to use callbacks to manage your asynchronous flows, but if you need to handle more than one async call, you can use promises to avoid nested callbacks and to have a single exception management point to deal with.

```javascript
// bad
function doSomething( callback ) {
    // do stuff
    callback( error, result );
}
doSomething( function( error, result ) {
    if ( ! error ) {
        doSomethingElse( function( error2, result2 ) {
            if ( ! error2 ) {
                // ad infinitum
            }
        } );
    }
} );

// good
function doSomething() {
    return new Promise( function( resolve, reject ) {
        // do stuff
        if ( stuff ) {
            return resolve( stuff );
        }
        reject( error );
    }
}
doSomething()
    .then( doSomethingElse )
    .then( doSomethingElseMore )
    .catch( function( error ) {
        // manage error
    } );
```

Keep in mind that each function called from a `.then` should return a promise, or else the next `then/catch` in the chain would be always called immediately after it.
