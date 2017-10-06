---
title: yarn
tags:
  - npm
  - yarn
category:
  - setup
  - js
date: 2016-12-12
description: facebook 에서 만든 `NPM` 의 래퍼 `yarn`에 대해서 설명한다. `yarn`을 소개한다.
---

![](https://yarnpkg.com/assets/feature-speed.png)

`yarn`은 동시 다운로드를 지원하는 `npm`이라고 보면된다. 디펜던시 트리를 저장하고 있어서 빠르게 인스톨을 하므로 CI등에서 위력을 발휘한다.
기본 지식은 [문서](https://yarnpkg.com/en/docs/cli/info)를 참고하면 된다.

> `yarn global add [package]` 커맨드를 통해 인스톨을 한 경우 `npm`과 달리 실행이 안되는 문제가 있다.

```sh
> yarn global add typescript
yarn global v0.17.8
warning No license field
[1/4] 🔍  Resolving packages...
[2/4] 🚚  Fetching packages...
[3/4] 🔗  Linking dependencies...
[4/4] 📃  Building fresh packages...
success Installed typescript@2.1.4 with binaries:
      - tsc
      - tsserver
warning No license field
✨  Done in 1.56s.

> tsc
zsh: command not found: tsc
```

`bash` 를 이용하고 있다면 될 수도 있을 것 같은데 필자와 같이 `zsh`를 사용하고 있다면 아래와 같이 한줄을 추가해자.

```sh
> echo "export PATH=\$PATH:`yarn global bin`" >> ~/.zshrc
```

이후 실행

```sh
> tsc -v
Version 2.1.4
```

`npm`을 통해서도 인스톨을 했다라면 패스 문제로 버전 문제가 생길 수 있으니 둘중 하나만 사용하자.

---

2016-12-26 추가

`brew`를 통해 `yarn`을 설치한 경우 위 방법으로는 `yarn global add [package]`로 추가한 모듈이 동작을 안한다. `npm global`, `yarn global`, `brew`가 꼬여서 생기는 문제일 것 같은데 일단 해결책은 `brew`를 통해서 설치했다면 다시 아래를 추가로 입력해주면 된다.

```sh
> echo "export PATH="$PATH:$HOME/.config/yarn/global/node_modules/.bin"
```
