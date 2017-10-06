---
title: Google IO extended Seoul
layout: post
date: 2015-07-15
category: summary
tags: [google io, google io extended, web 세미나]
description: Google IO extended Seoul 세미나 요약
---

Google IO extended
===================

Chrome exchanted 2015
---------------------

### GeoLocation API
* permission

### META TAG
 * meta tag theme, content 로 주소창의 색을 바꿀 수 있다.

### simply recommend installing your web app
  설치 유도 웹 배너, 크롬에서 동작 가능(install banner)

일반적으로 manifest.json을 생성
html의 link태그에서 ref='manifest' href='xxx.json'과 같은 형식

```javascript
window.addEventListener('beforeinstallprompt', function(e) {
  //e.userChoice.then
});
```

### chrome custom tab

프로그램을 실행시키는 방법
 * browser
 * webview
둘간에 컨텍스트 공유가 되지 않는다.
chrome custom tab은 컨텍스트 공유가 가능하다.(로그인 정보 등)
sugar의 홈메뉴와 같은 느낌인듯
weight: browser > chrome custom tab > webview
cct가 뜨기전에 pre-start, pre-fetch등이 가능하다.

### native app install banner
  android만 가능

자주들어오는 방문자(2주 2일 2회이상)일 경우 네이티브 앱 설치 프로모션 배너를 띄울 수 있음

### service worker
off-line지원가능.
web worker와는 다르게 대몬과 같이 동작한다 생성이 아닌 설치의 개념으로 말해진다.
web worker는 ui thread와는 다르게 dom에 대한 접근이 불가능한데 이 점은 web worker와 같다.

push 서비스의 구현이 가능하다. chrome browser에서만 지원이 된다.

service worker의 경우 타 브라우저에서도 지원을 하기로 한 상태, 사파리는 모르겠음.
service worker가 지원되지 않는 경우에도 기존과 같이 동작이 가능한 장점이 있다.

### Polymer
첫 폴리머 사이트: google-status.com?
translate comnunity
santatracker.google
폴리머를 구성하는 4개의 스펙에 대한 관심을 가져달라.

Polymer Starter Kit
-------------------

### Web Starter Kit

  프로젝트 템플릿 & 워크플로
  speaker: @ragingwind

vulcanize: concat과 비슷한 과정(crisper?)
stream도 한번써보시구요
celenium은 무엇인지?

```bash
$ npm install -g generator-polymer
$ yo polymer
$ // yo polymer:element new-element
$ // yo polymer:seed new-element
$ // yo polymer:gh
```

mkcd 명령어가 존재했었나?

[커스텀 엘레먼트들이 모이는 곳](customelements.io)

FireBase와 Angular.js
--------------------
  기본적으로 PaaS
Parse, Firebase, bass.io
AngularFire라는 라이브리가 존재
AngularFire쓰지말고 mvc.js쓰지 말라고

RAIL
----
Response
Animation
Idle
Load

### Critical Rendering Path
requestAnimationFrame API를 사용하라
selector는 복잡도를 낮게 하라
wil-change(transform) 사용
크롬 실행실 화면에서 shift키 6번 히든메뉴

디자이너가 없어도 괜찮아
-------------------

무료 이미지 사이트

* [unsplash](https://unsplash.com/)
* [lifeofpix](http://www.lifeofpix.com/free-high-resolution-photos/)

floating action button은 1개정도
lazied button 10개 정도

버튼은 대문자

* [Guideline](https://www.google.com/design/)
* [getmdl.io](getmdl.io/)
