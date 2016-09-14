---
layout: post
title: Jekyll 블로그 운영
excerpt: 지킬을 통한 블로그 운영 컨셉
category: [setup, draft]
author_url: /author/deptno
author_avatar: deptno-yuri
show_avatar: true
show_related_posts: true
feature_image: https://lh3.googleusercontent.com/hZHHR5j5unjHkYdnNWdluWHZni6JgecIoQCLIpo7DIJSr2Z9n9dM5WLfelmnQgDo_7dcJCUGzgl4s04C5NU1vkEQKVn0lHu0HWg9BoZo1szNUiVLIKL6PUrgCvMzP9PV4VLoB6m7OnpMgpp6v67Ii7uura7wlZAH-SZIlr1arwd_LqJLYoVCdslj-BH9Tcg0Z_3h3wOobXCix_DBBQEu34VSynn3c_QaJJ6zfdNmv7rDgiweE1IRhQAhaRVg5W3cGNC5G8C9aYgdkYXOT8rxtAQdRJCUXHz0a9saWnj_7ANJZx3Uqvwhp_GDC-_MCshw0OZ_C0FZ70_r-OdvP3SRw6N6FYPiEJqV2dd6kiOe2oyxN5wvlwopOqsIu2vvs7xhtkX-YZiRT0lY7Lzjtj1DEPGS9j3KSqAd_UgWKpxjXzspyZvp_Gtx7V9GV-LKj4g7Jq-wrbYRI-_y1GDAR4nlUhiwuXTTwQF5mROKR46yItiEiOZzQsdWf-RMtnm9Z14zvJsypTbk_BEJixGZTurU_wdOpsxpw9MyIocdt86mwF9nJ_tE45L2StdIug3AVu-6InH1jQ=w960-h489-no
square_related: https://lh3.googleusercontent.com/DS60UTd7-ItzTJ1O6s0s3f4-XYd6CRWcHnBgloQL4_EkSQcgk1HDLrCuMyITDtB2hx8Gbj_h0NWpenMhwswZTr-fseIU2yVlr0wMYOxPdN0YWPKTujbRIUREVW5dJeEK7A2R3dmCMRRu8MnZfi-Y6GFHIPl62E_ixK8v6NmpsL2YjfB9MhGr3IH3jv6UXY7HD6MiyVZ8uzMWplQyExOfy7ihPByvLmUQ869ZOpJr_G0h_XAvb_VGcV1EBbv_FsnoOTa49rG3TH1EXyiRIz9VVoBuRnwtuK8KhPblAvO3h-9UvNIsQLp0YBUN3AayxgNuY4rHlQaKLJi4RefSGrm2VCY6u_Pdbts8rryf3FuIorF8CAQPUFE0JhuEq5whXkzDe8X66dgtz1VMteRCJXhLPOcNCCCLx5nWmHxVf4Fk7a1YIX0h1hk58hDtSSeD4yAmtGkoKtpgxDlisRWf2CGnlKa2McUXj4D3pDw7jdWXdhXLplqMHJ8_PKPr-sf7qMg5yPBzYmJKoDyDM2XAUh-ooDklout43DJDUMiVHI_Ybq0jG08bLpsSiwFJpiKdah5DoIPCzg=s830-no
read_time: 10
comments: true
publish_date: 2016-01-11T10:03:50.615
date: 2016-01-30T16:12:12.347
---

> 나는 메모장을 열고 .md 로 저장을 했더니 블로그에 글이 포스팅 되어 있다.

# 설치

## install Jekyll

``` bash
$ gem install jekyll
```

