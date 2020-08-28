# ECMAScript 2015 (ES6)

## Constants !!!

[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const)

`const` allows us to now declare constants in JS.

```javascript
const someVal = 123;
someVal = 456; // error
const someVal = 789; // error
```

`const` will make sure you don't change the type of data you declare initially, but you can mutate data like objects and arrays, this is valid.

```javascript
const someArr = [];
someArr.push(123);
someArr.push(456); // [123, 456] this won't error
```

## Blocked scope variables with let !!!

`let` is another new way to declare variables, and it differs from `var` in a few ways. The first is

#### Scope

consider the function below

```javascript
function example() {
    var bool = true;

    if (bool) {
        var someVar = 'var';
        let someLet = 'let';
    }

    console.log(someVar); // no problems here
    console.log(someLet); // ReferrenceError
}
```

`var` variables **function scoped**, so the variable declared in our if block was available elsewhere inside the `example` function. Our `let` variable is **block scoped** which means when declared, it's only available inside the block (in this case the `if` block) it's declared in.

#### Hoisting

Variables declared with `var` are hoisted, this means they are available in the script (as in, created yet undefined) before actually being declared. This isn't the case with `let`, see the example:

```javascript
console.log(foo); // undefined
var foo = 'foo';

console.log(bar); // ReferrenceError
let bar = 'bar';
```

#### Redeclaring

You can redeclare a variable with `var`, now whilst you can actually do the same with `let`, you cannot do it with `let` in strict mode. Remember, modules are in strict mode by default. Trying to do so will cause an error

```javascript
var foo = 'foo';
var foo = 'foo'; // this is fine

let bar = 'bar'; // ReferrenceError (in strict mode)
let bar = 'bar';
```

## Arrow functions !!!

[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions)

Arrow functions are a short-hand alternative to regular functions. They have a shorthand syntax that have the following rules:

-   parameters don't need to be wrapped if there is only one
-   simple functions can omit the return keyword

```javascript
function normalFunc(num) {
    return num + 1;
}
```

The same function as an arrow function

```javascript
const arrowFunc = (num) => num + 1; // return keyword omitted
```

Arrow functions do not have its own bindings to a number of keywords in JS; `this`, `arguments`, `super`, or `new.target`, so they wouldn't be useful as methods for objects.

## Default parameter values

[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Default_parameters)

When defining a function you may want some defaults if an argument isn't passed.
Before ES6 you would have had to write some `if` logic inside the function like this

```javascript
function f(a, b, c) {
    if (a === undefined) {
        a = 1; // set default
    }
}
```

ES6 introduced the functionality to just pass this into the function like so:

```javascript
function f(a = 1, b, c) {
    // ...
}
```

## Rest parameter !!!

[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/rest_parameters)

'Rest' refers to remaining arguments passed to a function consider the following function:

```javascript
function f(a, b, ...restOfArgs) {
    // ...
}
```

The first two arguments passed to this function would be referenced as `a` and `b`, whilst any number of additional arguments would be accessible as an array referenced as `restOfArgs`.

## Spread operator !!!

[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax)

Spread an array's items into a function's arguments or another array

```javascript
const someArr = [1, 2, 3];
const someOtherArr = [...someArr, 4, 5, 6]; // [1, 2, 3, 4, 5, 6]
```

## Template literals !!!

[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals)

In essense, this allows for interpolation, you can now pull values and expressions into strings directly with a more comfortable syntax. Instead of using single or double quote marks, template literals are written using backticks ` `` `

```javascript
const name = 'Ross';
const city = 'London';
const intro = "Hi I'm" + name + ' from ' + city;
```

becomes

```javascript
const name = 'Ross';
const city = 'London';
const intro = `Hi I'm ${name} from ${city}`;
```

It also supports mutli line and indented strings, no need to add `\t` and so on. This leads to much cleaner and easier to read strings, especially if you are writing something like HTML in JavaScript:

```javascript
const htmlBlock = `
    <header>
        <h1>${title}</h1><i class=${logoClass}></i>
    </header>
`;
```

You can also use them to access raw strings like so;

```javascript
console.log(String.raw`hello\nnot a newline`); // hellonot a newline
```

## Enhanced object properties

Short syntax for object properties if you're setting it to a value with the same name:

```javascript
const num = 1;
const str = 'abc';

