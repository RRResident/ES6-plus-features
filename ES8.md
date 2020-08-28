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

This is considered the most important update of ECMAScript 2017, and for good reason. They build on top of the JavaScript solution for asynchronous programming - Promises, and make them cleaner and easier to work with, ridding us of the need for `then()` chaining in particular.

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

In the above example, the `await` keyword 'awaits' for the promise to resolve, it then saves the resolved data as the constant `data`.
Syntactically `async await` is much easier to understand, its code is cleaner, and it is easier to debug compared to plain promises.

A more real-world example is when we want to fetch data on some user action, let's say a click of a button.

```javascript
// Add an event listener to a button
document
    .getElementById('load-msgs-btn')
    .addEventListener('click', async (e) => {
        // Fetch some data and wait for it to be resolved
        const messages = await fetch('./address/for/messages');

        const messagesContainer = document.getElementById('msgs-container');

        // Insert each of our retrieved messages into the DOM
        messages.forEach((messages) => {
            const p = document.createElement('p');
            p.textContent = message;
            messagesContainer.append(p);
        });
    });
```

## Shared memory and atomics

[Atomics](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Atomics)
[SharedArrayBuffer](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/SharedArrayBuffer)

The `Atomics` object, like `Math` cannot be instantiated, instead it has static methods and properties we can access. This is part of a plan to add a kind of multi-threading feature in JavaScript allowing developers to manage memory themselves, rather than the JS engine. This is done using the new `SharedArrayBuffer` object which esentially stores data in a shared memory space, using it makes data available to the JS engine and web worker threads.
The `Atomics` object works as a kind of 'manager' to allow for safe reading and writing of the shared memory between multiple threads, this is done via its methods. This is necessay to avoid [race conditions](https://en.wikipedia.org/wiki/Race_condition).

_Eli5: Sharing across multiple threads can end up with some issues, `Atomics` is the manager that brings order to the chaos._

Below is an example of `SharedArrayBuffer` in use between the main JS engine and a web worker.

**main.js**

```javascript
const worker = new Worker('worker.js'); // Instantiate new web worker
const sab = new SharedArrayBuffer(40); // Instantiate a new shared array buffer

// Share the new shared array buffer with the worker
worker.postMessage({ sharedBuffer });

// Wrap the array buffer in a typed array so we can edit its data
const ta = new Int32Array(sab);
```

**worker.js**

```javascript
self.addEventListener('message', (e) => {
    // Pull out our shared array buffer
    const { sharedBuffer } = e.data;

    // Convert it to a typed array
    const sharedArray = new Int32Array(sharedBuffer);
});
```

In this format we come into issues when expecting reading and writing to be done in a certain order, and the synchonizing of multple threads becomes difficult to order / manage.
For example if we add this in `main.js`

```javascript
sharedArray[1] = 5;
sharedArray[2] = 6;
```

For single threads this wouldn't be an issue as nothing is being read or written between these new lines, of course if this data was shared between multiple threads that may be reading or writing, this creates a problem.

This new global `Atomics` aims to solve this, primarilty bring 3 features:

-   safe synchronization
-   waiting (for notifications)
-   atomic operations

There are a number of methods such as `add`, `wait`, `notify` and `store` (documented in the links above) we can use on the typed arrays. A simple example below

```javascript
const sab = new SharedArrayBuffer(40);
const ta = new Uint8Array(sab);

// Using Atomics method to add the value 12 to position 0 in our typed array
Atomics.add(ta, 0, 12);
```

As of the time of writing, there [isn't great support for `Atomics` as it currently stands](https://caniuse.com/#search=atomics).
