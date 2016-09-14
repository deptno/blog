---
layout: post
title: "netatalk 3"
excerpt: "netatalk, afp protocol 사용을 위한 서버 설치"
category: program
publish_date: "2015-07-25"
date: "2015-07-25"

author_url: /author/deptno
author_avatar: deptno-yuri
show_avatar: true
show_related_posts: true
feature_image: ubuntu
square_related: ubuntu
read_time: 2
comments: true
---

## netatalk

> OSX finder 에서 `cmd + k`를 통해 네트워크 드라이브를 쉽게 연결하여 쓸 수 있는데, `afp://` 프로토콜로 연결하려면 server에 netatalk 설치가 필요하다. 이 시점에서는 3.1.7버전이 사용되었고 설치된 서버는 hardkernel odroid xu3 lite가 사용됐으며 os는 ubuntu 14.04 32bit다.

[http://netatalk.sourceforge.net/](http://netatalk.sourceforge.net/)
``` bash
$ ./configure --with-init-sytle=debian
$ sudo apt-get install libdb-dev
$ make
```

> **O_IGNORE**관련 에러가 날 수 있는데 [해결방법](http://sourceforge.net/p/netatalk/bugs/574/) 은 아래와 같다.

include/atalk/acl.h, line 63, `#define O_IGNORE 0`를 추가한다.
``` c
#define O_NETATALK_ACL 0
#define O_IGNORE 0 // added
#define chmod_acl chmod
```

``` bash
$ sudo make install
```

> afp는 `ln -s`로 생성되는 symbolic link를 지원하지 않는 것으로 보인다.