const obj = { num, str }; // Instead of num: num, str: str
```

Adding methods/functions to objects now omits the `function` keyword when defining the object.

```javascript
const foo = {
    oldWay: function() {...},
    newWay() {...}
}
```

Also when defining your object you can have computed names like so;

```javascript
const num = 1;
const obj {
  [num + num]: "two"
}
```

## Destructuring !!!

[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)

In essence this is pulling values out of arrays and objects directly into variables/constants. Below we declare an array and an object, and pull out values into constants.

```javascript
const arr = [1, 2, 3];
const [a, , b] = arr;
a; // 1
b; // 3

const obj = { val1: 123, val2: 456 };
const { val2 } = obj;
val2; // 456
```

When destructuring you can include defaults for if a value isn't there like this

```javascript
const arr = ['a', 'c'];
const [a, b = 'b', c] = arr; // defaults b to 'b' if found to be undefined
```

## Modules !!!

[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import)

Modules are not a new concept for JavaScript, but ES6 introduced ESM, the official ECMAScript module system.

```javascript
// some-library.js
export const myObj = { a: 1, b: 2 };

// some-other-library.js
default export class MyClass {
    ...
}

// SomeApp.js
import MyClass from 'some-other-library';
import { myObj } from 'some-library';
myObj.a; // 1
```

Values can be named or default exports like in the example above. Default exports can be imported in directly whilst other exports can be imported in using destructuring `{}`. Whilst browser support is generally good for modules, it's common to use a bundler like [Webpack](https://webpack.js.org/) to bundle all your modules together for delivery to the browser.

## Classes !!!

[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes)

Whilst ES6 classes are just syntactic sugar for prototype objects, they make creating objects or 'classes' in JavaScript more inline with other programming languages, there is even a constructor function to set properties of the 'class'.

```javascript
function User(name, email) {
    this.name = name;
    this.email = email;
}

User.prototype.someMethod = function () {};
```

Can now be written as:

```javascript
class User {
    constructor(name, email) {
        this.name = name;
        this.email = email;
    }

    someMethod: () => {};
}
```

Classes can inherit properties from other classes with the `extends` keyword, it also offers easy access to its base class constructor with `super()`, and methods like `toString()`. You can also declare static methods with the `static` keyword. You can also set getters and setters directly within the class.

```javascript
class Phone {
  constructor(screenWidth, screenHeight) {
    this._screenWidth = _screenWidth;
    this.screenHeight = screenHeight;
  }

  set width (screenWidth) {this._screenWidth = screenWidth}
  get width () {return this._screenWidth}
  get screenSize () {return return this._screenWidth * return this.screenHeight}
}
```

Under the hood these are still JS objects.

## Symbols

[MDN](https://developer.mozilla.org/en-US/docs/Glossary/Symbol)

ES6 introduces another primitive type called symbols. They are essentially tokens that serve as unique IDs. You can create them like so

```javascript
let sym1 = Symbol('sym');
let sym2 = Symbol('sym');
sym1 === sym2; // false
```

Every symbol, whether passed (the optional) value or not, is unique, comparing two of them will never return `true`.

Symbols are particularly useful as unique keys for objects, consider the following example:

```javascript
function addProp(object, key, value) {
    object[key] = value;
}

const obj = {};
const somePropSymbol1 = Symbol();
const somePropSymbol2 = Symbol();

addProp(obj, somePropSymbol1, 32);
addProp(obj, somePropSymbol2, 44);

// obj: {Symbol(): 32, Symbol(): 44}
```

Whilst `Object.keys` won't return our `Symbol` properties, someone could still access it with `Reflect.ownKeys`, so it isn't truely private perse, but it will always be unique in an object / class.

## Iterators and generators

[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Iterators_and_Generators)

Iterators (in this ES6 format) are new constructs (in JavaScript) for iterating across data, giving us the ability to access data via an interface which determines _how_ we can iterate over the object.
An example:

```javascript
for (let letter of ['a', 'b', 'c']) {
    console.log(letter);
}
// a, b, c
```

This looks like regular looping, but under the hood, this construct lets the object decide what data it gives out when iterated, in this case it is a normal array so it just pulls out the 'next' item. This is really calling `.next()`, a built in method that returns the next item in the array. `String`, `Array`, `Map`, `Set` all have this functionality built in, but normal `Object`s do not.

```javascript
let letters = ['a', 'b', 'c'];
let lettersIterator = letters[Symbol.iterator]();
lettersIterator.next(); // {value: "a", done: false}
// ...iterate until the end
lettersIterator.next(); // {value: undefined, done: true}
```

We can define an object as an iterable by implementing the `iterator` method (`[Symbol.iterator()]`).

Generators are in essence functions that are wrappers for iterators. They are created with `function*`, and return a generator object. These functions can be 'paused' between returning values using the `yield` keyword.

```javascript
function* generatorFunc(i) {
    yield i;
    yield i + 1;
}

