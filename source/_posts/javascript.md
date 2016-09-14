---
title: JavaScript
tags:
---

jshint
---

``` js
function exam() {
}
new (Function.prototype.bind.apply(exam, [null].concat([1,2])));
```

> ^ Missing '()' invoking a constructor.

jshint를 실행할 때 supernew 을 함께 준다.
