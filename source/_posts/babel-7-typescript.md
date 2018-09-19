---
title: babel@7-typescript
typora-copy-images-to: babel@7-typescript
tags:
  - babel7
  - babel@7
  - typescript
  - preset-typescript
date: 2018-09-19 18:23:40
---


## Babel@7 :heart: TypeScript

바벨 7.0이 나오면서 타입스크립트의 빌드 환경변화가 생겼고, 긍정적이다. 기존의 버전업과는 꽤나 다른 철학적(혹은 디자인) 차이가 있어 적어본다. 미니멀한 **babel@7** + TypeScript 의 조합을 만들어보면서 정리하도록 한다.

### Babel@7

바벨은 오랫동안 내가 피해 왔던 시리즈 빌드 시리즈중 하나인데 **7.0**에 와서는 그 컨셉에 변화가 있었다. 디테일하게는 능력상의 무리가 있고 가볍게 설명하자면 바벨은 이제 타입스크립트 프리셋을 추가함으로써 **flow** 와 마찬가지로 타입관련 코드를 모두 스트립(삭제) 한다. 그리고 순수 바벨로만 컴파일(트랜스파일)을 하게 된다.

기존에는 웹팩과 함께 먹이려면 웹팩에서 로더를 통해 **타입스크립트를 자바스크립트로 변환** 후 다시 바벨을 통하게되는데 기본적으로는 타입스크립트 컴파일러(`tsc`)는 자바스크립트 컴파일 기능을 가지고 있다. 헌데 `css` 임포트와 같은 특수 구문 로드등 여러가지 복잡성이 얽히면서 내 제어를 벗어나 버렸고 그냥 받아들였다. 내가 아는 사실은 바벨을 걷어낼 수는 없다는 것 정도다.

헌데 이번 **7.0** 에서는 그냥 바벨은 타입스크립트를 벗겨내고 자체적으로 빌드를 하며 타입스크립트는 빌드 과정이 아닌 외부에서 타입 어시스트를 받는 컨셉이다. `tsconfig.json` 의 `noEmit: true` 설정을 통해 `.js`를 생성하지 않으면서 타입 어시스트만 받고 실제 빌드는 바벨이 담당한다. 때문에 오류가 있어도 바벨은 무시하고 빌드를 하게 되며 타입스크립트에 대한 롤은 그 외부로 남겨진다. 종속성이 떨어지므로 당연히 유연성이 더 높다.

### Preset 패러다임

지금 빌드에서는 프리셋이 패러다임이다. 기존과 프리셋 빌드 패러다임이 어떻게 다른가를 이해하는 것은 지금 이 방향이 올바른가에 대한 판단할 기회를 제공하니 알아볼 만한 주제다. 대충 내 감으로 느낀 것이기 때문에 실제와는 다를 수 있으나 그 부분은 "이 사람 틀렸네?" 하고 넘어갈 문제로 보인다 어쨋든 무엇이 다른가 얘기해 보겠다.

프리셋을 보는 방향에는 두가지가 있다. 하나는 투박한 단어 선택이지만 **입력 값**의 선택 이고 하나는 **출력 값**에 대한 정의다.

#### 입력 값

**기존**의 빌드 방식은 바벨에 사용하고자 하는 기능들을 추가해야했다. 예를들어 `{...spread}` 와 같은 구문을 사용하기 위해서는 `spread` 어쩌고하는 플러그인등을 빌드 환경에 추가로 인스톨 하면서 사용하는 것이다.  여기에 나중엔 프리셋이 등장한다 아마 이런 것이었던 것으로 기억한다. `preset-stage-0` 스펙이 되기 전까지 실험적 기능, 안정화, 실제로 스펙에 포함되기 위한 마무리 단계 등이 존재하는데 이를 프리셋으로 묶어서 이를 사용하는 것이다.

이렇게 되면 스펙을 이루는 개별적인 기능을 임포트하는 수고를 덜 수 있다. 이는 어느정도 제공되고 있었고 `post-css` 또한 이러한 방식으로 전환되고 있다.

#### 출력 값

출력 프리셋에 대한 이해는 `node` 가 이해를 도울 수 있을 것이라 생각된다. 입력값에 스펙 얘기를 했었는데 타입스크립트에서는 출력에 대한 타겟 스펙을 정할 수 있다.

```json
{
  "compilerOptions": {
    "target": "es2018"
  }
}
```

