---
title: Promise || Async/Await
layout: post
category: [dev, js]
date: 2016-07-11
tags: [es6, es7, es2015, async await, async/await, promise]
description: callback, promise, async/await 코드를 통해 비교한다.
---

callback방식

```javascript
function asyncFunction(callback) {
    setTimeout(function() {
        if (callback) callback(1);
    }, 1000);
}

function caller() {
    asyncFunction(function(returnVal) {
        console.log('after callback()');
    });
}
```

promise

```javascript
function asyncFunction() {
    return new Promise(function(resolve, reject) {
        setTimeout(function() {
            resolve(1);
        }, 1000);
    });
}
function caller() {
    asyncFunction()
        .then(function resolved(resolveVal) {
            console.log('after callback()');
        })
        .catch(function rejected() { });
}
```

Async/Await / ES7

```javascript
async function caller() {
    try {
        const resolveVal = await asyncFunction();
        console.log('after callback()');
    } catch (rejected) {
        console.log('after reject(throw)');
    }
};
```

Async/Await은 Await을 사용하기 위해 함수를 async로 선언해야 한다. TypeScript는 1.7버전 이상이 있으면 사용 가능하다.

마지막으로 async 함수는 Promise를 리턴한다.

```javascript
async function returnPromise(wantToThrow) {
    if (wantToThrow) {
        throw false;
    }
    return true;
}

async function awaiter() {
    const mustBeTrue = await returnPromise();
    console.log(mustBeTrue) // true

    try {
        await returnPromise(true);
    } catch(ex) {
        console.log(ex); // false
    }
}
```
