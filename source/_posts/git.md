---
title: Git
layout: post
category: [dev, git]
date: 2016-04-12
tags: [git, submodule]
---

Submodule
---

``` bash
$ git init
$ git add *
$ git submodule add [git repo#1] [target path]
$ git submodule add [git repo#2] [target path]
$ git commit -m "add submodules"
$ git remote add origin [git repo#0]
$ git push -u
```

저장소를 생성하고 서브 모듈로 두개 저장소를 가져온후 이를 리모트에 푸시한다.

``` bash
$ git clone [git repo#0] [path]
$ cd [path]
$ git submodule init
$ git submodule update
```

이제 다른 경로에서 아까 푸시한 리모트 경로를 클론하고 서브 모듈까지 모두 내려받는다.

``` bash
$ tree
├── /
│   ├── /submodule-01
│   │   ├── sub01.file01
│   │   └── sub01.file02
│   │       ├── sub01.file03
│   │       └── sub01.file04
│   └── /submodule-02
│  		├── sub02.file01
│   	└── sub02.file02
├── root.file01
└── root.file02
```

[git repo#0]을 기준으로 하면 커밋된 파일은 root.file01 root.file02 submodule-01 submodule-02가 된다. submodule들의 폴더만을 가지며 하위 폴더는 submodule 내 각자의 저장소에서 처리된다. 폴더가 커밋되는 이유로는 리비전 정보를 가지기 때문인 것으로 보인다.
