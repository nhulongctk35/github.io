---
layout: post
title: "ES6 Features Every JavaScript Developer Must Know Part 2"
date: 2017-10-24 16:10:13
categories: Web Development
meta: "ES6 Features Every JavaScript Developer Must Know Part 2"
---


![](https://viblo.asia/uploads/58609de6-ffe3-4348-899b-6bfcda6bd4af.jpeg)

Trong bài viết này, tôi sẽ cover thêm những tính năng mới của ES6, phần đầu tiên đã được post tại [ES6 Features Every JavaScript Developer Must Know](https://viblo.asia/p/es6-features-every-javascript-developer-must-know-GrLZDbE35k0)

## 1. Object Property Assignment
Combine 2 hoặc nhiều nhiều Objects.
### 1.1 Trường hợp Object property khác nhau
```
var destination = { a: 0 };
var src1 = { b: 1, c: 2 };
var src2 = { d: 3, e: 4 };

// ES5
Object.keys(src1).forEach(function(k) {
  destination[k] = src1[k];
})
Object.keys(src2).forEach(function(k) {
  destination[k] = src2[k];
});

// ES6
Object.assign(destination, src1, src2);

// RESULT
destination === { a: 0, b: 1, c: 2, d: 3, e: 4 };
```
### 1.2 Trường hợp Object property trùng nhau
```
var o1 = { a: 1, b: 1 };
var o2 = { b: 2 };

var destination = Object.assign({}, o1, 02);
// RESULT
destination === { a: 1, b:2 }
```
## 2. Clone hay Copy một Object
```
// ES6
var obj = { a: 0 };
var copy = Object.assign({}, obj);
copy === { a: 0 }
```
## 3. Tìm một phần tử trong Array
ES6 bổ sung thêm 2 methods mới đó là `find()` và `findIndex()`. Lưu ý là method find sẽ trả về phần từ đầu tiên tìm thấy trong Array.
```
var array = [ 'a', 'b', 'c', 'd', 'e', 'e', 'e' ];

// ES5
array.filter(function (x) { return x === 'e' })[0]
// RESULT: 'e'

// ES6
array.filter(x => x === 'e');
// RESULT: 'e'

array.findIndex(x => x === 'e')
// RESULT: 4
```
## 4. String Repeating
```
// ES5
var repeat = Array(3 + 1).join('foo ');
console.log(repeat); // foo foo foo

// ES6
var repeat = 'foo '.repeat(3);
console.log(repeat); // foo foo foo
```
## 5. Array Destructuring 
```
// ES6
let a = 'world', b = 'hello';
[a, b] = [b, a];
console.log(a); // hello
console.log(b); // world
```
## 6.  Async/Await with Destructuring
```
// ES6
const [user, account] = await Promise.all([
  fetch('/user'),
  fetch('/account')
])
```
