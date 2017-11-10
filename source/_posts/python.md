---
title: Python
layout: post
category: [service]
date: 2017-11-11
description: 머신러닝을 위한 macOS 파이썬 환경설정
tags:
  - python
  - 파이썬
  - ml
  - 머신러닝
  - anaconda
  - 아나콘다
  - virtualenv
  - virtualenvwrapper
---
> macOS 기반으로 작성되었다.

ml(머신러닝) 공부를 하기 위해 파이썬을 환경을 설정했다.  
## installation

brew는 기본적으로 설치되어 있어야 한다. [참조](http://deptno.github.io/posts/2016/osx)

설치 각각에 종속성이 있을 수 있으므로 패키지 설치시마다 터미널을 재시작하면서 하도록 한다.

아나콘다라는 파이썬 배포판으로 데이터 사이언스 관련 패키지들을 포함하고 있다. 보통 책들에서 강력히 추천하므로 설치한다. 파이썬 배포판이므로 글을 쓰는 현재 python 3.6.3을 포함하여 설치된다.

```sh
brew cask install anaconda
```

프로젝트별 독립된 개발 환경을 만들어주는 `virtualenv`와 이를 편하게 사용하도록 해주는 `virtualenvwrapper`를 설치한다.

```
pip install virtualenv virtualenvwrapper
```

사용하는 쉘의 설정 파일을 열어 다음을 추가한다. 설정 파일을 쉘에 따라 다르다.

~/.zshrc or ~/.bashrc

```bash
# python virtualenv settings
export WORKON_HOME=~/workspace/virtualenvs
export VIRTUALENVWRAPPER_PYTHON=/usr/local/anaconda3/bin/python
source /usr/local/anaconda3/bin/virtualenvwrapper.sh
```

`~/workspace/virtualenvs` 이 부분은 사람마다 다른 경로를 사용하므로 사용하고자 하는 경로를 쓰면된다. 설정파일이 적용되기 전에(터미널 재시작 전) 폴더를 만들어준다.

```sh
mkdir -p ~/workspace/virtualenvs
```

터미널을 재시작하면 스크립트가 돌고 아래와 같은 명령어를 사용할 수 있게된다.

- mkvirtualenv 프로젝트명
- rmvirtualenv 프로젝트명
- workon 프로젝트명
- deactivate

`ml`이라는 가상황경을 위한 라이프 사이클은 아래와 같다.

```sh
# 가상환경를 생성한다.
bglee@since-20171107  mkvirtualenv ml
# 가상환경를 종료한다.
(ml)  bglee@since-20171107  deactivate
# 가상환경에 진입한다.
bglee@since-20171107  workon ml
# 가상환경를 종료한다.
(ml)  bglee@since-20171107  deactivate
# 가상환경를 삭제한다.
bglee@since-20171107  rmvirtualenv ml
```

이제 가상환경은 설정이 되었고 프로젝트를 활성화(진입)한 상태로 `pip install package_name`을 할 경우 가상환경에 패키지가 포함되게 된다.
