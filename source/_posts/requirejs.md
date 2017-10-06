---
title: requirejs
layout: post
category: [dev, js]
date: 2016-04-12
tags: [r.js, requirejs]
description: requirejs 
---

> build.js

``` javascript
({
    baseUrl: ".",
    paths: {
        jquery: "1",
        jhary: "2"
    },
    include: ['1','2'],
    out: "main-built.js"
})
```

``` bash
r.js -o build.js optimize=none
```

> main-built.js

``` javascript
define('1',[], function() {
    return 1;
})
;
define('2',[], function() {
    return 2;
})
;
```
