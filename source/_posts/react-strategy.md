---
title: React 구현 전략
layout: post
category: [dev, js]
date: 2016-07-22
tags: [react]
description: React 구현 전략
---

임베디드 브라우저에서 사용되는 앱을 Web 기술을 통해서 구현했던 터라 일반적인 Web 서비스의 구현이 처음이기도 해서 시행착오가 꽤나 있었다(인증, 배포, 서버 통신).
처음으로 주어진 사내 서비스에 대한 구현을 잘 알지도 못하는 React로 구현하면서 느낀 점, 그리고 다음 구현에 대한 개인적인 글이다. 

### TypeScript를 적용해야겠다.

서버와의 통신이 많은데 interface는 컴파일레벨에서 많은 도움을 준다. 아쉬운 점은 React에서 중점적으로 쓰이는 [Rest/Spread](https://github.com/Microsoft/TypeScript/issues/2103) 지원이 아직 되지 않는 점인데 이 부분에 대한 부분은 pure js로 라이브러리화 하여 사용하는 면이 귀찮지만 그래도 득이 있을 것으로 생각된다. 실무에서는 데이터 통신 부분에서만 TypeScript를 적용하여 반쪽짜리 사용을 하고 있는데 이를 통해서도 득이 있었다.

### Container와 Component에 대한 개념을 탑재하자

`Redux`를 적용하면서 `Container`와 `Component`에 대한 개념적인 부분이 약했다. code상으로는 redux의 `connect`를 통해 wrapping된 `component`가 Container인데 UI에서 제공하는 페이지당 하나의 Container를 둬서 통신에 대한 모듈로서의 역할을 명확히 하고 나머지 UI에 대한 부분은 `Pure Component`로 작성하는 것이 중요하다. 이에 대한 처리가 미흡하면 여기 저기선 `connect`를 하게 되어 data flow가 망가지고 컴포넌트의 순수성이 망가져서 test code의 작성을 불가 하게 한다.

### Immutable에 대한 적용은 뒤로 미뤄보자

`react`, `redux`, `immutable`이 마치 세트처럼 다뤄지고 있지만 각각의 역할이 있고 도입 동기가 있어야했다. immutable은 사용성에서 pure javascript와는 이질적인 부분이 존재하고 객체가 pure인지 immutable객체인지에 대한 판단을 코드레벨에서 요구하게 된다. 사용성에 대한 부분을 개선한 `seamless immutable` 같은 경우에는 class의 instance를 유지하지 못하는 단점이 있다. `react-addons-update`가 제공하는 방식 또한 우아하다고 느껴지진 않지만 일단 최소한의 Container에서 사용하게 된다라고 가정한다면 바이러스처럼 퍼저나가는 immutable을 나중에 수습하는 비용을 뒤로 미룰 수 있다.

### UI component가 raw data를 직접 참조하지 않도록 한다.

UI component가 참조하는 데이터 객체는 raw data를 품은 UI data class를 만들어서 사용한다. 이는 데이터 구조의 변경등이 있을 경우 수정 영역을 한정 할 수 있는 장점이 있고 이에 대한 저장을 store를 통한다면 강점이 있을 것 같다. 실무에서는 `seamless immutable` object의 instance를 유지하지 못하는 문제로 새로운 레이어를 만들어서 참조를 유지하도록 했다.

### Store

Store에 서버로 부터 받은 데이터 형태를 그대로 저장하고 있었는데 [normalizr](https://github.com/paularmstrong/normalizr)등을 통해 Object형태로 key값을 통해 바로 참조할 수 있는 형태가 여러 API를 통해 data의 수정등에 유연하게 대처할 수 있지 않나 한다.