let generator = generatorFunc(2); // Creates generator object
generator.next(); // {value: 2, done: false}
```

## Set (data structure)

[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set)

JavaScript now has sets, these are lists of values similar to arrays but can only contain unique values, duplicates will not be added. They have their own methods with names that differ from arrays for some reason.

```javascript
const s = new Set();
s.add(1);
s.add(2);
s.add(1); // won't be added as set already has 1
s.size; // 2
```

## Map (data structure)

[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map)

A map object, like normal objects, holds key-value pairs and remembers the order of insertion, it takes any value as keys or values, making them different from normal objects.

```javascript
const mapObj = new Map();
mapObj.set(Symbol(), 1);
map.Obj.set({ keyName: 'key' }, 'value'); // this key is an object, invalid for regular objects
```

## Weakset and Weakmap (data structure)

[MDN weakset](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/WeakSet)
[MDN weakmap](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/WeakMap)

These are similar, but slightly different to `Map` and `Set`.
`WeakSet`s are collections of **objects only**, and cannot take other types like normal `Set`s can. They are also 'weak' links, for example if you have an object created in your global context, and set that as an item in the `WeakSet` with `weakset.add(globalObj)`, and _no other_ code referenced that `globalObject`, then it could be garbage collected. This can be helpful when considering memory allocation.

`WeakMap` are similar with how their 'weak' link works without outside objects, and they also can only have objects as their keys.
[A good article explaining the difference betwee Map and WeakMap](https://www.mattzeunert.com/2017/01/31/weak-maps.html).

## Typed arrays

[MDN typed arrays](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/TypedArray)
[MDN array buffer](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/ArrayBuffer)

Typed arrays are arrays where the contents are strictly controlled, every element in the array will be restricted to a certain kind of number, 8bit integer, 32 point float etc. There are a lot of APIs which make use of typed arrays such as Web GL, Canvas, Fetch, Web Workers, web audio etc.

Say we wanted to create an array of only 8bit integers, we would use one of the new constructors (see MDN for a full list) for 8bit integers which is `Int8Array()`

```javascript
const typedArray = new Int8Array(8);
// [0, 0, 0, 0, 0, 0, 0, 0]
typedArray[0] = 32;
typedArray[0]; // 32
typedArray[1] = 'some string'; // won't be set
typedArray[10] = 123;
typedArray[10]; // undefined as only 8 indexes
```

There are signed, unsigned, and clamped varients for 8, 16, 32, float32, float64 types. Support for `bigint`s was added in ES2020.

In addition to this is a container called `ArrayBuffer`, which is a generic, fixed-length, raw binary data buffer. You cannot interact with it directly, for that we make use of the `DataView`, this is what has the `get` and `set` methods. Typed arrays share a lot of funtionality/methods as normal arrays such as `forEach`, `map` etc.

```javascript
const buffer = new ArrayBuffer(16); // creates 16 byte buffer
const view = new DataView(buffer); // DataView to access/set the whole buffer with

view.setInt8(1, 10); // in slot 1 set 10
view.getInt8(1); // returns 10
```

## New built in methods

Added functionality to existing types: objects, arrays, strings and numbers

#### Object.assign()

`Object.assign()` can assign enumerable properties of one or more source objects into a destination object

```javascript
let dest = { prop1: 'a' };
let src1 = { prop2: 'b' };
let src2 = { prop3: 'c', prop4: 'd' };

Object.assign(dest, src1, src2);
dest.prop2; // "b"
dest.prop4; // "c"
```

#### Array.prototype.find() and findIndex()

`Array.prototype.find()` and `Array.prototype.findIndex()` are two new functions to find elements in arrays. `Array.prototype.find()` returns the value of the first element in the array that satisfies the testing function. `Array.prototype.findIndex()` will return the value's index.

```javascript
[1, 3, 4, 2]
    .find((x) => x > 3) // 4
    [(1, 3, 4, 2)].findIndex((x) => x > 3); // 2
