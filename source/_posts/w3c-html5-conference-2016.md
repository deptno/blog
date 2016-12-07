---
title: W3C HTML5 Conference 2016
tags:
  - pwa
  - amp
category: summary
date: 2016-12-07 21:10:13
---

개인 정리

---

![](https://lh3.googleusercontent.com/OsicCBywLiPusnZQD31bwLImBkmsJWksa5VD4mfWZQfRTWY62sG_jUTH47pfkuhskisXm1bN8ZoEfbm80hIGAESFrF0psQIHmtKqKFh2zRfQxlqtEomjfuJwHZXvHiCAXGkgj_wg4UY6gjq_D0h6SiJ7Dv5YGb70OKDIvqPafLuJ9q5RiEZRIDvtebpGLoUhaVJaSZxctEW9TXLaI1IDlymPvfmvg0YK9wcbGMQM2xbXuX2IH1uzI5zEhInVhcpMwIyHUf-E-u9nwO0G56x9FSHQVBAo8cLhBBBfbL-bGY6ZsGJlMaXEGmph49MbofjWVRnDF2AblUcW59LcJnxr0mgDrV1HrPKDY6o4AnEg2nrh-i4KbLOKrRnIGDm1jEh_YFJtHe19PLDHnLyv-3ZyjfYiDrKjA-AM9z0pPtKhR60CX5nAkrKQBJI_F5loag5fA0g5Kv9vdOh0H02Xyhle_nJVSzmDmw3iWSHw9WuxPoN9UhVfVxTtx1To4QoinhWAGhwoAhFwFvpSssB18rQOLnxjLLjycWI8CWcUbSnIf-AUsLPIjj8z6Yi3h4cI5fhYP2TT8bTFv_9xEYuSEW3htguMuYkjN49RHA9_oxpo19S12ALKUw=w895-h672-no)

## 키노트 1(박종목)

### 웹의 발전

* 스태틱 웹
* 웹 2.0(ajax) - symbolic boom
* read, write web(muchine to muchine), 스맨틱 웹(figure, article, header tag), 웹 3.0

### 네이버의 인공지능 사례들

  * 페이지랭킹은 오래됬고 머신러닝으로 랭킹 먹인다.
  * 지식 쇼핑 40억개 상품, 4000카테고리가 있어서 딥러닝을 통해 자동분류한다.
  * 위치 기반 검색 지역의 특성등을 인공지능으로 추출한다.
    * 전주 한옥마을에 대한 블로그 데이터의 자연어를 분석해서 전주 한옥마을에 대한 특성들을 분석함(크롤링과는 좀 다름)
      * Context-aware analysis based on charateristics of loacation
      * 해시 태그화 등
  * 이미지 검색시의 정렬등
  * 상품검색의 스타일 서치(유사 상품) topic model을 추출한다.
  * 인식기술이 많이 발전함
    * 머신러닝을 하면서 인식률이 85% => 95%로 증대
  * 음석 합성도 지원함(사전, 뉴스 읽기, 라인 사전)
  * 기계번역(통계기반 번역) => 머신러닝(뉴런) 투입하면서 번역률이 2배정도됌
  * amica.ai, 12초 whale browser는 파파고 엔진으로 웹페이지 번역 지원됨

사용자와의 인터렉션은 음성기반으로 바뀔꺼다.(시리 등등)
챗봇(사용자들은 메시징하는데 더 많은 시간을 쓴다(웹은 20%정도)) - 페이스북
자율주행(아직은 연관성 별로 없음)
스마트 홈(IoT)
로봇

### 관련된 웹 표준

  * Voice XML
  * 시맨틱 웹(3.0에서 실패했던 것이 지금)

### 이에 대한 안좋은 시각(생각)

  * 웹표준이 아닌, 애플, 구글, 페이스북등이 사실상 리딩
  * 산업에 변화에 비해 웹 표준은 느리다는게 걸림
  * 태생이 다큐먼트 컨셉

### Web의 Next?

  * IoT

## 키노트 2(김국현)

IT 세대론...

코볼은 | C, C++에
Java(자바는 코볼을 품음) | JS
에 막히고 3세대로 가고 있다.

특이사항 없음

@goodhyun

## 패널토의

웹이 다큐먼트인가? 거의 UI로 사용된다. 그러면 모델이 다큐먼트인게 맞는가
1조개의 센서, 세금을 블록체인으로 걷는다.
서로 다른얘기 함.. 여튼 지금 혁명중

Q: 네이버 브라우저 왜 만드셨어요?

A: 사용성이 안좋다. 선두업계 주자라 인터넷 환경 자체를 개선하겠다.

Q: 삼성도 MS따라서 오픈소스하나?

A: 변명함, artik이라는 보드도 오픈소스로 내놓음 ecma준수하는 jscript 만듬(40k정도)

Q: 웹이 성숙된 표준을 가지고 잘 따라고 있는가?

A: 웹 !== 웹브라우저, 칩에도 js call이 들어간다. 쉐이딩 영역도 js가 먹을꺼같다. iot도 먹고있다. 웹을 구성하는 기술들로 간다.

Q: JS로 IoT하는데 이게 웹인가?

A:

 * 웹이라고 하는듯 의견이 갈림
 * 스펙은 잘 따라가지 못하기 때문에 프레임웍에서 많은 일들을 해야하며 리액트를 디펙토로 가던지 등등 맞다고 생각되는 것을 밀어야한다.
 * 웹의 기본 철학은 탐험과 연결이다.

Q: 4차 산업 혁명 준비 예산은? 내년

A: 주용환

 * 40억 규모: 엑티브엑스 개선, 웹 표준 확산
 * 기업지원은 웹이 새롭게 쓰이는 서비스에 지원하려함 올해 2억 수준이었는데 10억 이상 수준으로 편성하려함, VR등 국내사례가 별로 없음.
 * 빅테이터, 인공지능 이쪽이 지원 받을게 많다.

Q: 뉴런번역 API 제공 계획 없나

A: 모른다 ㅠㅠ

---

## 트랙 B

### Second Screen & WebRTC

#### WebRCT 표준 현재와 미래

최진호 - REMOTE MONSTER

페이스 챗 카카오톡 슬랙, 아마존 등등에서 다 사용이나 아직 Draft단계이며 5년정도밖에 되지 않는다.

전화기 오픈소스라고 생각하면 이해하기 빠르며 표준화를 진행중이다.(구글이)
P2P지만 서버(시그널 서버)는 필요하다.
토폴로지 구성 및 브로드캐스팅이 가능

API

 * getUserMedia
 * RTCPeerConnection
 * RTCDataChannel

표준화된 프레임웍이 없다.
네트워크 및 안드로이드 파편화에 따라 성능차이가 많이 난다.
UDP를 사용

그럼에도 불구하고

* 커뮤니티가 강하다. <https://www.facebook.com/groups/rtc.korea>
* WebRTC PaaS도 존재한다.
* 1200+ 서비스, 브라우저들이 지원함, 3.1조억+ 기업 인수, 투자 규모
* 엔지니어 구하기가 힘들어서 인수할 정도로 귀족 개발자
* 17년 1Q Recommandation 예정
* 엣지도 지원, 애플도 지원 예정 (H.264)
* 크롬 55에 RTC관련 업데이트가 대거
* 용량이 크기때문에 IoT에 대응하기에는 쉽지않지만 빌드 잘하면된다..
* AR, VR에 유용할 것으로 예상된다.
* Google Duo = WebRTC + QUIC 빠른 서비스.
* 통신, 방송, IoT분야에서 활약할 것으로 기대된다.

#### WebRCT 서비스 개발사례와 시사점

이랑혁 - 구루미

HLS보다 딜레이가 적다. RTSP, RTMP, HLS 가능
일반전화 연결이 가능하다(?)

협업 > 교육 > 상담(의료) > 관제 등 순으로 비지니스 니즈가 있었음

WinXP, IE 이슈 해결

 * 일렉트론
 * 세계적으론 20%대이니 기다려라(국내는 70%이상)
 * 아니면 앱 만들어서 배포해라

기회

 * 크롬 기반 브라우저 확산(웨일 브라우저)
 * 에코 확장
   * 대기업의 신규 서비스
   * 교육 및 의료 부문 전환중(정부 3.0)
 * ActiveX 제거

#### Second Screen과 웹 커넥티비티

송효진 - LG전자

다른 기기로 화면이나 앱을 보내서 실행하는 것

커넥티비티

  * 네트워크를 통해 떨어진 두 지점을 연결

ECMA
W3C
IETF(L3)
IEEE(L2)

IETF

  * HTTP/2(One TCP Connection)
  * QUIC(based on UDP)
  * IPv6
  * IPv6 over Bluetooth/NFC(IOT 기반으로 이터넷이아닌디바이스에서 가능)

Application

  * W3C Presentation API
  * W3C Remote Playback API

Underlying Protocol
 
  * SSDP, mDLS

기대
  * 교육 분야
  * 전광판(QR 대체)

FlyWeb Project

웹페이지가 서버를 구동하고 디바이스가 여기에 접속 가능

### 웹고속화

#### AMP(Accelerated Mobile Pages)는 어떻게 빠른 성능을 내는가?

김태훈 - 네이버 <http://facebook.com/groups/webfrontend>

* 구글 라이브러리다. amp.js 다른거 할필요없이 가이드를 따르면 된다.
* 인스턴스 로딩이 가능하다.
* 커스텀 엘리먼트 기반이다.
* url + #development=1 를 사용하면 에러 뿜어준다.
* amp.js는 외부 js를 허용하지 않는다.
* AMP Cache - 구글이 CDN 으로 다 뿌려준다.
* HTML <-> AMP-HTML 페어로 구성 또는 AMP-HTML 온리
* 슬랙등에 링크걸면 AMP로딩해준다.
* amp 컴포넌트를 사용해서 리소스를 로딩하게되면 뷰포트에 보이는 시점에 레이지로딩한다.
* 크로스 브라우징 해결해준다.
* 속도는 엄청 좋다는 말로 부족하다.
* https만 가능하다(video)
* 커스텀 js를 사용하지 못하는 치명적인 단점이있어서 이벤트 페이지 같은 경우에 적합할 것으로 보임.

특징

  * 비동기 스크립트만 로딩 허용
  * 모든 리소스의 사이즈 지정
  * 확장 컴포넌트들이 렌더링 차단 없이 실행한다.
  * 써드 파티 자바스크립트 크리티컬 패스에서 제거
  * ?(CSS관련)
  * 웹폰트 효율적으로 허용
  * GPU 가속 애니메이션으로만 제한됨(레이아웃 변경 CSS는 막힌다)**(css triggers 참조)**
  * 리소스 로딩 순서 제어(뷰포트에 의해)
  * 페이지 즉시 로드
  * 리플로우 최적화
  * 스타일 변경 작업을 배치작업을 통해서 최적화한다(코드 붙여서 작성하면됌)

6억+ 페이지가 700k+ 사이트가 작성되었다.

커스텀 엘리먼트를 사용해서 라이프 사이클을 제어한다(이를 통해 뷰포트안에 있을때만 로딩등 처리)

#### 프로그레시브 웹앱(PWA)

송정기 - 삼성전자

모바일 웹사이트

장점

 * URL, 연결성, 빠른 배포(URL > store)
 * 표준 기반 다양한, 폭넓은 기기 지원

단점
 * Native 대비 떨어지는 UX
 * 성능
 * 기능

---

Manifest + Service Worker + Push

---

<https://whatwebcando.today>

PWA - 홈스크린 인스톨레이션, 오프라인 모드, 푸시 메시지

* 머무는 시간이 3배 증가
* 40% 더 재접속
* 70% 홈스크린을 통해 들어오는 유저가 구매할 확률이 더 높다
* 3X 데이터를 덜 사용한다

PWA List 앱 참조

PWA를 구성하는 표준 기술

##### Web App Manifest

네이티브와 동일한 앱 접근성을 제공한다. 크롬, 오페라, 삼성 브라우저, 사파리
메니페스트가 추가됏을때는 브라우저에서 탭했을때 메뉴가 다르게 뜬다.
홈화면에 인스톨레이션을 지원하는 로직은 아직 사파리가 지원하지 않는다. 인스톨레이션 팝업은 브라우저별 휴리스틱 알고리즘을 통해 제공된다.

<http://pwa.rocks>

PWA indication의 조건

* https://
* Service worker
* WebApp Manifest

`theme_color`를 통해 상태바 색도 바꿀 수 있다.
`display`를 통해 풀사이즈 앱으로 띄울 수 있다.

Splash screen - 크롬이 들고나오고 삼성 인터넷도 탑재 예정, 로딩 전 앱 로딩 화면처럼 만들어준다.

##### Service Worker

오프라인 우선, 백그라운드 처리

이벤트마다 워커가 종료된다. 웹 워커와는 다르다. 실 브라우저 구현(크로미움 기준) 2-30초는 살아있다.

```javascript
const navigator.serviceWorker;
sw.register(scriptURL, {scope: scopeURL});
;;
oninstall = e => { /* pre-cache here */ };
onfetch = e => { /* respond with magic */ };
onactivate = e => { /* Deleting cache is on you * }; // 서비스 워커 버전이 변경됐을때
registration.update(); // 강제 업데이트
```

SW의 이벤트가 가지고 있는 `watUntil` 메서드를 통해 SW가 죽는 것을 방지할 수 있다.
캐시에 애드하는 형식 캐시에 매칭되지 않으면 거기서 `fetch`한다.

Service Worekr Cookbook 찾아볼 것 <https://jakearchibald.com>

##### Push API

푸시의 효과는 검증됨.

필수요소

  * Background Service **(Service Worker)**
  * Push 등록/해지/이벤트 **(Push API)**

`push`이벤트를 받아서 캐싱에 저장하고 노티피케이션을 띄움 띄운 후에는 클릭 이벤트를 받아 그에 따른 처리.

<https://medium.com/samsung-internet-dev>

삼성 인터넷 베타 프로그램 신청 <https://goo.gl/1yFP1L>

### Web 엔진

#### Naver Webkit - Sling Project 소개 및 오픈소스

김준걸 - 네이버 랩스

* 슬링 4년 6개월 작업했고 오늘 깃헙에 오픈했다.
* 애플의 웹킷에서 fork
* win/android 지원
* JS엔진 - JSC vs V8(과할 수록 유리 V8, 실사용에선 JSC, 슬링은 JSC)
* 리소스가 부족해서 안드로이드로 개발 => 스크립팅 => C++ 소스 제네레이팅(android pp, github 참조)
* 크로미움 네트워크 성능이 뛰어남
* GFX Tool for Sling 이라는 디버깅 툴 만들어서 그걸로 선검증 후포팅
* WebExtensions API가 400개 정도 되서 아직 다 지원은 못함
* 

#### Chrome Dev Summit 2016 참관기

김지한 - 네이버 랩스

* PWA 포커싱
* AMP to PWA
* 매 단계마다 20%의 사용자가 떨어져나간다.(설치나, 가입 등등 모두)
* 3G 기준으로 5초 이내에 인터렉션이 가능해야한다.
* 홈 화면에 추가된 사이트는 4배 더 많이 방문한다(알리바바, 2016)

PRPL pattern - Push, render, Pre-cache, Lazy-load 

`<link rel="preload"/>`

크롬 51+, 오페라 41+, 안드로이드 5+

<https://www.flipkart.com> PWA의 정석
<https://www.housing.com> 부동산 체크

##### Crediential API

`navigator.credentials` 인증을 브라우저에 던진다.

##### Web Payment API

브라우저에 결재 던짐

##### Debugging the Web

* 크롬개발자도구
* Lighthouse 익스텐션(PWA)

##### 결론

* PWA의 핵심 - 오프라인 지원, 로딩 성능 향상
* Lyft등 선행 사업자들도 이머징 마켓 중심으로 적용 중
* 로그인, 결재 등이 브라우저로 이동(크롬, 안드로이드), 이에 대한 강력한 지원

#### HTML5기반 웹앱 그리고 다가올 HTTP / 2

커넥션 제약으로 이미지 등을 로딩할때

* image sprite
* local cache`<link rel="prefetch" href="image.png"/>`
* domain lookup, `x-dns-prefetch-control`, `preconnect`
* minify
* `prerender`
* `preresolve`
* google에서 instant page 검색

##### HTTP2

* 바이너리 기반 프로토콜
* 헤더 압축
* 멀티플렉스 스트림(프로세스, 쓰레드 같은 개념)
* 스트림 프라이어티티(리소스 의존성 지정가능)
* 서버 푸쉬

1.1 에선 핑퐁 치던걸 2에선 한방에 내림

APM<https://ampproject.org> + PWA

포커싱이 HTTP2가 아니라 아쉬움