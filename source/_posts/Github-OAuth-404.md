---
title: Github OAuth 404, 그리고 헤더의 중요성
typora-copy-images-to: Github oAuth login 404
date: 2018-08-07 20:32:50
tags:
  - github
  - OAuth
  - login
  - 깃허브
  - 로그인
  - 404
---

> 이 것도 한 3번인가 삽질을 한 것 같은데.. 기록을 해두지 않아 매번 삽질을 한다. 오늘도 4시간 정도 날린거같다...

## 3 티어 인증 프로세스

- 클라이언트(웹)에서 깃헙로 리다이렉션을 시도한다.
- 깃헙에서는 스코프랑 유저 허락 받고 허락 되면 어플리케이션 정보 저장시에 사용했던 콜백(서버)로 **code** 를 전송한다.
- 서버는 **code** 를 **token** 으로 교환한다.
- 교환 후에는 **token** 을 가지고 다시 클라이언트로 돌아갈 수 있도록 **302** 리턴을 하면서 쿼리 파라메터로 **token** 을 넘긴다.

## 이슈

코드에서 토큰으로 교환하는 과정에서 깃헙이 **404** 를 내뱉는다. 무엇이 문제인가 :eyes:

```javascript
const response = await fetch(`https://github.com/login/oauth/access_token`, {
  method: 'POST',
  body: {
    client_id    : GITHUB_CLIENT_ID,
    client_secret: GITHUB_CLIENT_SECRET,
    code,
    // state,
    // redirect_uri
  }
})
```

### Content-Type 헤더의 중요성

문제는 이것 저것 해보다가 설마 헤더 때문인가? 라는 생각에 이르렀다. 코드는 다음과 같이 수정해봤다.

```javascript
const response = await fetch(`https://github.com/login/oauth/access_token`, {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json'
  },
  body: {
    client_id    : GITHUB_CLIENT_ID,
    client_secret: GITHUB_CLIENT_SECRET,
    code
  }
})
```

바로 **200 OK** 가 떨어진다. :fist_raised:

### 서버는 서버 엔지니어 마음

언젠가는 URL에 뒤에 `/` 를 붙이냐 마느냐로, `200` vs `201`,  `200` vs `404` 등등 피곤하고 시나리오적인 흐름과 주관이 섞일 수 밖에 없는 문제가 있다.

소 제목은 **서버는 서버 엔지니어 마음이다.** 라고 달았지만 흥분을 가라 앉히고 보면 내가 잘못한게 맞다. 이런 날들을 위해 준비되어있는 스펙을 무시했다. 최근 근무한 직장도 그렇고 기본적으로 `JSON` 이 일반화 된 세상에서 살고 있다보니 기본이 `JSON` 일 것이라 예단했다. 깃헙도 꽤나 오랜 시간을 버텨왔고 매우 많은 유저가 사용하는 만큼 바꾸지 못하는 레거시가 있기 마련이다.

## 리턴 값

자성을 하고 리턴 값을 확인하려고 코드를 짜니 또 에러가 난다.

```javascript
const json = await response.json()
```

고통을 교훈으로 헤더를 확인했다.

```javascript
{
  'content-type': [ 'application/x-www-form-urlencoded; charset=utf-8' ],
}
```

두번 반성한다. 리턴값도 당연히 `JSON` 일리가 없었다. `response.text()` 도 가능하지만 요청 헤더에 `Accept` 를 추가했다.

```javascript
{
  headers: {
    'Content-Type': 'application/json',
    'Accept': 'application/json'
  }
}
```

이제 잘 날아온다. :boxing_glove:

## 다시 한번 헤더의 중요성, 다시 한번 서버는 서버 엔지니어의 마음

**API** 를 사용하다보면 헤더도 마음데로 들어온다.  서버리스의 **serverless offline** 플러그인을 통하면 `event.headers.referer` 가 들어오는데 반해 실제 서버에 디플로이되면 `event.headers.Referer` 가 들어온다. 이 경우는 플러그인 버그라 할 수 있겠지만 대소문자도 예단해선 안된다.