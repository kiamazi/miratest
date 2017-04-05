---
utid: 20151216203648
date: 2015-12-16 20:36:48
title: Promisified Arrays
_index: promise-arrays
tags:
  - Javascript
  - Promise
---
I like Javascript [Promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) a lot, so I’m trying to fit them everywhere in my apps. If you haven’t heard about Promises, or haven’t used them yet, I suggest you check out [this great guide](https://davidwalsh.name/promises) on them.

I also like ES5 array functions and in my opinion they’re a few of the best additions to the language. My favorites are `filter` and `map`. The problem with these functions is they are synchronous, and I find myself trying to do something like this:


    var result = arr.map(function(item) {
        // Do something Async here.
    });


Which clearly isn’t going to work out since the callback function passed to `map` needs to have a return value. A possible solution to this is turn each array item into a `Promise` object and wait for all promises to resolve using `Promise.all`. Something like:


    var arr = [1, 2, 3];
    var promisified = arr.map(function(item) {
        return new Promise(function(resolve) {
            resolve(item * 10);
        });
    });

    Promise.all(promisified).then(function(result) {
        console.log(result); // [10, 20, 30];
    });


This solution is alright, until you need to do it repeatedly then it will get verbose pretty fast. So [I made a small helper library](https://github.com/sallar/promise-arrays) to do it.

### promise-arrays
This super small library provides async versions of `map` and `filter` which return a `Promise` with the result. Take this example:


    var PromiseArrays = require('promise-arrays');
    var arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

    PromiseArrays.filter(arr, function(item) {
        return new Promise(function(resolve) {
            window.setTimeout(function() {
                resolve(item > 3 && item < 8);
            }, 100);
        });
    }).then(function(result) {
        console.log(result); // [4, 5, 6, 7] after about ~100ms.
    });


It’s possible to return a new `Promise` or a normal value from the callback.

`promise-arrays` is available to use with Node.JS, UMD and browser globals. [Check it out on Github](https://github.com/sallar/promise-arrays) and please report any issues. All suggestions are also welcome :-)
