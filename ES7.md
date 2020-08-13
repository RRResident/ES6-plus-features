# ECMAScript 2016 (ES7)

## Array.includes

[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/includes)

This is a useful method for arrays that simply returns a true or false if a value is present in an array

```javascript
const names = ['John', 'Ross', 'Dave'];
names.includes('Ross'); // true
names.includes('Max'); // false
```

## Exponentiation infix operator

[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Arithmetic_Operators#Exponentiation)

Examples of infix operators in JavaScript would be `+`, `-` etc, this introduces `**` which is the exponent operation, a replacement for `Math.pow()`. In other words, a number times itself `n` times.

```javascript
2 ** 3; // 8, same as 2 * 2 * 2
```
