---
layout: post
title: "ES6 Features Every JavaScript Developer Must Know"
date: 2017-07-31 16:10:13
categories: Web Development
meta: "ES6 Features Every JavaScript Developer Must Know"
---

Trong bài viết này, tôi sẽ cover những tính năng mới của ES6 mà tôi nghĩ bất cứ JavaScript Developer nào cũng cần phải nắm vững. Bài viết này đặc biết hữu ích cho những ai mới bắt đầu tìm hiểu ES6 or học 1 front-end framworks nào đó.
## 1. Let and Const
Let khá là giống với var nó chỉ khác ở chỗ let có block scope. Những biến khai báo bằng let chỉ access được trong block scope mà nó được định nghĩa. Hãy nhìn 2 ví dụ dưới đây để thấy sự khác biệt giữa var và let.

```
// Su dung var
if (true) {
 var name = "ES6"';
}
console.log(c); // Output:  'ES6'

// Su dung let
if (true) {
  let name = 'ES6';
}
console.log(name); // Output: name is not defined.
```
## 2. Const
const được dùng để khai báo hằng số. Giá trị của nó sẽ không được thay đổi một khi đã khai báo.
```javascript
const a = 50;
a = 60; // Output: You cannot change the value of const.
```
## 3. Arrow Function
Để dễ hình dung, tôi sẽ đưa ra cú pháp của arrow function.
```javascript
(a, b) => a + b; // return a + b;

(a, b) => {
    const sum = a + b;
    return sum;
} // xử lý code trước khi return

(user) => ({
  name: user.name,
  isSingle: user.isSingle,
}) // return object. Lưu ý dấu ()

```
## 4. Property Shorthand
ECMAScript 6.
`obj = { x, y }`
ECMAScript 5.
`obj = { x: x, y: y };`
## 5. Method Properties
ECMAScript 6.
```javascript
const obj = {
    foo (a, b) {
        …
    },
    bar (x, y) {
        …
    }
}
```
ECMAScript 5.
```javascript
const obj = {
    foo: function (a, b) {
        …
    },
    bar: function (x, y) {
        …
    }
};
```
## 6. Default Parameter Values
ECMAScript 6.
```
function f (x, y = 7, z = 42) {
    return x + y + z
}
f(1) // Output: 50
```

ECMAScript 5.
```
function f (x, y, z) {
    if (y === undefined)
        y = 7;
    if (z === undefined)
        z = 42;
    return x + y + z;
};
f(1) // Output: 50
```
## 7. Rest Parameter
ECMAScript 6.
```
function f (x, y, ...a) {
    return (x + y) * a.length
}
f(1, 2, "hello", true, 7) === 9
```

ECMAScript 5.
```
function f (x, y) {
    var a = Array.prototype.slice.call(arguments, 2);
    return (x + y) * a.length;
};
f(1, 2, "hello", true, 7) // Output: 9;
```
## 8. Spread Operator
ECMAScript 6.
```
var params = [ "hello", true, 7 ]
var other = [ 1, 2, ...params ] // [ 1, 2, "hello", true, 7 ]

function f (x, y, ...a) {
    return (x + y) * a.length
}
f(1, 2, ...params) // Output: 9

var str = "foo"
var chars = [ ...str ] // [ "f", "o", "o" ]
```
ECMAScript 5.
```
var params = [ "hello", true, 7 ];
var other = [ 1, 2 ].concat(params); // [ 1, 2, "hello", true, 7 ]

function f (x, y) {
    var a = Array.prototype.slice.call(arguments, 2);
    return (x + y) * a.length;
};
f.apply(undefined, [ 1, 2 ].concat(params)) // Output: 9;

var str = "foo";
var chars = str.split(""); // [ "f", "o", "o" ]
```
## 9. Set Data-Structure
ECMAScript 6.
```javascript
let s = new Set()
s.add("hello").add("goodbye").add("hello")
s.size === 2
s.has("hello") === true
for (let key of s.values()) // insertion order
 console.log(key)
```
ECMAScript 5.
```
var s = {};
s["hello"] = true; s["goodbye"] = true; s["hello"] = true;
Object.keys(s).length === 2;
s["hello"] === true;
for (var key in s) // arbitrary order
    if (s.hasOwnProperty(key))
        console.log(s[key]);
```

## 10. Map Data-Structure
ECMAScript 6.
```javascript
let m = new Map()
let s = Symbol()
m.set("hello", 42)
m.set(s, 34)
m.get(s) === 34
m.size === 2
for (let [ key, val ] of m.entries())
    console.log(key + " = " + val)
```
## 11. Number Formatting
ECMAScript 6.
```javascript
var l10nEN = new Intl.NumberFormat("en-US")
var l10nDE = new Intl.NumberFormat("de-DE")
l10nEN.format(1234567.89) === "1,234,567.89"
l10nDE.format(1234567.89) === "1.234.567,89"
```