`es2018`, `es2017`, `es2016` 뭐 여러 타겟이 있는데 실제와는 괴리가 있다. AWS에 람다를 배포한다고 생각해보자. AWS는 근래에 들어서야 `node8.10`을 런타임으로 지원하기 시작했다.

**"어?"** 타겟을 뭘로 지정해야하는지 알 수가 없다. 그래서 대충 잘돌아가는 옛날 컨피그를 사용한다. 네이티브로 지원되는 기능도 코드를 통해 돌리게 되는 비효율과 가독성의 저하를 가져온다. 좀 더 도전을 해본 사람이라면 https://node.green 과 같은  곳에서 정보를 얻어 그에 맞게 타겟을 지정할 수도 있다. 그런데 여럿이 작성하는 코드에 무엇이 존재하는지 어찌 일일이 파악하겠는가?

그래서 바벨에서는 타겟(출력) 프리셋을 지정할 수 있다. 아까도 말했듯이 이제 컴파일은 바벨이 혼자 책임지는 구조다. `es2015`로 컴파일해줘가 아니라 타겟을 **노드 버전으로 명시**한다.

```
const presets = [
  '@babel/preset-typescript',
  ['@babel/preset-env', {targets: {node: '8.10'}}]
]
```

AWS는 `node8.10` 런타임을 지원하므로 이제 고민없이 바벨에게 최적화된 빌드가 떨어지겠거니 기대할 수 있게 됐다.

## 👷‍♂️ 핸즈온

이런걸 핸즈온이라고 하는 것 같아 따라 붙여봤다. :kissing_smiling_eyes:

```bash
mkdir babel-typescript
yarn init -y -p
yarn add -D @babel/cli @babel/core @babel/preset-env @babel/preset-typescript typescript
```

그럼 `package.json` 파일과 디펜던시들이 인스톨 됐으니 `index.ts` 파일을 생성하자

```typescript
export default (pkg: Package = {version: "0.0.1"}) => pkg.version

interface Package {
  versionsss: string
}
```

그럼 이번엔 `babel.config.js` 파일을 생성할 차례다.

```javascript
module.exports = {
  presets: [
    '@babel/preset-typescript',
    [
      '@babel/preset-env',
      {
        targets: {
          node: '8.10'
        }
      }
    ]
  ]
}
```

이제 설정을 모두 했으니 컴파일을 해보자.

```bash
npx babel index.ts
"use strict";

Object.defineProperty(exports, "__esModule", {
  value: true
});
exports.default = void 0;

var _default = (pkg = {
  version: "0.0.1"
}) => pkg.version;

exports.default = _default;
```

결과도 확인했다. 참고로 `-o` 옵션을 통해서 결과 파일명을 지정할 수 있다. 매우 잘된다.

### 흠… :thinking:

`index.ts` 파일에서 인터페이스를 선언할때 의도적으로 오타를 냈었다.

```typescript
  versionsss: string
```

이 부분이다. 바벨은 그런거 모르고 타입스크립트 관련한 코드는 그냥 삭제하고 빌드한다. 타입과 관련된 어시스트는 타입스크립트가 처리해야한다.

그럼 타입스크트를 활용하기 위해 조금 더 가보자. `tsconfig.json` 파일을 생성하도록 하자.

```typescript
{
  "compilerOptions": {
    "target": "es2018",
    "noEmit": true
  }
}
```

설정은 대충 최신 스펙을 반영하기 위해 `es2018` 을 타겟으로 했다. 중요한 포인트는 `noEmit` 이다. 이를 통해 타입스크립트는 타입체킹만을 하며 실제로 빌드에선 제외시킨다.

```bash
tsc
index.ts:1:33 - error TS2322: Type '{ version: string; }' is not assignable to type 'Package'.
  Object literal may only specify known properties, and 'version' does not exist in type 'Package'.

1 export default (pkg: Package = {version: "0.0.1"}) => pkg.version
                                  ~~~~~~~~~~~~~~~~

index.ts:1:59 - error TS2339: Property 'version' does not exist on type 'Package'.

1 export default (pkg: Package = {version: "0.0.1"}) => pkg.version
                                                            ~~~~~~~

```

대충 위와 같은 에러가 날 것이다. 이를 통해 타입 체킹은 테스트 했고 `versionsss` 에서 ~~sss~~ 를 삭제하고 다시 `tsc` 를 실행한다. 그럼 아무 에러가 표시되지 않고 `js` 파일 또한 생성되지 않는다.

이제 타입스크립트를 통해서 어시스트만을 받고 빌드관련 설정은 바벨로 이관되어 깔끔하게 분리되어 서로의 역할을 한다.

