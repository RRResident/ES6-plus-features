# ECMAScript 2015 (ES6) +

## Intro

JavaScript has changed a lot since the days of jQuery. Early iterations of JavaScript could be discribed as lacking in functionality, and at the time the functionality it did have, had mixed support in the popular browsers. jQuery was a library that added a lot of functionality on top of JavaScript, made it more readable (some might say) and allowed for more consistancy among how it was implemented in browsers.

Esentially, when ECMAScript 2015 (also known as ES6) was released, it included a **LOT** of new functionality that made an additional library like jQuery unnecessary. ES6 and onwards is well supported in modern browsers, however many developers still feel it is best practice to transpile code down to ES5 with a tool like Babel. Luckily this is simple to implement into modern build tools / processes.

This is a guide I have wrote for myself and to anyone who wants to see what has changed in JavaScript since ES5, and what some of its modern features are.

Read on or jump to a section:

1. [ECMAScript 2015 (ES6)](#es6)
1. [ECMAScript 2016 (ES7)](#es7)
1. [ECMAScript 2017 (ES8)](#es8)
1. [ECMAScript 2018 (ES9)](#es9)
1. [ECMAScript 2019 (ES10)](#es10)
1. [ECMAScript 2020 (ES11)](#es11)
1. [Resources](#resources)

### <a id="es6"></a>ECMAScript 2015 (ES6)

There were a **LOT** of changes/features introduced with ES6, to focus on the more interesting features, I've marked them with `!!`, so you can search for that if you don't want to jump to the bigger features.

#### Constants !!

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

This only makes the variable itself immutable, not its content, so items in an array or object could be changed.

#### Blocked scope variables with let !!

As well as `const`, ES6 also introduced another keyword for defining variables: `let`.
Variables defined with `var` are scoped to the immediate function body, whilst `let` variables are scoped to the immidiate code block `{}`. There a few more differences of note, [a great Stackoverflow answer breaks this down in depth](https://stackoverflow.com/questions/762011/whats-the-difference-between-using-let-and-var). In essence, the function scope of `var` was a common cause of bugs in JavaScript and `let` was aimed at changing that.

#### Arrow functions !!

Ever seen `thiz = this` or `that = this`?
Arrow functions differ to functions in many ways, for a fuller and indepth explination [check out the MDN docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions) but here I'll point out 2 things:

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

2. No separate `this`
   This was an issue when defining 'methods' of 'classes' in JavaScript, where `SomeClass.prototype.someFunction`'s `this` would refer to _itself_, its own `this` rather than the instance. Arrow functions have no `this`, and using `this` in an arrow function that is an object method will refer to the instance. Nice.

#### Default parameter values

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

#### Rest parameter !!

'Rest' refers to remaining arguments passed to a function. In ES5 you'd end up with unreadable Array prototype functions to make this happen, but ES6:

```javascript
function f(a, b, ...restOfArgs) {
  // ...
}
```

#### Spread operator !!

Spread an array's items into a function arguments or another array

```javascript
const someArr = [1, 2, 3];
const someOtherArr = [...someArr, 4, 5, 6]; // [1, 2, 3, 4, 5, 6]
```

#### Template literals !!

In essense, this allows to pull values into strings (interpolation?) with a more comfortable syntax.

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

Template literal's functionality goes beyond this into custom interpolation and raw string access. TODO:

#### Enhanced object properties

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

#### Destructuring

In essence this is pulling values out of arrays and objects.

```javascript
const arr = [1, 2, 3];
const [a, , b] = arr;
a; // 1
b; // 3

const obj = { val1: 123, val2: 456 };
const { val2 } = obj;
val2; // 456
```

When destructuring you can include defaults for if a value isn't there. There is more to know about destructuring [if you are interested](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment).

#### Modules !!

Modules are a game changer, they allow for importing and exporting of values to and from modules without global namespace pollution.

```javascript
// some-library.js
export const myObj = { a: 1, b: 2 };

// SomeApp.js
import { myObj } from 'some-library';
myObj.a; // 1
```

You can mark values for export and also mark default exports. Default exports can be imported in directly whilst other exports can be imported in using `{}`. Whilst browser support is still iffy for modules, it's common to use a bundler like [Webpack](https://webpack.js.org/) to bundle all your modules together for delivery to the browser.

#### Classes

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

There is a lot to know about ES6 classes, [further reading here on the MDN docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes).

#### Symbols

ES6 introduces another primitive type called symbols. They are essentially tokens that serve as unique IDs. You can create them like so

```javascript
let sym1 = Symbol('sym');
let sym2 = Symbol('sym');
sym1 === sym2; // false
```

Every symbol, whether passed (the optional) value is unique, comparing two of them will never return `true`. For more details on what they are and why one might use them, [check out the MDN docs](https://developer.mozilla.org/en-US/docs/Glossary/Symbol).

#### Iterators

TODO:

#### Generators

TODO:

#### Set (data structure)

JavaScript now has sets, these are lists of values similar to arrays but can only contain unique values. They have their own methods with names that differ from arrays for some reason.

```javascript
let s = new Set();
s.add(1);
s.add(2);
s.add(1);
s.size; // 2
```

#### Map (data structure)

TODO:

#### Weakset and Weakmap (data structure)

TODO:

#### Typed arrays

TODO:

#### New built in methods

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

- be exactly represented as an IEEE-754 double precision number
  and its
- IEEE-754 representation cannot be the result of rounding any other integer to fit the IEEE-754 representation.

It can return `true` or `false`.

7. `Number.EPSIOLON` isn't a method, it holds a value which is `2.2204460492503130808472633361816E-16`, which represents the difference between 1 and the smallest floating point number greater than 1.

8. `Math.trunc()` is a method that drops the fractional part of a number, like a round down.

```javascript
Math.trunc(11.6); // 11
```

9. `Math.sign()` method either returns a positive or a negative, in other words, if passed a positive number it will return `1`, or `-1` for a negative number. It will return `0` if passed a `0`, same with `-0`. Anything else will return `NaN`.

#### Promises !!

Goodbye `$.ajax()`! ES6 brings us built in promises, which are objects which represent the eventual completion (or faliure) of an ansychronous operation and its resulting value.
There is a lot to know about promises and how they work, so of course [the MDN docs are a good place to start](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise), but in essence you tell JavaScript to do something, and once done _then_ run some code, or if it fails, run some other code, a simplified example below:

```javascript
doSomethingAsync().then(doSomethingOnSuccess).catch(doSomethingOnFail);
```

Promises are a powerful and complex tool in JavaScript, and are especially useful for making calls to APIs, for example when using `fetch()` (browser API), as calling `fetch()` returns a promise.

```javascript
fetch('http://www.someurl.com')
  .then(doSomethingWithReturnedData)
  .catch(logErrorMessage);
```

These are simplifed examples, `then`s can be changed to allow for a lot of functionality to run on resolve/reject cases of a promise.
The functions to run on resolve or reject are non blocking, and will only run once the call stack is clear and all normal code has finished running.

#### Proxying

Using the `Proxy` object you can hook into runtime-level object meta-operations TODO:

#### Reflecting object keys

`Reflect.ownKeys()` is a method that returns an array of the target object's own property keys.

```javascript
const obj = { a: 1, b: 2 };
Reflect.ownKeys(obj); // [a, b];
```

#### International and localization support

There is now an object `Intl` which provides language sensitive string comparison, number formatting, and date and time formatting. To read more about `Intl` [see the MDN docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Intl), the new constructors are:
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

#### Other

- Direct support for safe binary and octal literals.
- Extended support for using unicode within strings and regular expressions.
- Enhanced regular expressions.

### <a id="es7"></a> ECMAScript 2016 (ES7)

ES7 was a _much_ smaller upgrade when compared to ES6, with only 2 new features to JavaScript.

#### Array.includes

This is a useful method for arrays that simply returns a true or false if a value is present in an array

```javascript
const names = ['John', 'Ross', 'Dave'];
names.includes('Ross'); // true
names.includes('Max'); // false
```

#### Exponentiation infix operator

Examples of infix operators in JavaScript would be `+`, `-` etc, this introduces `**` which is the exponent operation, a replacement for `Math.pow()`. In other words, a number times itself n times.

```javascript
2 ** 3; // 8, same as 2 * 2 * 2
```

### <a id="es8"></a> ECMAScript 2017 (ES8)

More useful features came along in ES8, arguably the most notable is `async await`.

#### Object.values

An existing function called `Object.keys()` returns an array of the object's keys. `Object.values()` is a new function introduced in ES8 that does the same but for the values of the object.

```javascript
const countries = {
  europe: 'Italy',
  africa: 'Nigeria',
  southAmerica: 'Brazil',
};
Object.values(countries); // ["Italy", "Nigeria", "Brazil"]
```

<a id="resources"></a>This guide has been compiled from a number of resources listed below. Some bits of text and examples have been copied over from the below resources.
[FreeCodeCamp](https://www.freecodecamp.org/)
[ES6-features](http://es6-features.org/#Constants)
[Mozilla Development Network](https://developer.mozilla.org/en-US/)
