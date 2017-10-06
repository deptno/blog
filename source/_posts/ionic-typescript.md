---
title: Ionic + TypeScript
layout: post
category: [draft]
date: 2016-01-03T18:19:28.244
description: Angular.js와 Ionic을 공부할 겸 해서 TypeScript와 함께 앱을 제작해 보려고 한다.
---

> Angular.js와 Ionic을 공부할 겸 해서 TypeScript와 함께 앱을 제작해 보려고 한다.

## 프로토타입 제작

Ionic framework을 이용하기로 했는데 이미 Ionic은 플랫폼화가 되어버려 지금은 [Ionic Creator](http://creator.ionic.io)라는 프로토타입 제작 툴을 제공한다. 여러가지 도전을 해본 결과 UI디자이너가 없는 경우라면 프로토타입을 만드는데 시간이 오래걸려(UI를 만들어야하기 때문) 실패하는 경우가 꽤 있었다. 그래서 이를 이용하기로 했다.

### Ionic Creator

Ionic Creator는 Ionic에서 앱을 생성할때 불러올 수 있는 장점이 있어 빠르게 프로토타입을 제작할 수 있을 것 같아 바로 선택했다.

``` bash
$ ionic start [APP NAME] [TEMPLATE]
```
기본적으로 TEMPLATE에 템플릿을 지정해서 앱 생성을 하게 된다.


``` bash
$ ionic start [APP NAME] creator:[IONIC CREATOR PROJECT CODE]
```
이때 만들어 놓은 프로토 타입을 불러와서 만드려면 위와 같이 하면 된다.  
Ionic Creator에서 Export를 누르면 영상을 포함해서 친절하게 설명해준다.

---

## 프로젝트 생성

### 환경 셋업

``` bash
$ npm -g install cordova ionic typescript gulp bower
```

### 프로젝트 생성

프로토 타입을 만들었으면 코드 그릇이 될 프로젝트를 생성한다.

``` bash
$ ionic start [APP NAME] creator:[IONIC CREATOR PROJECT CODE]
$ cd [APP NAME]
$ ionic serve
```
**APP NAME** 은 앱 이름인 동시에 폴더 이름이 된다. 위 코드를 실행하면 브라우저가 뜨면서 제작한 프로토 타입이 동작하는 것이 확인된다. 여기서 부터 시작이다.

---

> 2016-01-03

아이오닉 프로젝트에서 관리되는 gulpfile.js 를 열고 ts파일을 와치할 수 있도록 해준다

``` bash
$ npm -g install gulp-tsc
```

``` javascript
var typescript = require('gulp-tsc');

gulp.task('compile', function() {

  })
```

진행중

## 참조

* [Automating Icons and Splash Screens](http://blog.ionic.io/automating-icons-and-splash-screens/)
