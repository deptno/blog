---
title: React.js 프로 리액트
layout: post
category: [summary]
date: 2016-07-22
tags: [pro react, 프로 리액트]
description: React.js 프로 리액트를 읽고 난 후기
---

> React.js를 이용한 모던 프런트엔드 구축

읽으면서 중요하다고 생각하는 것 몇 가지에 대한 정리

### Props convention

`onChange`등과 같이 camelCase가 사용되며 DOM API의 기준으로 element에 class등을 넣기 위해서는 `className` 프로퍼티가 대신 사용된다.

### Controlled Component와 Un Controlled Component

둘을 나누는 차이는 가볍게 코드 상에서 props.value의 존재 유무라고 생각하면 된다.
value가 존재하면 이에 대한 제어를 위해 `onChange`와 같은 이벤트를 받아 state를 관리하게 된다. 이 말은 `Pure Component`가 아니라는 말과 의미가 통하는 부분이 있다.

#### Controlled Component

Controlled Component는 `user action(click, typing 등)`에 의해서 변하는 상태 값을 `Component`가 핸들링한다.

#### Un Controlled Component

상태값을 유지하지 않기 때문에 특정 시점에 Un Controlled Component 로 부터 데이터를 가져오는 작업이 필요하다. `value` prop이 사용되지 않기 때문에 기본 값 등의 셋업을 위해서는 `defaultValue`가 사용된다.

### Virtual DOM

Virtual DOM을 통해 렌더링을 다시할 부분을 계산하게 되는데 이때 List형식의 DOM에 대해 insert, remove, move 등을 인지 할 수 없으므로 `key` 프로퍼티가 사용된다. 실무에서 적용할 때는 시간이 없어(?) `warning`을 지우기 위해 의미없는 index값을 대입했었으나 이 부분이 의미 있기 쓰이려면 List의 각 row가 가지고 있는 데이터의 매칭이 이루어져야 하므로 data의 `id`값 등을 넣어야한다.

### Shallow Copy

React에서 컴포넌트의 렌더링에 대해 비교할 때 레퍼런스를 비교하게 되는 것으로 예상(?)하고 있는데 이에 대한 처리를 정확히 해서 렌더링을 줄여 퍼포먼스 향상을 위해 Shallow Copy를 이해할 필요가 있다. 이에 대한 처리를 위해 facebook에서 만든 [Immutable.js](https://facebook.github.io/immutable-js)이 있고 순수 자바스크립트처럼 사용이 가능한 [seamless-immutable](https://github.com/rtfeldman/seamless-immutable) 등의 구현체가 있으나 각각이 가지는 단점이 존재하여 단순한 큰 규모의 어플리케이션이 아니라면 얕은 복사에 대한 이해를 바탕으로 해결 할 수 있는 부분이 있을 것으로 보인다.

Object, Array등 레퍼런스등의 객체를 직접 수정하는 것이 아닌 새로운 객체를 대입하는 케이스가 `flux` 아키텍쳐에서는 자주 일어나는데 이를 위해 `Object.assign`, Array의 `map`, `concat`등의 메서드를 자주 사용하게 된다. 새로운 레퍼런스를 반환하지만 Array나 Object안에서 다른 Array나 Object를 포함하고 있다면 이에 대한 레퍼런스는 유지되게 된다. 이에 대한 이해가 있으면 될 것 같다.

#### 관련 라이브러리

[react-addons-update](https://facebook.github.io/react/docs/update.html)

### Test

React에서는 test framework으로 [jest](https://facebook.github.io/jest/) 를 권장하고 있다.

#### 관련 라이브러리

[react-addons-test-utils](https://facebook.github.io/react/docs/test-utils.html)

---

### ETC

이 외에도 Router(react router)와 flux(redux가 아닌) 자체, 서버 사이드 렌더링, 퍼포먼스 튜닝등의 내용을 담고 있다.