```

#### String.prototype.repeat()

This new method gets passed a number of times to return a repeat of the string it is called on.

```javascript
let yo = 'yo';
yo.repeat(5); // yo yo yo yo yo
```

#### String substring methods

5 new methods for strings that allow searching for substrings. These string methods are all self explanatory, the optional argument is the position in the string to start searching.

```javascript
'hello'.startsWith('ello', 1); // true
'hello'.endsWith('hell', 4); // true
'hello'.includes('ell'); // true
'hello'.includes('ell', 1); // true
'hello'.includes('ell', 2); // false
```

#### Number.isNan() and isFinite()

`Number.isNan()` and `Number.isFinite()` Number type checking are new built in methods to check for these specific types of numbers.

#### Number.isSafeInteger()

`Number.isSafeInteger()` is a method that determines whether the provided value is a number that is a 'safe' integer (hence the name), this mean it can:

-   be exactly represented as an IEEE-754 double precision number
    and its
-   IEEE-754 representation cannot be the result of rounding any other integer to fit the IEEE-754 representation.

It can return `true` or `false`.

#### Number.EPSIOLON

`Number.EPSIOLON` is a new property, it holds the value `2.2204460492503130808472633361816E-16`, which represents the difference between 1 and the smallest floating point number greater than 1.

#### Math.trunc()

`Math.trunc()` is a method that drops the fractional part of a number, like a round down to a whole number. It differs from `Math` object's other methods as it doesn't actually do any math, it simply cuts off the `.` and any numbers after it, no matter if it's a positive or negative number.

```javascript
Math.trunc(11.6); // 11
```

#### Math.sign()

`Math.sign()` method either returns a positive or a negative, in other words, if passed a positive number it will return `1`, or `-1` for a negative number. It will return `0` if passed a `0`, same with `-0`. Anything else will return `NaN`.

## Promises !!!

[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)

ES6 brings us built in promises, which are objects that represent the eventual completion (or failure) of an ansychronous operation and its resulting value.
There is a lot to know about promises, but in essence you tell JavaScript to do something, and once done _then_ run some code, or if it errors, run some other code, a simplified example below:

```javascript
doSomethingAsync().then(doSomethingOnSuccess).catch(doSomethingOnFail);
```

Promises are a powerful and complex tool in JavaScript, and are especially useful for making calls to external APIs, for example when using `fetch()` (browser API), as calling `fetch()` returns a promise.

```javascript
fetch('http://www.someurl.com') // after a delay, data returns
    .then(doSomethingWithReturnedData) // if successful, pass to this function
    .catch(logErrorMessage); // if failed, do something with the error
```

These are simplifed examples, `then`s can be chained to allow for a lot of functionality to run on resolve/reject cases of a promise.
The functions run on resolve or reject are non blocking, whilst described as 'asynchronous' JS, what's really happening is the functionality is being **deffered** by getting moved to a special call stack and running after all global code has finished executing.

You can create your own promise objects, below is a simple example where a "call" for a user from a database is made inside a promise. If the call is successful, we call resolve and pass the data we retrieve. If the call fails for some reason, we pass in an error message.
Further down we add some functionality for what should happen should the promise succeed or fail. This code will happen once it get's resolved and the main call stack is clear.

```javascript
let getData = new Promise((resolve, reject) => {
    setTimeout(() => {
        const exampleData = { name: 'Ross', job: 'Frontend developer' };
        let somethingWrong = false;
        somethingWrong ? reject('Error') : resolve(exampleData);
    }, 500);
});

getData
    .then((data) => {
        console.log(data.name);
    })
    .catch((errorMsg) => {
        console.log(errorMsg);
    });

// Run other code ...

// "Ross"
```

## Proxying

[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy)

The `Proxy` object is used to define custom behavior for fundamental operations in JavaScript (e.g. property lookup, assignment, enumeration, function invocation, etc). Let's say we wish to make a person object, with properties that can only have certain value types.
When creating a proxy instance, you need to pass it two things:

-   the target object to wrap with `Proxy`
-   The handler object which has functions that define the proxy objects behavior

The handler object can add custom functionality via its methods known as 'traps', below we'll use `set`, which is a trap for setting property values.

```javascript
const handlerObject = {
    set: (obj, prop, value) => {
        if (prop === 'age') {
            if (!Number.isInteger(value)) {
                throw new TypeError('Age should be integer');
            }

            if (value > 200) {
                throw new RangeError('The age is too high');
            }
        }

        obj[prop] = value; // If all tests pass, add the key-value into the object

        return true; // indicate success
    },
};

