---
title: ES6 on io.js v1.3.0
date: 2015-02-27 02:17:14
template: article.jade
tags: javascript, es6, iojs
---

io.js has released version 1.3.0 shipping with V8 4.1.0.14 while Node.js v0.12.x ships with V8 3.28.73. That means latest io.js includes ES6 features well beyond Node.js. In io.js 1.3.0, there is no more `--harmony` flag, all stable ES6 features are integrated by default. Running io.js with `--es_staging` for staged features. For other in-progress features to land on future io.js releases is just a matter of time. I definitely will look positively into io.js, this post is a summary on stable ES6 features on io.js 1.3.0 with real snippets.

<span class="more"></span>

- Block scoping
```javascript
'use strict';

// let
for (let i=0; i<5; i++) {}
console.log(i) // ReferenceError: i is not defined

// const
const foo = 'foo';
foo = 'bar'; // TypeError: Assignment to constant variable
const foo = 'bar'; // SyntaxError: Identifier 'foo' has already been declared
```

---

- collection
```javascript
// Map
var map = new Map();
map.set({}, 'key object');
map.set('str', 'key string');
map.set(function () {}, 'key function');

console.log(map.size); //3
for (var k of map.keys()) { // {}, str, [function]
    console.log(k);
}
for (var v of map.values()) { // key object, key string, key function
    console.log(v);
}

// Set
var set = new Set([1, 2]);
set.add({});
for (let kv of set.entries()) { // [1, 1], [2, 3], [{}, {}]
    console.log(kv);
}
```

---

- Generator
```javascript
function* gen(i) {
    yield i;
    if (i > 3) {
        yield* gen(i - 1);
    }
}

var g = gen(5);
console.log(g.next().value); // 5
console.log(g.next().value); // 4
console.log(g.next().value); // 3
console.log(g.next().done); // true
```

---

- Binary and Octal literals
```javascript
0b10 // binary 10 -> decimal 2
0B10 // binary 10 -> decimal 2
0o10 // octal 10 -> decimal 8
0O10 // octal 10 -> decimal 8
```

---

- Promise
```javascript
var p = new Promise(function (resolve, reject) {
    setTimeout(function () {
        resolve('foo');
    }, 50);
});

p.then(function (val) {
    console.log(val); // foo
});
``` 

---

- New String method
```javascript
console.log(String.fromCodePoint(20013, 22269)); //中国
console.log('中国'.codePointAt(0)); // 20013
console.log('mo'.repeat(3)); // momomo
console.log('mopododa'.includes('da')); // true
console.log('mopododa'.startsWith('mo')); // true
console.log('mopododa'.endsWith('da')); // true
```

---

- Symbol
```javascript
var foo = Symbol(), bar = Symbol();
var o = {};
o[foo] = 'foo';
o[bar] = 'bar';
```

---

- Template strings
```javascript
var foo = 'fool';
console.log(`bar loves ${foo}`); // bar loves fool
```

---
[iojs.org](https://iojs.org/en/es6.html)