[지킬 인스톨 가이드](http://jekyllrb.com/docs/installation/)를 참고하면 되겠다.

## Forking theme

테마를 하나 포킹해야한다. 유료건 무료건 지킬 버전에 맞는 테마를 구글에서 검색해서 포킹한다. "**jekyll theme**" 로 [구글에서 검색](http://lmgtfy.com/?q=jekyll+theme&l=1)하면 쉽게 얻을 수 있다.

포킹한 후에는 이를 반드시 **클론** 하여 버전관리를 포함한 시작 뼈대를 갖추도록 한다.

## configuration

테마 루트에 있는 **_config.yml** 파일을 자신에 맞게 수정한다.

## operate Jekyll

두가지 방법이 있다.

* 지킬 자체가 웹서버를 런칭하고 스스로 빌드
* 웹서버는 Apache, Nginx등에 위탁하고 지킬을 통해 빌드만 하는 방법이다.  

내 선택 기준은 다음과 같았다. 기타 이미 서비스를 하고 있는 웹 페이지등이 있다면 후자를 이용하고 아무것도 해본적이 없으면 첫번째 방법을 이용하면 된다. 어차피 웹서비스는 80포트를 통해 제공되며 이는 겹쳐선 안되므로 하나만 사용 가능하다. 전자의 방법은 [지킬 인스톨 가이드](http://jekyllrb.com/docs/installation/)를 참고하면 되겠다.  
후자는 이미 다른 웹서버가 사용되고 있다는 가정하에 지킬 테마 밑의 제네레이트된 파일 폴더인 **_site** 폴더를 운영되고 있는 웹 서버에서 서빙할 수 있도록 해주면된다. 이 부분은 각자 웹서버 프로그램의 설정 가이드를 참고해야한다.(그래봤자 아파치 아니면 Nginx다)  
**_site** 폴더는 빌드를 통해 업데이트된다.

``` bash
$ jekyll build --watch #watch option은 변경된 파일이 생기면 그 파일을 리빌딩한다.
```

빌드는 위 커맨드를 통해 가능하다.

# 운영

설치가 끝나면 이제 포스팅을 작성해야하는데 하고자 하는 바는 아래와 같았다.

* 서드파티 댓글 시스템 추가
* 이미지와 컨텐츠(마크다운 파일) 분리
* 컨텐츠 버전 관리
* 테마 버전 관리

## 서드파티 댓글 시스템 추가(Disqus)

디스커스로 정했다. 설치도 너무 쉬워서 가입하고 방식에 맞는 스크립트를 받아(유니버설 스크립트) 포스트에 사용되는 레이아웃 파일을 **_layout** 폴더에서 찾아 수정해주면 된다. HTML을 조금 이해하고 있으면 도움이 되겠지만 눈치로 잡을 수 있는 수준이다.

## 이미지와 컨텐츠 분리

이런 공수를 따로 들이는 이유는 관리적인 측면에서의 차이 때문이다. 코딩을 하다보면 재사용성을 높이기 위해 모듈화, 캡슐화가 진행되는데 그와 다르지 않다. 컨텐츠를 버전관리할 터인데 이미지를 여기에 함께 관리하는건 바람직하지 않다는 개인적인 의견이다.  
드롭박스, 원드라이브, 구글 포토등 여러가지 클라우드 저장소가 있는데 여기서는 무제한 용량을 제공하는 구글 포토를 이용했다. 사용하는 방법은 간단하다. 어차피 사진은 폰으로 찍을 것이므로 폰에 구글 포토(안드로이드라면 기본적으로 깔려있을지도 모른다.)를 설치하고 공유 앨범을 하나 만든다. 공유 앨범의 이름을 블로그 포스팅을 위한 무언가로 지정해주면 관리하기 편하겠다.  
사진을 찍고 구글 포토를 열어서 공유 폴더에 추가해준다. 글은 어차피 PC에서 작성하므로 구글 포토에 접속한후 공유 폴더에 있는 사진을 열고 이미지 링크 복사를 통해 이미지 주소를 가져와 이를 컨텐츠파일에서 사용하면된다.

## 컨텐츠 버전 관리

테마 폴더 아래 **_posts** 폴더가 우리가 마크다운 파일을 작성하는 곳이 될 것이므로 이 폴더를 버전관리 하면 된다. 깃허브나 비트버킷이 있는데 비공개 저장소는 비트버킷이 가능하므로 나는 이를 이용했다.

## 테마 버전 관리

말이 테마지 지킬에서는 테마가 서빙에 필요한 모든 구조를 다 포함하고 있으므로 설정파일인 **_config.yml** 파일을 포함해 layout파일들이 모두 여기에 포함되므로 UI와 관련된 모든 수정이 여기서 발생하게 된다. 이 때문에 포킹이 필요하게 되며 무료 테마의 경우 보통 깃 허브를 통해 포킹하게 될 것이므로 포킹후 테마를 클론하여 여기서부터 시작하면 이미 버전관리는 시작됐고 수정하는 것들에 대해서 커밋, 푸쉬등을 퉁해 그대로 버전관리를 진행하면된다.

## 추가적인 운영 팁(?)

마크다운을 통해 로컬 텍스트 에디터를 통해 포스팅을 할 수 있다는 강력한 장점이 있지만, 글 도입부에 [YAML](http://yaml.org/)을 통한 포맷을 반복적으로 해줘야하는데 이런 부분은 각자가 사용하는 에디터에 대한 무언가로 해결 할 수 있으리라 생각된다.  
단순하게는 포맷 파일을 하나 두고 열어서 복사하는 방법이 있겠고 개인적으로는 아톰을 사용하는데 아톰은 자바스크립트를 통한 핵이 가능하고 플러그인을 만들기는 공수가 큼으로 아래와 같은 방법을 통해 하고있다.

![블로그 포스트 스니펫](https://lh3.googleusercontent.com/QaQPpiJQ2uHKCJu_QgiAB5ANryC-YFCTIehnMQnvLLZ5fqb_HOTjOjJ_vcsrwqnYUzHPsMVoqBp7vlYJ_j6HLgSDxEk9zPkrqLTFBWxE58n-X9-x_8m8ugiaOFsi0h4HVXZCMihWUCpapTSyO7eSrHbieqi8yLg6qxcigOf2dJdX7xcNDS2jeIPGdsqSsmeX44gdK-cM7i5QChSIbYhR173DhFH1l6AnEoFNK_9lgIShiAIWCPO-o3kv8slz7606-lpKHZ3ONcFjPx5am-HmSH6LNMBFzSjyOSxga_3lIvJWP1xixo_15q88vI8Hgjp7gpVRd8onpsRpxUdHe6SK_-82itMiMHuIPedSGpa5XZ3Hi4CVsoexwvJd0mDmvlcdAb1Hz724FEAjUTwOVA4pkGf0luzLWArQXGlgfDlRMZn9DGQfn0j8yHauUKxVwxRzcJ807tEnB5RCtlAUDQTa43L51yAK2TkuF0ZmXodYDvBLHPB-dIti6lnwTjlKEpRWDFOJzhj9flnstFU7CaEK6zfag9uXCo1f-y68A7uPEV6slt0ixQ6iXel-uS1wsx0Vjun-Vg=w1176-h656-no)

> 2016-01-30

## 웹페이지를 통한 이메일 발송

구매한 지킬 테마에 **contact** 페이지가 포함되어있는데 이메일을 웹페이지에서 바로 보낼 수 있는 서비스다. 이를 위해선 서드파티 이메일 서비스가 필요하다.