const person = new Proxy({}, handlerObject);
person.age = 100; // Passes validation
person.age = '100'; // returns TypeError - 'Age should be an integer'
person.age = 999; // returns RangeError - 'The age is too high'
```

The `proxy` object is highly customizable and is able to assist in real world applications with things like validation. [TypeScript](https://www.typescriptlang.org/) is a superset of JS that makes use of type-checking and interfaces, however there isn't currently a way in TypeScript to set limits on a property like we do with the `proxy` object. This can only be checked / enforced at runtime.

## Reflect object

[MDN comparing object and Reflect methods](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Reflect/Comparing_Reflect_and_Object_methods)
[MDN Reflect](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Reflect)

The `Reflect` object provides methods to interact with objects programatically. Like `Math` it isn't something you create an instance of, instead it has static methods we can call.

Below is an example of one of it's methods; `Reflect.ownKeys()` it returns an array of the target object's own property keys. This doesn't return any inherited keys like `Object.keys()`.

```javascript
const obj = { a: 1, b: 2 };
Reflect.ownKeys(obj); // [a, b];
```

There are many methods for the `Reflect` object, read the documentation for a full list.

## International and localization support

[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Intl)

There is now an object `Intl` which provides language sensitive string comparison, number formatting, and date and time formatting. The new constructors are:
`Intl.Collator`
`Intl.NumberFormat`
`Intl.NumberFormat`
`Intl.DateTimeFormat`
An example of something you can do with this:

```javascript
let britishPounds = new Intl.NumberFormat('en-GB', {
    style: 'currency',
    currency: 'GBP',
});
britishPounds.format(10300); // "£10,300.00"
```

This is useful for properly formatting dates and numbers with the use of a locale string like `en-US`

## Octal and binary literals

You can now create octal literals with the `0o` prefix, and it will only accept characters 0 -7

```javascript
let a = 0o54;
let b = 0o58; // SyntaxError
```

ES6 also added support for binary literals with the `0b` prefix, valid numbers are `1` and `0`

```javascript
let a = 0b111;
```

## Regex / string extended unicode support

Strings have some additional methods that can assist with unicode quirks.

#### String.prototype.codePointAt()

`String.prototype.codePointAt()` is a method that returns the unicode code point value.

```javascript
let star = '★';
star.codePointAt(0); // '9733'
```

#### String.fromCodePoint()

`String.fromCodePoint()` is a method that returns a string, it takes the code point value

```javascript
String.fromCodePoint(9733); // ★ (star emoji)
```

#### String.prototype.normalize()

`String.prototype.normalize()` is a method that can normalize unicode values, as the same character could be created with different unicode string.

```javascript
let string1 = '\u00F1';
let string2 = '\u006E\u0303';

console.log(string1); //  ñ
console.log(string2); //  ñ
```

`nomalize()` takes a form parameter and will normalize the unicode string to the specified form.

## Enhanced regular expressions

#### Sticky flag /y

ES6 introduces a new 'sticky' flag `/y` which starts searching for matches after `re.lastIndex` (this is the index _after_ the last match). If matched, it will set the `re.lastIndex` once again to the index after this match.

```javascript
const str = 'hello world';
const regex = /llo/y;

regex.test(str); // true, finds a match, lastIndex is now 4
regex.test(str); // false, no match after index position 4
```

There is also a `re.sticky` propery which will return true if the regex has the sticky flag.

#### Unicode flag /u

This new flag turns on a special unicode mode for regex. This allows using unicode point code escape sequences like `\u{1T52A}`. Let's say you are trying to match the sequence against its emoji, without the `/u` flag, this wouldn't work

```javascript
/\u{1f40e}/.test('🐎'); // false
```

This is because normal regex won't read this correctly having to escape the `/u` at the start. Adding the `/u` flag gives regex a different set of rules, and the below is now checked correctly.

```javascript
/\u{1f40e}/u.test('🐎'); // true
```

Regex also now have a `unicode` property which returns true if the regex makes use of the `/u` flag.

#### flags property

You can now get the flags from a regex by accessing its `flags` property

```javascript
/hello/gi.flags; // gi
```

#### RegExp() copies

You can now use the regex construtor to create copies and even change flags of your regex.

```javascript
const regex = /abc/gi; // i and g flags set
new RegExp(regex, 'i'); // /abc/i
```
