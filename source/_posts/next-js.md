---
title: Next.js
layout: post
category: [dev, js]
date: 2017-05-08
description: react SSR 을 지원하는 next.js의 사용법을 간단히 소개한다. 
---

Next.js
===

현재 회사의 [고객사이트][0]를 [next.js][1]와 [typescript][3] 기반으로 2017-04-10 일 기준으로 배포되었다. 간단하게 소개를 하고자 한다.

## next.js

> [next.js][1]는 node.js, [react][2]기반의 유니버살 렌더링을 지원하는 프레임웍이다. 첫 접속시에는 서버로부터 첫 페이지를 구동하기 위한 청크를 가져가게되며 한번 접속한 페이지에 대해서는 데이터 청크가 있으므로 SPA와 같이 동작하게 된다.

### 라우팅
라우팅은 기본설정으로 `pages/` 디렉토리가 루트가 된다. 기본 라우팅은 `index.html`에 매칭되는 `index.js`가 된다.

디렉토리 구조가 그대로 서비스 라우팅에 매칭되기 때문에 특별히 할 일이 없어서 빠른 속도로 개발을 시작할 수 있다.

그럼 html에 해당하는 template은 어디서 제어하는가?

#### pages/_document.js

`_document.js`는 `index.html`의 템플릿에 해당하는 정보를 내려주는 리엑트 컴포넌트로 시그니쳐가 `React.Component`의 상속이 아닌 `next/document` 패키지의 Document 컴포넌트를 상속해서 만들게 된다. 자세한건 문서를 참조한다.

#### pages/_error.js

SSR이 지원되기때문에 데이터를 서버에서 미리 패칭하여 내릴 수 있다. 이런 경우를 포함해서 예외처리가 제대로 되지 않은 경우 `500`에러를 내리게 되는데 그에 대한 에러 `status`를 받아 그에 맞는 화면을 내릴 수 있다. `_document.js`와는 달리 `React.Component`를 상속한다.

### api

next.js에서는 리액트 컴포넌트를 내릴때 특수한 `static` 메서드를 정의할 수 있는데 이 함수는 서버와 클라이언트 모두에서 사용되며 서버일 경우와 클라이언트일 경우에 대한 분기를 여기서 정할 수 있다. 이를 통해서 서버인 경우에 대한 처리, 클라이언트에 대한 처리를 추가할 수 있다.

#### getInitialProps()
```typescript
export default class Index extends React.Component<null, null> {
    private store: Store<RSquareStore>;

    static async getInitialProps({req}) {
        const server = !!req;
        const store  = await storeCreator(reducer, null, server);
		
        try {
            await store.dispatch(getCatalogue());
        } catch(ex) {}
        
        const initialState = store.getState();

        return {initialState, server, catalogue: initialState.themes.catalogue};
    }

    constructor(props) {
        super(props);
        this.store = storeCreator(reducer, props.initialState, props.server);
    }

    render() {
        return (
            <Provider store={this.store}>
                <Layout>
                    <Head>
                        <title>{`SEO_TEXT`}</title>
                    </Head>
                    <Home catalogue={this.props.catalogue}/>
                </Layout>
            </Provider>
        );
    }
}
```

단순한 구현은 위와 같다. 구조에는 redux가 쓰였으며 이를 위해 스토어를 생성해서 내리고 있는 걸 볼 수 있다. `<Head>`컴포넌트도 보이는데 `helmet` 역할로 헤더쪽으로 자식 태그들을 펌핑해준다. 동적으로 바껴야하는 메타태그들을 여기서 처리하면 된다.

### pros
* 라우팅이 정형화된 디렉토리 구조로 되어 있기 때문에 직관적이다.
* API가 적다.
* 유니버설 렌더링의 지원한다.

### cons
* 표준으로 자리잡았다 싶은 기존의 리액트용 라이브러리들과 함께 사용하려면 커스텀 셋업이 필요하다.

###

[0]: https://www.rsquare.co.kr
[1]: https://zeit.co/blog/next2 "next.js"
[2]: https://facebook.github.io/react/ "react.js"
[3]: https://www.typescriptlang.org/ "typescript"
