---
title: webpack babel 설정
layout: post
category: [dev, js]
date: 2016-07-21
tags: [babel, webpack]
description: webpack과 babel 설정
---

## webpack을 통해 번들링

[Webpack](https://webpack.github.io/)은 파일을 하나로 묶어주는 번들러고 [Babel](https://babeljs.io)은 ES2015/2016/2017 => ES5로 내려주는 트랜스파일러다. 둘을 세트로 셋업하는 경우가 흔하고 사용 방법이 두가지가 존재하고 많은 예제들이 너무 여러가지를 포함하고 있어서 기본을 셋업할 수 있도록 정리하려 한다.

```sh
webpack ./entry.js bundle.js
```

Webpack을 위와 같이 실행하면 entry로 부터 참조는 모든 의존성을 묶어 bundle.js 파일 하나로 묶어낸다. 실제로는 `config` 파일을 생성해서 쓰게 되다.

`webpack.config.js`
```js
module.exports = {
    module: {
        loaders: [{
            test: /\.jsx*$/,
            exclude: /node_modules/,
            loader: 'babel'
        }]
    }
};
```

이 파일에서 로더를 설정하여 `js`, `jsx`파일을 만나게 되면 `babel-loader`를 통해서 번들링되며 이 과정에서 babel은 babel 설정을 로드한다. 여러가지 설정 방법 중에 `.babelrc`를 생성하는 방법을 선호하며 json을 다음과 같이 작성한다.

`.babelrc`
```json
{
  "presets": ["es2015", "stage-0", "react"]
}
```

사용하는 프리셋들은 babel 플러그인들의 집합인데 각각 간단히 설명하면 다음과 같다.

    * es2015: es2015(ES6) 문법을 지원한다.
    * stage-0: es2016(ES7) 이상의 실험적인 문법을 지원한다.
    * react: jsx를 지원한다.

이렇게 설정을 해 놓으면 entry.js 부터 ES2015 혹은 그 이상(stage-0 때문)의 문법으로 작성이 가능하고 바벨을 통해 ES5로 `bundle.js`파일이 생성되게 된다.

클라이언트에서 로드하게 되는 static file은 위와 같은 방식으로 만들면 된다.

## runtime에 적용

node로 작성을 한다고 가정해보자. 클라이언트에서 생성된 파일을 로드하는 것과는 달리 실행을 시켜야한다. 보통 이런식이 된다.

```sh
node index
```

node는 버전에 따라 다르지만 현재를 기준으로 `es2015` 문법이 완전히 지원되지 않는다. 이전에 작업한 파일을 가지고 시작한다고 가정하면 하나의 파일만 작성하면된다.

`index.js`
```js
require('babel-register')({
    "plugins": [[
        "babel-plugin-webpack-loaders", { "config": "./webpack.config.js" }
    ]]
});
require('main.js');
```

`babel-register`로드하면서 플러그인으로 `babel-plugin-webpack-loaders`를 설정하면서 `webpack-config.js`를 로드한다 이후 파일(`main.js`) 부터는 es2015 문법을 지원하게 된다.

## babel-node

`babel-node`는 node의 wrapper라고 생각하면된다. `.babelrc`만 작성되어있으면 바로 `es2015`등의 문법 사용이 가능하다. 사용을 위해선 설치를 해야한다.

```sh
npm install --save-dev babel-cli
./node_modules/.bin/babel-node main
```

성능상의 이슈가 있으며 프로덕션 모드에서는 사용하지 않는다.

---

## trouble shooting

> problem 2016-08-22

```sh
$ npm start
> open http://localhost:3000/
/Users/bglee/tmp/paperboy-client/node_modules/rimraf/node_modules/glob/glob.js:106
  for (var j = 0; j < set[0].length; j++) {
                            ^

TypeError: Cannot read property 'length' of undefined
    at Function.glob.hasMagic (/Users/bglee/tmp/paperboy-client/node_modules/rimraf/node_modules/glob/glob.js:106:29)
    at rimraf (/Users/bglee/tmp/paperboy-client/node_modules/rimraf/rimraf.js:61:36)
    at ExecBuffer.cleanup (/Users/bglee/tmp/paperboy-client/node_modules/exec-buffer/index.js:124:2)
    at ExecBuffer.<anonymous> (/Users/bglee/tmp/paperboy-client/node_modules/exec-buffer/index.js:103:10)
    at FSReqWrap.readFileAfterClose [as oncomplete] (fs.js:439:3)
```

> solution

```sh
npm i rimraf@2.2.8
```

ramraf와 glob관 버전 호환성 문제로 보인다 rimraf를 강제로 버전을 고정시켜준다.
