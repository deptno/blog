---
title: TypeScript와 Redux connect
layout: post
category: [dev, ts]
date: 2017-10-03
---

타입스크립트와 리덕스로 SPA를 구현하는데 있어 기본적인 셋업이 아닌 문법적인 측면에 포커싱된 글로 타입스크립트를 활용하는데 도움이 되었으면한다.

필자는 하드 타이핑을 하지 않으며 추가적으로 린트를 쓰지 않는다. 리덕스는 자체적으로 타입스크립트 데피니션 파일(d.ts)을 가지고 있으므로 따로 타입 관련 패키지를 설치하지 않아도 된다.

## 설치

```
npm install --save-dev typescript
npm install redux
```

## redux

리덕스에 대한 설명을 돕기 위해 루트 스토어가 가지는 스테이트의 타입을 아래와 같이 정의하기로한다.

```typescript
//index.d.ts
interface RootState {
	bglee: User
	deptno: User
}
interface User {
	name: string
	email: string
}
```


### connect(..args)

리덕스를 쓰면서 가장 많이 쓰게 되는 함수일텐데 코드를 먼저 보이면 아래와 같다.

```typescript
import * as React from 'react'
import {connect, bindActionCreataors} from 'redux'

interface StateProps {}
interface DispatchProps {}
interface OwnProps {}
type Props = StateProps & DispatchProps & OwnProps
interface State {}

const Container = connect<StateProps, DispatchProps, OwnProps>(
	(state: RootState) => ({}),
	dispatach => bindActionCreators({}, dispatch)
)(
	class Component extends React.Component<Props, State> {
		render() {
			return	<p>hello world</p>
		}
	}
)
```

위 코드는 완전히 동작하는 코드이고 컨테이너를 정의하기 위해 풀어쓴 기본적인 형태다. 참고로 connect 함수는 추가적인 옵션을 기술하기 위해 4개까지 인자를 받을 수 있다.

connect를 보면 3개의 타입을 받고 있다. 순서대로 살펴 보겠다.

#### StateProps

connect의 첫번째 인자는 `mapStateToProps`로 정의되어 있으며 함수 명과 같이 `Store`의 state를 `Component`의 props로 주입하는 함수다. 이 함수의 리턴 타입이라고 생각하면 된다.

예를 들어 Store에 `bglee`라는 프로퍼티를 연결하고자 한다면 `mapStateToProps`는 아마도 다음과 같이 작성되게 된다.

```typescript
const mapStaetToProps = (state: RootState) => state.bglee
```

그러면 Container는 props로 bglee가 가지고 있는 `name`, `email`을 받게 된다. 그럼 그에 맞춰서 `StateProps`를 정의하면 된다.

```typescript
interface StateProps {
	name: string
	email: string
}
```

작성하고 보니 `User` 타입과 동일하다.

```typescript
type StateProps = User
```

로 정의해도되고 코드에서 StateProps를 User로 치환해도 된다. 이 경우는 예제를 위해 특수한 케이스기 때문에 이러하지만 여러 state를 props로 연결해야한다라면 곧 확장을 해야하니 StateProps라는 이름의 컨벤션을 유지해서 코드 일관성, 가독성을 확보하자.

다시 `mapStateToProps` 함수를 구현한 코드를 보면 아래와 같이 보일 것이 의심치 않는다.

```typescript
const mapStaetToProps = (state: RootState): StateProps => ({
	name: state.name,
	email: state.email
})
```

이해를 돕기 위해 완전히 풀어서 보였다. StateProps가 리턴되고 있다는 것만 인지하고 있으면 되며 위와 같은 코드작성은 connect가 타입을 받는 이유와 상충되므로 줄여서 작성하도록 하자.

#### DispatchProps

두번째 인자는 dispatch할 액션들이 Component의 props로 매핑된다. 위와 마찬가지로 두번째 인자의 이름인 `mapDispatchToProps`의 리턴 타입이다.

완벽한 이해를 위해 아래와 같이 액션이 정의해보자.

```typescript
const contactBglee = (from: string) => ({
	type: 'CONTACT_BGLEE'
	payload: {
		from
	}
})
```

액션은 `type`을 포함하는 오브젝트를 리턴하는 함수이며 `bindActionCreators`함수를 통해 dispatch가능한 형태가 된다.

그럼 두번째 인자의 구현부를 다시 보자.

```typescript
dispatch = bindActionCreators({}, dispatch)
```

이름을 달고 정의한 액션을 바인딩해보자

```typescript
const mapDispatchToProps = dispatch => bindActionCreators({
	contactBglee
})
```

작성된 `mapDispatchToProps`함수를 두번째 인자로 전달하게 되며 Container는 `contactBglee`액션을 디스패칭할 수 있다. Container는 `contactBglee` props를 가지고 있다는 의미며 이를 컴파일러에게 알려주기위해 두번째 DispatchProps는 아래와 같이 정의된다.

```
interface Dispatchprops {
	contactBglee: typeof contactBglee
}
```

`typeof`를 유심히 보자 contactBglee의 액션이 `from: string`인자를 취하는데 이를 재 다시 정의하는 것이아니라 기존 정의로부터 타입을 가지고 와서 추가해준다. 따라서 우린 중복정의 없이 기존 정의를 `typeof` 키워드를 통해서 쓸 수 있다.

#### OwnProps

마지막 타입은 기존 우리가 컴포넌트를 정의할 때 받는 부모로 부터 받게되는 그 인자를 정의하면 된다. **컴포넌트를 컨테이너로 변경하는 경우라면 기존 컴포넌트의 인자가 OwnProps로 변경된다.**

---

개인적으로 수없이 정의하게 되는 이런 타입과 함수들이 매우 반복적이기 때문에 `mapStateToProps`와 같은 함수를 정의하는 대신 inline으로 삽입하는 것을 선호하며 위에 설명한 것들을 함쳐 결과를 보면 아래와 같다.

```typescript
import * as React from 'react'
import {connect, bindActionCreataors} from 'redux'
import {contactBglee} from './actions' // 액션이 존재하는 곳

interface StateProps extends User {} // type으로 정의도 가능하다
interface DispatchProps {
	contactBglee: typeof contactBglee
}
interface OwnProps {}
type Props = StateProps & DispatchProps & OwnProps
interface State {}

const Container = connect<StateProps, DispatchProps, OwnProps>(
	(state: RootState) => ({
		...state.bglee
	}),
	dispatach => bindActionCreators({
		contactBglee
	}, dispatch)
)(
	class Component extends React.Component<Props, State> {
		render() {
			return	<p>hello world</p>
		}
	}
)
```

## next

이제 `render()`함수 안에서 this.props 그리고 `.`을 찍어보자.