# ECMAScript 2015 (ES6)

## Constants !!

[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const)

```javascript
var someVal = 123;
var someVal = 456;
```

This is valid JavaScript, much to many people's fustration. Luckily since ES6, we are able to use constants by defining values using the `const` keyword

```javascript
const someVal = 123;
someVal = 456; // error
const someVal = 789; // error
```

This only makes the variable itself immutable, not its content, so items in an array or object defined with `const` could be changed.

## Blocked scope variables with let !!

[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let)

As well as `const`, ES6 also introduced another keyword for defining variables: `let`.
Variables defined with `var` are scoped to the immediate function body, whilst `let` variables are scoped to the immidiate code block `{}`. There a few more differences of note, [a great Stackoverflow answer breaks this down in depth](https://stackoverflow.com/questions/762011/whats-the-difference-between-using-let-and-var). In essence, the function scope of `var` was a common cause of bugs in JavaScript and `let` was aimed at changing that.

## Arrow functions !!

[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions)

Ever seen `thiz = this` or `that = this`?
Arrow functions differ to regular functions in many ways, but here I'll point out 2 things:

1. Shorthand syntax

```javascript
function normalFunc(num) {
    return num + 1;
}
```

The same function as an arrow function

```javascript
const arrowFunc = (num) => num + 1;
```

The parenthesis around the parameter are optional if there is only one, the brackets are also optional around the function body as is the `return` keyword if you have a simple one line code body.

2. No separate `this`
   This was an issue when defining 'methods' of 'classes' in JavaScript, where `SomeClass.prototype.someFunction`'s `this` would refer to _itself_, its own `this` rather than the instance. Arrow functions have no `this`, and using `this` in an arrow function that is an object method will refer to the instance. Nice.

## Default parameter values

[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Default_parameters)

When defining a function you may want some defaults if an argument isn't passed.
Before ES6 you would have had to write some `if` logic inside the function like

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

## Rest parameter !!

[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/rest_parameters)

'Rest' refers to remaining arguments passed to a function. In ES5 you'd end up with messy unreadable Array prototype functions to make this happen, but ES6:

```javascript
function f(a, b, ...restOfArgs) {
    // ...
}
```

## Spread operator !!

[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax)

Spread an array's items into a function's arguments or another array

```javascript
const someArr = [1, 2, 3];
const someOtherArr = [...someArr, 4, 5, 6]; // [1, 2, 3, 4, 5, 6]
```

## Template literals !!

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

Adding methods/functions to objects now omits the `function` keyword when defining the object. Also when defining your object you can have computed names like so;

```javascript
const num = 1;
const obj {
  [num + num]: "two"
}
```

## Destructuring !!

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

When destructuring you can include defaults for if a value isn't there. For more details check out the MDN docs.

## Modules !!

[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import)

Modules are a game changer, they allow for importing and exporting of values to and from modules without global namespace pollution.

```javascript
// some-library.js
export const myObj = { a: 1, b: 2 };

// SomeApp.js
import { myObj } from 'some-library';
myObj.a; // 1
```

You can mark values for export and also mark default exports. Default exports can be imported in directly whilst other exports can be imported in using `{}`. Whilst browser support is still iffy for modules, it's common to use a bundler like [Webpack](https://webpack.js.org/) to bundle all your modules together for delivery to the browser.

## Classes !!

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

Classes can inherit from other classes with the `extends` keyword, it also offers easy access to its base class constructor with `super()`, and methods like `toString()`. You can also declare static methods with the `static` keyword. You can also set getters and setters directly within the class.

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

There is a lot to know about ES6 classes, refer to the MDN docs.

## Symbols

[MDN](https://developer.mozilla.org/en-US/docs/Glossary/Symbol)

ES6 introduces another primitive type called symbols. They are essentially tokens that serve as unique IDs. You can create them like so

```javascript
let sym1 = Symbol('sym');
let sym2 = Symbol('sym');
sym1 === sym2; // false
```

Every symbol, whether passed (the optional) value or not, is unique, comparing two of them will never return `true`.

## Iterators and generators

[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Iterators_and_Generators)

Iterators are new constructs (in JavaScript) for iterating across data, giving us the ability to access data via an interface which determins how we can iterate over the object.
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
let lettersIterator = [Symbol.iterator]();
lettersIterator.next(); // {value: "a", done: false}
// ...
lettersIterator.next(); // {value: undefined, done: true}
```

The iterations return an iterator object that contains the value and a `done` property that returns true when iterating has complete. We can now also create custom interators for our objects. They are objects with one property: `[Symbol.iterator]`, which will resolve to a unique value, unable to be duplicated. This property's value is a function which returns an object. To call this, we would

```javascript
const characters = {
  nintendo: 'Mario',
  microsoft: 'Master Chief,
  sega: 'Sonic'
}

let count = 0;
const iterable = {
  [Symbol.iterator]() {
    return {
      next() {
        count++;
        switch(count) {
          case 1:
            return {value: chracters.nintendo, done: false;}
            // ...
        }
      }
    }
};

let iterable = iterator[Symbol.iterator]();

iterable.next(); // { value: 'Mario', done: false }
```

Generators are in essence functions that are wrappers for iterators. They are created with `function*`, and returns a generator object. These functions can be 'paused' between returning values using the `yield` keyword.

```javascript
function* generatorFunc(i) {
    yield i;
    yield i + 1;
}

let generator = generatorFunc(2); // Creates generator object
generator.next(); // {value: 5, done: false}
```

## Set (data structure)

[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set)

JavaScript now has sets, these are lists of values similar to arrays but can only contain unique values. They have their own methods with names that differ from arrays for some reason.

```javascript
let s = new Set();
s.add(1);
s.add(2);
s.add(1);
s.size; // 2
```

## Map (data structure)

[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map)

A map object, like normal objects, holds key-value pairs and remembers the order of insertion, it takes any value as keys or values, making them different from normal objects.

```javascript
const mapObj = new Map();
mapObj.set(Symbol(), 1);
map.Obj.set({ keyName: 'key' }, 'value');
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
const 8bitIntTypedArray = new Int8Array();
```

There are signed, unsigned, and clamped varients for 8, 16, 32, float32, float64 types. Support for `bigint`s was added in ES7.

In addition is a container called `ArrayBuffer`, which is a generic, fixed-length, raw binary data buffer. You cannot interact with it directly, for that we make use of the `DataView`, this is what has the `get` and `set` methods. Typed arrays shared a lot of funtionality/methods as normal arrays such as `forEach`, `map` etc.

```javascript
const buffer = new ArrayBuffer(16); // creates 16 byte buffer
const view = new DataView(buffer); // DataView to access/set the whole buffer with

view.setInt8(1, 10); // in slot 1 set 10
view.getInt8(1); // returns 10
```

TODO:

## New built in methods

Adding functionality to existing types: objects, arrays, strings and numbers

1. `Object.assign()`, can assign enumerable properties of one or more source objects into a destination object

```javascript
let dest = { prop1: 'a' };
let src1 = { prop2: 'b' };
let src2 = { prop3: 'c', prop4: 'd' };

Object.assign(dest, src1, src2);
dest.prop2; // "b"
dest.prop4; // "c"
```

2. `Array.find()` and `Array.findindex()` are two new functions to find elements in arrays.
   `Array.find()` returns the value of the first element in the array that satisfies the testing function.
   `Array.findIndex()` will return the value's index.

```javascript
[1, 3, 4, 2]
    .find((x) => x > 3) // 4
    [(1, 3, 4, 2)].findIndex((x) => x > 3); // 2
```

3. `String.repeat()` gets passed a number of times to return a repeat of the string it is called on.

```javascript
let yo = 'yo';
yo.repeat(5); // yo yo yo yo yo
```

4. 5 new methods for strings that allow searching for substrings. These string methods are all self explanatory, the optional argument is the position in the string to start searching.

```javascript
'hello'.startsWith('ello', 1); // true
'hello'.endsWith('hell', 4); // true
'hello'.includes('ell'); // true
'hello'.includes('ell', 1); // true
'hello'.includes('ell', 2); // false
```

5. `Number.isNan()` and `Number.isFinite()` Number type checking are new built in methods to check for these specific types of numbers.

6. `Number.isSafeInteger()` is a method that determines whether the provided value is a number that is a 'safe' integer (hence the name), this mean it can:

-   be exactly represented as an IEEE-754 double precision number
    and its
-   IEEE-754 representation cannot be the result of rounding any other integer to fit the IEEE-754 representation.

It can return `true` or `false`.

7. `Number.EPSIOLON` isn't a method, it holds a value which is `2.2204460492503130808472633361816E-16`, which represents the difference between 1 and the smallest floating point number greater than 1.

8. `Math.trunc()` is a method that drops the fractional part of a number, like a round down to a whole number.

```javascript
Math.trunc(11.6); // 11
```

9. `Math.sign()` method either returns a positive or a negative, in other words, if passed a positive number it will return `1`, or `-1` for a negative number. It will return `0` if passed a `0`, same with `-0`. Anything else will return `NaN`.

## Promises !!

[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)

Goodbye `$.ajax()`! ES6 brings us built in promises, which are objects that represent the eventual completion (or failure) of an ansychronous operation and its resulting value.
There is a lot to know about promises, but in essence you tell JavaScript to do something, and once done _then_ run some code, or if it fails, run some other code, a simplified example below:

```javascript
doSomethingAsync().then(doSomethingOnSuccess).catch(doSomethingOnFail);
```

Promises are a powerful and complex tool in JavaScript, and are especially useful for making calls to APIs, for example when using `fetch()` (browser API), as calling `fetch()` returns a promise.

```javascript
fetch('http://www.someurl.com')
    .then(doSomethingWithReturnedData)
    .catch(logErrorMessage);
```

These are simplifed examples, `then`s can be chained to allow for a lot of functionality to run on resolve/reject cases of a promise.
The functions to run on resolve or reject are non blocking, and will only run once the call stack is clear and all normal code has finished running.

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
                throw new TypeError('Age is not an integer');
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
person.age = '100'; // returns TypeError - 'Age is not an integer'
person.age = 999; // returns RangeError - 'The age is too high'
```

The `proxy` object is highly customizable and is able to assist in real world applications with things like validation.

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

## Other

-   Direct support for safe binary and octal literals.
-   Extended support for using unicode within strings and regular expressions.
-   Enhanced regular expressions.
