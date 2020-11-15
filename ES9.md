# ECMAScript 2018 (ES9)

## Regex updates

[MDN regular expressions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions)

There are 4 changes to regex in JS, [explained in detail in this article by Faraz Kelhini](https://www.smashingmagazine.com/2019/02/regexp-features-regular-expressions/#lookbehind-assertions), brief descriptions below:

#### /s flag

To add to the regex flags like `g` (matches multple times), or `i` (makes regex case insensitive), we now have `s` which causes the `.` to also match new line characters.

#### Named capture groups

Say you have a regex like `/www\.(\w*)\.(\w*)/` you wanted to grab the subdomain and top level domain from, once you create a match object, you would have to pull the groups out by numbers with `match[1]` etc, which makes things difficult when there are many groups. Now JavaScript has named capture groups that make this easier using `(?<name>)`, so we could do the above using `/www\.(?<subdomain>\w*)\.(?<topleveldomain>\w*)/` and pull out the groups using `match.groups.subdomain`.

#### Look behind assertions

Prior to ES9, only look ahead assertions were available in JavaScript, now you can use `(?<=...)` to only match expressions that _come after_ a pattern.

#### Unicode property escapes

You can now search for special characters by mentioning their unicode character property inside of a `/p{}`. Unicode characters have a lot of properties, for example `Letter` matches any unicode that represents a letter in any language, so we could write `/p{Letter}` to match these.

## Object rest and spread properties

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

You may have seen this used in frontend frameworks that copy or update data/state objects that do something along the lines of this;

```javascript
let newState = { ...oldState, new: 'thing' };
```

## Promise finally

[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/finally)

In addition to `.catch()` and `.then()`, to avoid duplicate code inside these handlers, we now have access to `.finally()` which returns a promise and takes a callback function that executes when the promise is fulfilled _or_ rejected.

```javascript
fetch('http://somesite.com')
    .then((data) => console.log(data))
    .catch((error) => console.log(error))
    .finally(() => console.log('Either resolved OR rejected!'));
```

## Asynchronous iteration

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
// Ross
// Miyamoto
// Pauline
```
