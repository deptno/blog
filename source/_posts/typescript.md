---
title: TypeScript
tags: requirejs typescript amd amd-dependency
---

<!-- toc -->

TypeScript로 작업을 작업을 한다라면 여러 문제들이 부딪히게 될 텐데 대 부분 문제들의 시작은 아마 JavaScript를 TypeScript로 포팅하면서 시작 될 것이다. 그에 대한 정리다.

requirejs.config paths관련 하여 alias된 모듈 로드
---

이미 프로젝트가 RequireJS를 사용하여 의존성 관리를 하고 있다면 의존성에 관한 경로 문제를 겪을 수 있다. 문제의 원인은 TypeScript는 컴파일을 하는 언어고 런타이밍에 세팅되는 `requirejs.config` 함수의 설정을 모르기 때문이다. 코드를 보자

``` javascript
// config.js
requirejs.config({
  paths: {
    "jquery": "lib/jquery"
  }
});
})
```
paths를 통해 "jquery" 위치를 정의한다.

``` typescript
import * as $ from "jquery";

$(function() {
	console.log("document ready");
});
```
ts파일을 작성한다.

``` bash
tsc main.ts --m amd
```

> main.ts(1,20): error TS2307: Cannot find module 'jquery'.

모듈을 찾을 수 없다는 에러메시지가 발생하지만 `main.js`파일은 의도한 대로 생성된다.

``` javascript
define(["require", "exports", "jquery"], function (require, exports, $) {
    "use strict";
    $(function () {
        console.log("document ready");
    });
});
```

의도한 대로 파일이 생성됐다. 문제는 컴파일 에러가 나는 부분인데 `config.js` 파일의 내용을 주입시킬 수 있는 방법이 없다.

TypeScript 1.8에서 도입된 [AMD-dependency 구문](https://github.com/Microsoft/TypeScript/wiki/What's-new-in-TypeScript#amd-dependency-optional-names)을 통해 해결 할 수 있게 됐다. 일단 구문을 모자

``` typescript
/// <amd-dependency path="lib/jquery" />
```
위 주석(?)을 파일 윗줄에 삽입함으로서 의존성을 삽입가능한데 일단 import 구문을 통했을 때와 차이를 알아야한다. 다시 컴파일을 하면 일단 `$`에 대한 오류가 당연히 발생하게 된다.

> main.ts(3,1): error TS2304: Cannot find name '$'.

``` javascript
define(["require", "exports", "lib/jquery"], function (require, exports) {
    "use strict";
    $(function () {
        console.log("document ready");
    });
});
```

에러와 함께 컴파일된 js파일이다. 모듈은 로딩 되었는데 참조가 불가능하다. 이에 대한 문제를 해결 할 수 이도록 `name` 프로퍼티가 1.8에 들어가게 되었다.

``` typescript
/// <amd-dependency path="lib/jquery" name="$"/>
declare var $;
```

`name`에 `$`를 지정해주고 실제 코드에서 사용할 수 있도록 `declare var $;` 구문을 주석 아래쪽에 삽입하여 `TS2304` 에러코드를 해결한다.

``` javascript
define(["require", "exports", "lib/jquery"], function (require, exports, $) {
    "use strict";
    $(function () {
        console.log("document ready");
    });
});
```

의존성과 컴파일 에러 모드 해결되었다.

---

`search-response.ts(2,15): error TS2307: Cannot find module 'lodash'.`

```ts
import _ from 'lodash';
```

es6 방식의 export가 지원 되지 않아 생기는 에러로 보인다.

```
import * as _ from 'lodash'
```

위와 같이 해결한다. 

