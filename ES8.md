# ECMAScript 2017 (ES8)

## Object.values

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

## Object.entries

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

## String padding

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

/*
codes:
 * 00003
 * 00045
 * 00722
 */
```

## Object.getOwnPropertyDescriptors

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

Some other descriptors include `configurable`, `enumerable` etc

## Trailing commas in function parameters

[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Trailing_commas)

This is simply a change to function parameter syntax, you are now allowed to have a comma after the last parameter

```javascript
function trailingCommas(
    arg1,
    arg2,
    arg3 // A comma here won't error
) {}
```

## Async await !!

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

In the above example, the `await` keyword 'awaits' for the promise to resolve, it then saves the resolved data as a constant.
Syntactically `async await` is much easier to understand, its code is cleaner, and it is easier to debug compared to plain promises.

## Shared memory and atomics

The `Atomics` object, like `Math` cannot be instantiated, instead it has static methods and properties we can access. This is part of a a plan to allow a kind of multi-threading feature in JavaScript allowing developers to manage memory themselves, rather than the JS engine. This is done using the new `SharedArrayBuffer` object which esentially stores data in a shared memory space, using it makes data available to the JS engine and web worker threads.

// TODO:
To read about this check out
[Atomics on MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Atomics)
[SharedArrayBuffer on MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/SharedArrayBuffer)
