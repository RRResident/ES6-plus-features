This is work in progress! There are still many details and sections missing, you may see a few TODO:'s dotted around as well.

# ECMAScript 2015 (ES6) +

## Intro

JavaScript has changed a lot since the days of jQuery. Early iterations of JavaScript could be described as lacking in functionality, and at the time the functionality it did have, had mixed support in the popular browsers. jQuery was a library that added a lot of functionality on top of JavaScript, made it more readable (some might say) and allowed for more consistancy among how it was implemented in browsers.

Esentially, when ECMAScript 2015 (also known as ES6) was released, it included a **LOT** of new functionality that made an additional library like jQuery unnecessary. ES6 and onwards is well supported in modern browsers, however many developers still feel it is best practice to transpile code down to ES5 with a tool like Babel. Luckily this is simple to implement into modern build tools / processes.

This is a guide I have wrote for myself and to anyone who wants to see what has changed in JavaScript since ES5, and what these modern features are. Most of the features will have links to the Mozilla Development Network page shortened to MDN.

Read on or jump to a section:

1. [ECMAScript 2015 (ES6)](#es6)
1. [ECMAScript 2016 (ES7)](#es7)
1. [ECMAScript 2017 (ES8)](#es8)
1. [ECMAScript 2018 (ES9)](#es9)
1. [ECMAScript 2019 (ES10)](#es10)
1. [ECMAScript 2020 (ES11)](#es11)
1. [Resources](#resources)

### << <a id="es6"></a>ECMAScript 2015 (ES6) >>

There were a **LOT** of changes/features introduced with ES6, to focus on the more interesting features, I've marked them with `!!`, so you can search for that if you want to jump to the bigger features.

#### ----- Constants !!

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

#### ----- Blocked scope variables with let !!

[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let)

As well as `const`, ES6 also introduced another keyword for defining variables: `let`.
Variables defined with `var` are scoped to the immediate function body, whilst `let` variables are scoped to the immidiate code block `{}`. There a few more differences of note, [a great Stackoverflow answer breaks this down in depth](https://stackoverflow.com/questions/762011/whats-the-difference-between-using-let-and-var). In essence, the function scope of `var` was a common cause of bugs in JavaScript and `let` was aimed at changing that.

#### ----- Arrow functions !!

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

#### ----- Default parameter values

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

#### ----- Rest parameter !!

[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/rest_parameters)

'Rest' refers to remaining arguments passed to a function. In ES5 you'd end up with messy unreadable Array prototype functions to make this happen, but ES6:

```javascript
function f(a, b, ...restOfArgs) {
  // ...
}
```

#### ----- Spread operator !!

[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax)

Spread an array's items into a function's arguments or another array

```javascript
const someArr = [1, 2, 3];
const someOtherArr = [...someArr, 4, 5, 6]; // [1, 2, 3, 4, 5, 6]
```

#### ----- Template literals !!

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

#### ----- Enhanced object properties

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

#### ----- Destructuring

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

#### ----- Modules !!

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

#### ----- Classes !!

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

#### ----- Symbols

[MDN](https://developer.mozilla.org/en-US/docs/Glossary/Symbol)

ES6 introduces another primitive type called symbols. They are essentially tokens that serve as unique IDs. You can create them like so

```javascript
let sym1 = Symbol('sym');
let sym2 = Symbol('sym');
sym1 === sym2; // false
```

Every symbol, whether passed (the optional) value or not, is unique, comparing two of them will never return `true`.

#### ----- Iterators and generators

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

#### ----- Set (data structure)

[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set)

JavaScript now has sets, these are lists of values similar to arrays but can only contain unique values. They have their own methods with names that differ from arrays for some reason.

```javascript
let s = new Set();
s.add(1);
s.add(2);
s.add(1);
s.size; // 2
```

#### ----- Map (data structure)

[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map)

A map object, like normal objects, holds key-value pairs and remembers the order of insertion, it takes any value as keys or values, making them different from normal objects.

```javascript
const mapObj = new Map();
mapObj.set(Symbol(), 1);
map.Obj.set({ keyName: 'key' }, 'value');
```

#### ----- Weakset and Weakmap (data structure)

[MDN weakset](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/WeakSet)
[MDN weakmap](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/WeakMap)

These are similar, but slightly different to `Map` and `Set`.
`WeakSet`s are collections of **objects only**, and cannot take other types like normal `Set`s can. They are also 'weak' links, for example if you have an object created in your global context, and set that as an item in the `WeakSet` with `weakset.add(globalObj)`, and _no other_ code referenced that `globalObject`, then it could be garbage collected. This can be helpful when considering memory allocation.

`WeakMap` are similar with how their 'weak' link works without outside objects, and they also can only have objects as their keys.
[A good article explaining the difference betwee Map and WeakMap](https://www.mattzeunert.com/2017/01/31/weak-maps.html).

#### ----- Typed arrays

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

#### ----- New built in methods

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

8. `Math.trunc()` is a method that drops the fractional part of a number, like a round down to a whole number.

```javascript
Math.trunc(11.6); // 11
```

9. `Math.sign()` method either returns a positive or a negative, in other words, if passed a positive number it will return `1`, or `-1` for a negative number. It will return `0` if passed a `0`, same with `-0`. Anything else will return `NaN`.

#### ----- Promises !!

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

#### ----- Proxying

[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy)

The `Proxy` object is used to define custom behavior for fundamental operations in JavaScript (e.g. property lookup, assignment, enumeration, function invocation, etc). Let's say we wish to make a person object, with properties that can only have certain value types.
When creating a proxy instance, you need to pass it two things:

- the target object to wrap with `Proxy`
- The handler object which has functions that define the proxy objects behavior

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

#### ----- Reflect object

[MDN comparing object and Reflect methods](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Reflect/Comparing_Reflect_and_Object_methods)
[MDN Reflect](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Reflect)

The `Reflect` object provides methods to interact with objects programatically. Like `Math` is isn't something you create an instance of, instead it has static methods we can call.

Below is an example of one of it's methods; `Reflect.ownKeys()` it returns an array of the target object's own property keys. This doesn't return any inherited keys like `Object.keys()`.

```javascript
const obj = { a: 1, b: 2 };
Reflect.ownKeys(obj); // [a, b];
```

There are many methods for the `Reflect` object, read the documentation for a full list.

#### ----- International and localization support

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

#### ----- Other

- Direct support for safe binary and octal literals.
- Extended support for using unicode within strings and regular expressions.
- Enhanced regular expressions.

### << <a id="es7"></a> ECMAScript 2016 (ES7) >>

ES7 was a _much_ smaller upgrade when compared to ES6, with only 2 new features to JavaScript.

#### ----- Array.includes

[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/includes)

This is a useful method for arrays that simply returns a true or false if a value is present in an array

```javascript
const names = ['John', 'Ross', 'Dave'];
names.includes('Ross'); // true
names.includes('Max'); // false
```

#### ----- Exponentiation infix operator

[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Arithmetic_Operators#Exponentiation)

Examples of infix operators in JavaScript would be `+`, `-` etc, this introduces `**` which is the exponent operation, a replacement for `Math.pow()`. In other words, a number times itself `n` times.

```javascript
2 ** 3; // 8, same as 2 * 2 * 2
```

### << <a id="es8"></a> ECMAScript 2017 (ES8) >>

More useful features came along in ES8, arguably the most notable is `async await`, but there was also some useful methods added to the `Object` object.

#### ----- Object.values

[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/values)

An existing function called `Object.keys()` returns an array of the object's keys. `Object.values()` is a new function introduced in ES8 that does the same but for the values of the object.

```javascript
const countries = {
  europe: 'Italy',
  africa: 'Nigeria',
  southAmerica: 'Brazil',
};
Object.values(countries); // ["Italy", "Nigeria", "Brazil"]
```

#### ----- Object.entries

[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/entries)

`Object.entries()` is a function that can get you the keys _and_ values out of an object (unlike the above mentioned funtions which get one or the other). It will return an array of arrays, each of which will be each of your object's key and value pairs.

```javascript
const someObj = { a: 1, b: 2, c: 3 };
Object.entries(someObj); // [[a, '1'], ['b', 2], ['c', 3]]
```

This makes it useful for looping over both using something like the below:

```javascript
for (let [key, value] of Object.entries(someObj)) {
  // ...
}
```

#### ----- String padding

[MDN padStart](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/padStart)
[MDN padEnd](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/padEnd)

Two new functions for strings allow for padding
`padStart()`
and
`padEnd()`
Both of these functions take 2 arguments, the first being the length the string should reach (target length), the second argument is an optional string to pad it with, `" "` by default.

```javascript
let codes = ['3', '45', '722'];

codes.map((code) => {
  code = code.padStart(5, '0'); // pad it to length of 5 with 0's
});
codes;
/*
 * 00003
 * 00045
 * 00722
 */
```

#### ----- Object.getOwnPropertyDescriptors

[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertyDescriptors)

`Object.getOwnPropertyDescriptors()` can be passed an object, the result will be an object that has all own property descriptors of the passed object. Down below is a basic example of how you can access some details of properties of the object:

```javascript
const person = {
  name: 'John',
  age: 44,
};

const personDescriptors = Object.getOwnPropertyDescriptors(person);
personDescriptors.name.writable; // true
personDescriptors.age.value; // 44
```

TODO:

#### ----- Trailing commas in function parameters

[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Trailing_commas)

This is simply a change to function parameter syntax, you are now allowed to have a comma after the last parameter

```javascript
function trailingCommas(
  arg1,
  arg2,
  arg3 // A comma here won't error
) {
  // ...
}
```

#### ----- Async await !!

[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function)

This is considered the most important update of ECMAScript 2017, and for good reason. They build on top of the JavaScript solution for asynchronous programming - Promises, and make them cleaner and easier to work with.

`async` and `await` are two new keywords we can use to control the flow of our asynchronous code.
We need to keep our code in a function, we can use the `async` keyword in front of the function declaration to create an `async` function. These functions always return a promise.

```javascript
const makeRequest = async () => {
  const data = await getData(); // getdata() returns a promise

  doSomethingWithData(data);
};

// makeRequest returns a promise, which can be accessed with .then()
makeRequest().then((result) => console.log(result));
```

In the above example, the `await` keyword 'awaits' for the promise to resolve, it then saves the resolved data into the constant on the left.
Syntactically `async await` is much easier to understand, its code is cleaner, and it is easier to debug.

#### ----- Shared memory and atomics

// TODO
To read about this check out
[Atomics on MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Atomics)
[SharedArrayBuffer on MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/SharedArrayBuffer)

### << <a id="es9"></a> ECMAScript 2018 (ES9) >>

ES9 brought in some nice additions to asynchronous programming in JavaScript.

#### ----- Regex updates

[MDN regular expressions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions)

There are 4 changes to regex in JS, [explained in detail in this article by Faraz Kelhini](https://www.smashingmagazine.com/2019/02/regexp-features-regular-expressions/#lookbehind-assertions), brief descriptions below:

1. `s` flag
   To add to the regex flags like `g` (matches multple times), or `i` (makes regex case insensitive), we now have `s` which causes the `.` to also match new line characters.
2. Named capture groups. Say you have a regex like `/www\.(\w*)\.(\w*)/` you wanted to grab the subdomain and top level domain from, once you create a match object, you would have to pull the groups out by numbers with `match[1]` etc, which makes things difficult when there are many groups. Now JavaScript has named capture groups that make this easier using `(?<name>)`, so we could do the above using `/www\.(?<subdomain>\w*)\.(?<topleveldomain>\w*)/` and pulling out the groups using `match.groups.subdomain`.
3. Look behind assertions. Prior to ES9, only look ahead assertions were available in JavaScript, now you can use `(?<=...)` to only match expressions that follow a pattern.
4. Unicode property escapes
   You can now search for special characters by mentioning their unicode character property inside of a `/p{}`. Unicode characters have a lot of properties, for example `Letter` matches any unicode that represents a letter in any language, so we could write `/p{Letter}` to match these.

#### ----- Object rest and spread properties

Rest and spread are already available inside arrays, now that functionality extends to objects. An example of how we can use the rest property when destructuring objects:

```javascript
let { a, ...rest } = { a: '1', b: '2', c: '3' };
a; // 1
rest; // {b: "2", c: "3"}
```

And the spread property to copy one object's properties into another

```javascript
let originalObj = { a: '1', b: '2', c: '3' };
let newObj = { ...originalObj, d: '4' };
newObj; // { a: '1', b: '2', c: '3', d: '4' }
```

#### ----- Promise finally

[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/finally)

In addition to `.catch()` and `.then()`, to avoid duplicate code inside these handlers, we now have access to `.finally()` which returns a promise and takes a callback function.

```javascript
fetch('http://somesite.com')
  .then((data) => console.log(data))
  .catch((error) => console.log(error))
  .finally(() => console.log('Done!'));
```

#### ----- Asynchronous iteration

[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for-await...of)
Using the new `for (await ... of ...)` format, we can now loop over an iterable and return promises. Below is an example of an iterable that returns promises, and an asychronous function that contains a `for (await ... of ...)` that loops through them.

```javascript
const users = ['Ross', 'Miyamoto', 'Pauline'];
const asyncIterable = {
  [Symbol.asyncIterator]() {
    let i = -1;
    return {
      next() {
        i++;
        if (i < users.length) {
          return Promise.resolve({ value: users[i], done: false });
        }

        return Promise.resolve({ done: true });
      },
    };
  },
};

(async function () {
  for await (let user of asyncIterable) {
    console.log(user);
  }
})();
```

### << <a id="es10"></a> ECMAScript 2019 (ES10) >>

This is currently the latest interation of JavaScript released in June 2019 and includes some nice additions to the language.

#### ----- Saving dynamic imports to variables

[MDN import statement](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import)

Imports are not new, but being able to assign dynamic (`import()`) imports to variables is. Say we want to load in a module when a button is clicked, and use a function from that module, we could do the following:

```javascript
button.addEventListener('click', async () => {
  const module = await import('./path/to/module.js'); // await import to resolve
  module.buttonClick(); // then call function from the import
});
```

#### ----- Array.prototype.flat()

[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/flat)

You can now easily flatted nested arrays, the `flat` function takes a depth. You can also pass it `infinity` to completely flatten an array.

```javascript
let nestedArray = [1, [1, 2, 3, [1, [1, 2, [3, [4]]]], [1]]];
nestedArray.flat(infinity); // [1, 1, 2, 3 1, 1, 2, 1, 4, 1]
```

#### ----- Array.prototype.flatMap()

[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/flatMap)

`flatMap()` essentially runs a map function on each of the array's items, then flattens it.

```javascript
const nums = [1, 2, 3, 4, 5];

nums.map((x) => [x * 2]); // [[2], [4], [6], [8], [10]]
nums.flatMap((x) => [x * 2]); // [2, 4, 6, 8, 10]
```

Notice how `.flatMap()` flattens the array it returns.

#### ----- String.trimStart() & String.trimEnd()

[MDN trimStart](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/trimStart)
[MDN trimEnd](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/trimEnd)

JavaScript already has a `trim()` function for strings, which takes off any white space from the start and end of the string. These two new functions for strings are pretty self-explanatory, they take off whitespace from the start or end of the string.

```javascript
const hello = '    hello    ';
hello.trimStart(); // "hello    "
hello.trimEnd(); // "    hello"
```

#### ----- Object.fromEntries()

[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/fromEntries)

`Object.fromEntries()` creates an object from key-value pairs, it takes an iterable like an `Array` or `Map` and retuns an object. Below we have a `Map` with some data, which we use to create a new object.

```javascript
const rossMap = new Map([
  ['name', 'Ross'],
  ['likes', 'video games'],
]);

const rossObj = Object.fromEntries(rossMap); // {name: "Ross", likes: "video games"}
```

#### ----- Stable Array.prototype.sort()

Esentially, for arrays with more than 10 items, this method used an unstable algorithm. ES10 now offers a stable `array.sort()`.

#### ----- Optional catch binding

This refers to changes with `try ... catch` blocks in JavaScript. See below we have some simple code where we `try` and do something, if there is an error we `return false`

```javascript
try {
  // ... do something
} catch (error) {
  return false;
}
```

Notice how we _have_ to have the `error` variable, even if we don't actually use it. ES10 now makes this optional, so the below code would not error

```javascript
try {
  // ... do something
} catch {
  return false;
}
```

#### ----- Symbol.prototype.description

[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol/description)

ES10 Adds the `description` read-only property to `Symbol` objects. `Symbol`s can be created with an optional description, which is a string you pass to it during instantiation.

```javascript
const sym = Symbol('desc');
sym.description; // 'desc'
```

#### ----- Other

- Well-formed `JSON.stringify()`
- `function.toString()` now more standardized

### << <a id="es11"></a> ECMAScript 2020 (ES11) >>

These features are due to be approved in June. [ES2020 candidate](https://github.com/tc39/ecma262/releases/tag/es2020).

#### ----- String.prototype.matchAll()

[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/matchAll)

The `matchAll()` method returns an iterator of all the results matched against a string. It was possible to iterate through multiple matches prior to this, but this has been made much easier.

```javascript
const regex = /l/g;
const str = 'Hello, World!';

const result = [...str.matchAll(regex)];
for (let i of result) {
  console.log(i); // ["l"], ["l"], ["l"]
}
```

#### ----- Bigint

[MDN](https://developer.mozilla.org/en-US/docs/Glossary/BigInt)

Before ES10, the biggest number supported in JavaScript was 9007199254740991. Now with support for big ints, 2⁵³ numbers are supported, these are bigints and are a new primitive. Appending 'n' will create a bigint.

```javascript
const a = 1n;
typeof a; // "bigint"
```

### <a id="resources"></a> << Resources >>

This guide has been compiled from a number of resources listed below

- [Mozilla Development Network](https://developer.mozilla.org/en-US/)
- [FreeCodeCamp](https://www.freecodecamp.org/)
- [ES6-features](http://es6-features.org/#Constants)
- [Hacker Noon](https://hackernoon.com/)
