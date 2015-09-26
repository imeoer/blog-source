title: 读书笔记 - 《Understanding ECMAScript 6》
date: 2015-08-01 12:00:00 +0800
update: 2015-09-18 10:00:00 +0800
author: me
tags:
    - JavaScript
    - 读书
preview: 《Understanding ECMAScript 6》是一本在Leanpub发售的开源电子书，较为详细的介绍了ES5的一些不足与在ES6中的改进以及新特性，计划慢慢读完这本书，在这里做一些笔记

---

《Understanding ECMAScript 6》是一本在[Leanpub](https://leanpub.com/understandinges6/read)发售的开源电子书，较为详细的介绍了ES5的一些不足与在ES6中的改进以及新特性，计划慢慢读完这本书，在这里做一些笔记。

# 基础部分

## 更好的Unicode支持

这里有一篇阮一峰关于JS中Unicode的[基础介绍](http://www.ruanyifeng.com/blog/2014/12/unicode.html)。在ES5中，字符串中每一个字符由16位编码表示，即每个字符占用2个字节，这导致一些超过该16位编码范围的字符占用4个字节来表示，因此当字符串包含这些字符时，长度与位置计算会产生偏差：

``` javascript
var text = "𠮷a"; //𠮷字超过16位编码
// ES5
text.length; // 3
text.charCodeAt(0); // 55362
text.charCodeAt(1); // 57271
text.charCodeAt(2); // 97
text.charAt(0); // ""
"\u20BB7"; // "7"， 被拆开为\u20BB与\u7，其中\u20BB不可打印
// ES6
Array.from(text).length; // 2
text.codePointAt(0); // 134071
text.codePointAt(1); // 57271
text.codePointAt(2); // 97
text.at(0); // "𠮷"
String.fromCodePoint(134071); // "𠮷"
"\u{20BB7}"; // "𠮷"
```

这个字符在Unicode中有两种表示方法，`'\u01D1'`与`'\u004F\u030C'`，分别为单一与复合表示方法，虽然表示同一字符，但在ES5中并不相等，ES6中的normalize()函数用于将该类字符正规化：

``` javascript
// ES5
'\u01D1'==='\u004F\u030C' // false
// ES6
'\u01D1'.normalize() === '\u004F\u030C'.normalize(); // true
```

## 正则表达式的改进

新增正则对象初始化方式

```javascript
// ES5
new RegExp('a', 'i'); // 只可用string初始化
// ES6
new RegExp(/a/i).flags; // 'i', 可用正则对象初始化，新增flags属性
```

新增u标识符用于更好的Unicode支持

``` javascript
var text = "𠮷";
// ES5
/^.$/.test(text); // false
// ES6
/^.$/u.test(text); // true，支持u标识符，可以正确匹配4字节的字符
```

新增的y标识符用于在每次匹配时都从开始位置匹配
``` javascript
var text = "aaa_aa_a";
// ES5
var r1 = /a+/g;
r1.exec(text); // ["aaa"]
r1.exec(text); // ["aa"]
// ES6
var r2 = /a+/y;
r2.exec(text); // ["aaa"]
r2.exec(text); // null
r2.sticky; // true
```

## 新增的字符串函数

``` javascript
var msg = "Hello";
msg.startsWith("He"); // true
msg.endsWith("o"); // true
msg.includes("ll"); // true
msg.repeat(3); // "HelloHelloHello"
```

## Object.is

更加严格的值比较方法：

``` javascript
+0 === -0; // true
NaN === NaN; // false

Object.is(+0, -0); // false
Object.is(NaN, NaN); // true
```

## Let声明

JS中没有块作用域，且由于变量提升(hoisted)问题，在块中声明的变量会被提升到函数顶部，即在该函数内块中声明变量可以在整个函数中被访问到：

``` javascript
function getValue(condition) {
    if (condition) {
        var value = "blue";
        return value;
    } else {
        return null;
    }
}
```

实际会被解释为：

``` javascript
function getValue(condition) {
    var value;
    if (condition) {
        value = "blue";
        return value;
    } else {
        return null;
    }
}
```

JS新增let声明方式，限制变量只能在块级范围内使用：

``` javascript
function getValue(condition) {
    if (condition) {
        let value = "blue"; // 只在该if条件下能被访问
        return value;
    } else {
        return null;
    }
}
```

由于let的特性，之前为了解决循环时因共享变量问题使用的闭包方式：

``` javascript
var funcs = [];
for (var i=0; i < 10; i++) {
    funcs.push((function(value) {
        return function() {
            console.log(value);
        }
    }(i)));
}
funcs.forEach(function(func) {
    func();
});
```

可以改为：

``` javascript
var funcs = [];
for (let i=0; i < 10; i++) {
    funcs.push(function() { console.log(i); });
}
funcs.forEach(function(func) {
    func();
})
```

> 好吧，表示细节好多，先读一遍阮一峰的《ECMAScript 6入门》 _(:з」∠)_
