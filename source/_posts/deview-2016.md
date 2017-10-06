---
title: deview-2016
layout: post
tags:
  - deview2016
  - deview
category: summary
date: 2016-10-24
description: Deview 2016 세미나 정리
---


Deview 2016
===

![](https://lh3.googleusercontent.com/o5PfgOsLJtkIDI1hRbb89DS8Os9SG_IJer64JvJfJyyS6zYuI_j0bTb2TPolXO2cYhL4gL9IZwc=w2145-h1343-no)

## CTO

### keynote

회사에서 프론트엔드 개발차 지원해준 기회기 때문에 FE로 스케줄을 잡았는데 키노트를 들으니.. 로봇과 음성인식 세션이 궁금했지만 계획대로 진행하기로했다.

네이버 랩스는 무인 주행, 음성 인식(자연어), 로봇에 집중하고 있는 것으로 보인다. 무인 주행 자체에 집중하기 보다는 시각데이터를 파싱하는 목적이 큰 것으로 보였다.

M1(?) 이라는 네이버 랩스 로봇 영상이 보여졌는데 돌아다니면서 영상을 찍는데(머리가 카메라로 무장) 단순이 동영상을 찍는게 아니라 걸 돌아다니면 녹화했던 영상 데이터를 다시 보여줄 때는 걸려있던 액자들의 내용을 바꿔서 보여줬다. 3D 맵 데이터로 파싱해서 데이터화 하고 있는 것으로 보였다.

### D2

#### Naver Open Source

2016년 4개를 공개함 <https://github.com/naver.com>

* pinpoint star: 2300

나눔 고딕 폰트가 금일 업데이트 되었다.

지도 API는 일일 20만 콜이 가능하다.

#### D2 Startup Factory

스타트업 지원

#### papago

한국, 일본, 영어, 중국어 지원

#### Whale

네이버의 Chromium 기반 브라우저다. 12월 런칭 예정이다 [공식 홈페이지]<https://whale.google.com>

* 네이버 브라우저 스플릿창 지원
* 글자 선택만으로 검색
* 스마트 팝업(구석에 나옴)
* 이미지 번역
* 악성코드 제어
* 파파고 기술이 들어가있음

12월 발표

#### BLUE

생활 환경 지능(Ambient Intelligence) - 사용자가 요구하지 않아도 상황을 인식하고 추천하는 것

* understand - 이해하는 것
* anticipatory - 답/정보/action을 예상 추천
* natural UX 음성, gesture등

##### AMICA

> AMI Connected All

영상이 소개되었는데 아이템은 팔찌로 보이고 차 내에서 또 자기전에 말을 하는데 음성인식관련으로 siri와 매우 유사했다.

<https://amica.ai>

`자연어 처리는 음성에만 국한된 것이 아닌 채팅에서도 사용이 가능하다.`는 것을 기억해야할 듯

AMICA는 11/7 까지 클로즈 베타 신청 받음(막상 들어가보니 11/6일)

Samsung ARTIK 으로(Intel Edison류의 보드인 것으로 보임) 포팅되어있어 사용이 가능하다.

#### 자율주행

미국(?)의 기준인 NHTSA level 3 수준이다.

#### NAVER LABS M1

Indoor mapping robot

* 공간 인식 및 정보화, 3D 실내지도

이미지 인식기술은 인공지능 기술과 합성되면 사람의 눈과 같은 역할을 하게될 것으로 보여졌다.(카메라니 당연히 눈이지만 시각정보를 인간과 같이 파싱하는 느낌)

## Session#01(Web Payment API의 현재와 미래)

2015-10 에 표준화 작업이 시작됨

### Motivation

68%는 카트에 담고 결재하지 않음, 데스크탑 보다 모바일에서 그보다 나가는 비율이 66% 더 됨

* 폼이 너무 복잡하다
* 계정생성 귀찮음
* 모바일 디자인이 구림
* 로딩 속도

### Web Payment with Basic Cards

크롬 53(4?)에서부터 지원한다.

구성

* Payment Request API,
* Payment Method Identifiers
* Basic Card Payment

복잡한 form으로 처리 되는 것을 **one button** 으로 처리한다.

브라우저가 가지고 있다. 판매자가 수용하는 payment request를 날리면 카드 리스트만 리스트업 됨

#### benefit

##### user

* 쇼핑몰에 관계없이 같은 ux제공
* 처음 이용 쇼핑몰에서도 저장된 신용카드를 이용가능

##### merchant

* 결재 ux개발안해도됌
* 보안이슈 및 서버 운용비용 절감

```javascript
var methodData = [{
	supportedMethods: ['visa', 'master'],
}, {
	supportedMethods: ['sampay'],
	data: {
	merchantID: '12345',
...
}]
var pr = new PaymentRequest(methodData, details, options);
```

shippingOptions:  배송정보
paymentOptions: 어떤 정보를 입력받고 받지 않을지 결정

프로미스 기반

언급된 이벤트

* shipping address change event
* shipping options change event

지원하지 않는 지역이나 옵션이 올 경우에 대한 처리를 이벤트를 받아 처리할 수 있다.

```javascript
e.updateWith(//이벤트 업데이트
```

결재를 판매자측 서비스에서 처리하는데 개발 부하가 걸린다

앞으로 미래는 payment app을 사용한 결재가 있다.

### Web Payment with Payment Apps

> SamsungPay, AliPay 등을 통해

신용카드 뿐만 아니라 결재 앱을 카드처럼 지원한다.

페이먼트 앱
  * 웹 기반(현재 포커스 되어있음 w3c)
  * 네이티브 기반(완전히 정의되지 않음) => 네이티브를 트리거하는 방식으로 진행 될 예정(지금이랑 같지 않은가?)

#### service worker

* 브라우저가 죽어도 살아있다.
* 브라우저에 설치되게 되며 필요한경우(푸시 알람) 이벤트를 수신받은 브라우저에 의해 활성화된다.
* 지정된 origin 및 scope에서만 동작하며, Secure Context에서 실행됨을 보장.

payment app <> service worker 는 1:1 관계다.

```javascript
navigator.serviceWorker.register('/tree_pay.js').then(
```

## Session#02(GraphQL)

기존

* rest
	* 필드제한 한계
	* 필터 문제
	* 문서 문제
	* 동기화 문제
* json:api

### GraphQL

* query
* mutation

object type을 가지고 schema를 정의

object
	* type(required): name
	* args: 인자
	* resolve: return function

rest: postman / graphql: GraphiQL(chrome extension)

### Relay

React와 GraphQL을 엮는다.

> Node

Resource에 대한 단일 interface

> Connection(페이지네이션 특화)

Node를 여러개 가져온다

> React Relay

```javascript
React.createContainer(COMPONENT_NAME, {
	fragments: {
		editor(props의 이름: () => Relay.QL`
		fragment on User {
		pictureURL
		${EditorInfo.getFragment
```

> Mutation Config

---

query(node, connections),
변경(mutation)

참고 <http://learngraphql.com>

검색해 볼 것

* nosyncdb
* lukka
* awesome graphql

Q&A 시간에 질의 응답을 볼때 Relay와 Redux는 함께 사용하기 껄끄러운(베스트 프렉티스가 없는) 상황으로 보여지며 기존 Redux 사용시의 전략인 외각 오브젝트에서 데이터 로직을 처리하고 나머지를 퓨어 컴포넌트로 가져가는 방식이 아닌 서버쪽 데이터를 활용하는 대부분의 컴포넌트가 Relay로 바인딩 되게 된다.

Redux, Relay 모두 Flux 아키텍쳐의 영향을 받았으므로 개인적으로는 Store가 로컬에 있느냐 리포트에 있냐를 차이로 보고 있지만 컴포넌트가 데이터와 강하게 커플링되는 것에 대한 거부감이 있는 것이 사실이다. 실제 적용을 해보면서 답을 찾아야할 부분으로 보인다.

## Session#03(Electron)

검색시에는 `github eletron`으로 검색해야 한다.

slack과 microsoft가 샘플이 됨(Document에도 존재하지 않는 것들이 쓰이니 볼 것)
electron은 소스가 다 보인다. 로직을 백엔드로 빼서 구현했다고 말하고 있다.

로직을 백엔드로 빼면서 Command 패턴을 사용했는데 로직이 백엔드로간 이유는 보안상의 이유라고 한다.(여기서의 백엔드는 **local**에서 node server를 돌린다는 이야기다.

Devtron 이라는 크롬 익스텐션이 존재하는 이를 개발 환경에서 이용할 수 있다.

node 바이너리 솔루션

* enclosejs: 플랫폼에 맞춘 바이너리화

eletron-builder

개인적으로 인상적인 세션은 아니었고 개발 방법론, 패턴등을 설명했음에도 30분내에 발표가 마무리되었다.

## Session#04(한 달 만에 개발한 하이브리드 앱, 50만 사용자 서비스가 되기까지)

### 해먹남녀

하이브리드가 넘어야할 장애물

* view 전환 효과
* 300ms의 지연이 존재(웹뷰) - single tab, double tab을 잡기 위해 존재
  * hammer.js, fastclick.js가 있음
* transition
  * 3d translate css 이용
* push
  * cordova 등

백수시절 6개월동안 7개의 앱을 만듬(평균 2주)

해먹남녀는 IonicFramework을 이용해서 제작했다. 1달만에 웹뷰를 이용한 하이브리드앱을 제작했으나 2달의 안정화 작업을 거쳤다.

1주차

comonent부터 ui를 만듬
문제는 라우팅으로는 위계 제어가 힘들다.

2주차

어려운 문제들을 해결하기 시작

  * sync
  * 플러그인 업데이트 종속성
  * 다중업로드
  * 보안정책(애플)

3주차

성능

* 데이터 프리패치
* 터치 컨트롤 통제
  * 스크롤시 터치를 받지 않는 등으로 속도 향상
* 이미지 리사이징
* css blur처리가 속도가 느려서 40px짜리를 up scaling해서 속도를 향상 시킴

4주차

* png 처리 투명

### VOC 사용자 피드백 대응

https://microsoft.github.io/code-push

클라우드에서 버전이 바뀌면 파일을 보내줌 급할때만 씀

### cordova핵시config.xml 프로젝트 설정 파일

버전관리, hook script 테스크 자동화, info, plist 자동 작성

### 세션 발표자의 회고

하이브리드 앱과 네이티브 앱 사이에는 넘을 수 없는 성능(트랜지션 등을 말하는 듯) 차이가 존재하나 하이브리드 앱이 보여주는 수준 자체가 준수하기 때문에 문제가 없다.

하이브리드 앱이 네이티브를 대체한다기 보다는 빠른 Time To Market을 맞추기 적합하며 이를 활용한다는 전략적 차원으로 발표자는 설명했다.

하지만 React Native가 있다면 어떨가...

## Session#05(5년간의 네이버 웹엔진 개발/삽질기 그리고...)

Naver Labs의 신규 브라우저 Whale의 개발에 대한 스토리다. 사실 이 세션은 고민이 많았다. 선택할 수 있는 세션이 4개 였는데 다음과 같았다.

* Clean Front-End Development
* React로 개발자 2명이 플랫폼 4개를 서비스하는 이야기
* 우리 팀에서도 코드리뷰를 할 수 있을까?
* 5년간의 네이버 웹엔진 개발/삽질 그리고...

원래는 첫번째 세션을 들을까 준비했는데 Single Page Application만 5년 이상을 진행했고(심지어 임베디드 웹 브라우저에서도...) 사실 이 정도의 기간이 SPA 역사기 때문에 나이가 어린 세션 발표자거나(실망 시킨 경우가 많았다) 뻔해 보이거나 답 없는 주제(React vs Angular)와 같은 주제를 피하고 **다수**의 개발자들이 시간과 노력을 들여 진행한 프로젝트 **이야기**가 듣고 싶어 이 세션을 선택했고 결과 적으로 성공적이었다.

브라우저 세션 답게 블락 다이어그램으로 스택을 소개하고 공감가는 삽질기를 들을 수 있었다. Q&A 시간을 포함해서 45분인데 그중에 무려 30분을 Webkit 기반으로 진행한 프로젝트 연혁을 말하고 있었는데. 결론적으로는 Webkit을 버리고 Chromium 기반으로 만들었다고... 

국내에 브라우저를 낼 수 있는 기업이 얼마나 있겠는가 기대를 걸어본다. 브라우저 같은 뿌리 기술은 가지고 있으면 시너지를 낼 것도 많고 파급력을 가지는 대신 그 만큼 출시 이후에 끌고가는 문제도 있는 아무쪼록 잘 되었으면 한다.

### Whale

일단 feature가 실제 사용 패턴과 관련 된 것이 맘에 들었다 대단히 실질적으로 사용패턴을 분석을 한 것으로 보였다.

Over Tabber를 예로 들었는데(탭을 엄청나게 띄워 놓는 사람) 이유인 즉슨 이렇다는 거다.

* 쇼핑몰에서 고르는데 인덱스 페이지 하나를 두고 상세 페이지를 들어갔다가 나왔다 하기 귀찮으니 여러개를 띄워둔다.
* 사내에서 보라고 링크 공유가 왔는데 아예 안보는 것은 매너가 아니니 일단 띄워놓고 하염없이 둔다.
* 구글에서 검색을 한뒤에 원하는 결과를 찾는데 이 때 도움이 될 것 같은 것은 일단 띄워둔다.

실제로 나도 봐도 그렇기 때문에 깊이 공감을 했다. 그래서 Whale은 스플릿 뷰를 지원하고 쇼핑몰등에서 유용히 사용할 수 있도록 했다.

읽기 모드를 미려하게 지원하고

블락하면 궁금하고 블락하지 않자니 너무 떠대는 탭들을 스마트 탭뷰를 만들어서 정리해서 볼 수 있도록 구현했다.

크롬 익스텐션이 풀리 호환되며 자체 지원을 위한 것도 준비되어있다.

음성 검색(파파고?) 이 들어가있다.

퀵 서치기능이 들어가있어서 단순 드래그만으로 단어나 인물등을 검색할 수 있는데 단순 검색이 아닌 검색 기반의 회사인 만큼 컨텍스트 검색을 통해 연관성이 높은 결과 값을 받을 수 있다.

일단 기능이 마음에 든다. 고민의 흔적도 공감할 수 있어서 개인적으로는 `아 맞게 고민했구나? (내가 쓸 수 있게...)` 이런 느낌을 받았다.

12월 런치로 보인다. <https://whale.naver.com>

---

## deview 2016에 대한 개인적인 감상

회사 업무도 업무고 상당히 가기 귀찮았는데 잘 갔다왔다는 생각이 든다. 무료 세미나에 가면 제품 홍보 성격이 강하다거나 어린 세션 발표자(어리다고 뭐라 하는 것이 아니라.. 확률상)의 경우 준비 미숙으로 50분 세션을 10분만에 끝내버리는 경우도 많았는데 그 보다는 훨씬 나았고 주제 자체가 진보된 것이 많았다. 무료 했었는데 좀 의지를 태울 수 있었다.
