---
title: Promise || Async/Await
date: 2016-07-11 22:57:19
tags:
---

callback방식

```js
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

```js
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

```js
async function caller() {
    try {
        const resolveVal = await asyncFunction();
        console.log('after callback()');
    } catch (rejected) {
        console.log('after reject(throw)');
    }
};
```

Async/Await은 Await을 사용하기 위해 함수를 async로 선언해야 한다. ES7의 실험적 문법을 사용하고 있으므로 컴파일러가 필요하다.

Babel에서는 `stage-0` 플러긴을 필요로 하고 TypeScript는 1.7버전 이상이 있으면 사용 가능하다.
