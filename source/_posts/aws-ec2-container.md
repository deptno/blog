---
title: AWS EC2 Container Service(ECS)를 통한 배포
tags:
  - ecs
  - ecs_cluster
  - ecs.config
  - aws
  - ec2
  - ec2 container
date: 2016-11-05 15:00:00
description: aws ec2 container service 를 통해 서비스를 배포하자.
---


> `ecs` 는 현재 서울 리전에서는 지원되고 있지 않다. 글을 쓰다가 개인 업무가 있어서 끊어 쓰다보니 매끄럽지가 않은데 질문사항이나 잘못된 점이 있으면 피드백 주세요.

![](https://lh3.googleusercontent.com/KLmTMdEkWSEkTDl9l8oe840Cxd8Siu4MG4qJJ9Z6emVjRhsH3_uC9WPhstkLq_q5qoRiFv20Ws_VSx7c0bsO1FfG8lfV8OSDEcQWHCLf4ZB5aOtRai4BmLmDJPEA1hVadtsyBVxJRzomTv-J-svCzFpyX1_jBwSdmi_FUCj31iJKNM0WsZc4nxkBHYz_2oBD2vbCa-HzAhcsDwgt61VzKRTyQHH0W9_ygKllbofMhHa6_x4hTwkmVW_MgGunvS2EULiJfnscxlW-QveQ9EGGWjx__xHBk_QhXYdbbPhf5U4rZw3nkejvTa12BfgnsvB9ROWvL3nFCO_NtKoAabNCVJ5cpZrHokxy7Aul02nTHcyXIjRpwuEaZd4Us3rQ2y_rL5sWEtE70FQCBmM_iYwfKTsTdPg_fkQkGO4nj5Idbe-RcCQ0oOsGcw1f67VTwsowi-S9T999WFdSSHsg3Y1jwrZ1JLnHkTZWwwRO0QEAjLAW1g3NsH3DPKuH6qdG3i269fauB-rsdqsqoB4s7VKaTN-JcuqRdqwuZAcBG1zYimIhFdVe7Qvobn8mwDbTo-InwPUM_qSClu6SRTYnTgGMDyhmsijwYHmePk1q8M5RM7RZlBLW5w=s803-no)
## 개요

`aws console` 을 통해  `EC2 Container Service`(ecs) 에 진입하면 3개의 메뉴가 보인다.

  * Cluster
  * Task Definitions
  * Repositories

> Cluster

하드웨어 클러스터를 의미한다. EC2의 모임이라고 생각하면 편하다.

> Task Definitions

이 곳에서 Docker 파일을 통해 `task` 를 정의한다. 정의한 `task`를 `cluster`에 할당하여 동작시킨다고 생각하면된다. `task` 에서는 `메모리를 얼마나 사용할 것인가?` 등의 `docker` 설정을 포함한다.

> Repository

`aws` 에서 사용할 수 있는 `docker hub` 라고 이해하면 빠르다. 이 곳에 빌드된 `docker image` 를 올린다.

## 실제 구성

### 저장소 구성 및 업로드

##### docker image build

먼저 어플리케이션을 구현하고 `Dockerfile`을 통해 이미지로 빌드한다(빌드 과정은 이 포스트의 영역을 넘어간다).

##### docker 저장소 생성

빌드 된 이미지를 `ecs repository` 에 업로드하기 위해서 `ecs` 서비스의 `repository` 에 진입하여 `Create repository` 버튼으로 저장소를 생성한다.

##### docker image upload

친절하게 `ecs`에서 `repository` 를 선택하면 `View Push Commands` 라는 버튼을 통해 업로드 방법을 볼 수 있다. `login`, `build`, `tag`, `push(upload)` 순으로 명령어가 적혀있는데  저장소 url, region을 자신에 맞게 수정하고 올리면된다.

### task definitions

##### task 생성
`Task Definitions` 메뉴에서 `Create a Task Definition` 버튼을 눌러 생성한다. `Container Definitions` 에서 `Add container` 버튼을 통해 컨테이너를 등록해야하는데, `image` 에서는 프로토콜(`http://`)을 제외한 이미지 경로를 넣어준다.

메모리의 경우는 `task` 가 실행될 실제 컨테이너의 메모리 안에서 설정하면된다. 실제 사용하는 메모리보다 적게 잡을 경우 속도저하, **응답없음** 등을 경험할 수 있다. 실제로 PDF를 생성하는 서버를 구현했다가 랜더링 할 메모리가 모자라 응답없음을 경험했었다.

추가적으로 포트 매핑, CPU unit 등을 설정해준다.

### cluster 에 배포

##### cluster 생성

##### service 생성

클러스터를 생성하고 나서 클러스터에 제공할 서비스를 생성한다. 여기서는 위에서 정의한 `task` 와 이를 서비스할 `cluster` 를 지정하고 몇개의 `task`를 돌릴 것인지 등을 셋업한다. 추가적으로 `ELB`, `auto scaling`등도 셋업을 한다.

**ELB** 최근에 **Application Load Balandcer** 가 추가 되면서 기본설값이 되어 있다. 기존에 쓰이던  ELB는 **Classic Load Balancer** 니 참고 바란다.

---

여기 까지는 그냥 따라오면 되는데  중요한 문제가 있다. **cluster** 에 등록된 컨테이너 인스턴스가 없다. 이제 이를 세팅해야한다. cluster에 ec2 instance를 등록하기 위해서는 `ecs agent`가 설치된 이미지가 로드되야한다. 이건 유저가 스스로 설치할 수도 있지만 기본적으로 아마존에서 제공하는 이미지가 있다.

테스트를 위해 **ec2 instance** 를 생성한다. **AMI**는 **Amazon ECS-Optimized Amazon Linux AMI** 를 이용한다. 


**auto scaling group**, **launch configuration** 을 통해 서비스를 하는게 정석이다.

여기서 **user data** 를 설정해서 인스턴스가 ecs에 접속해서 클러스터에 등록을 할 수 있도록 설정해 줘야한다.

```sh
#!/bin/bash
echo ECS_CLUSTER=CLUSTER_NAME >> /etc/ecs/ecs.config
```

user 데이터의 내용은 위와 같다.

여기서 **CLUSTER_NAME** 부분을 통해 인스턴스가 로딩되면서 ecs agent가 실행되게되고 여기서 ecs 에 접속하여 cluster에 등록되게 된다.

---

### update revision

docker image가 업데이트 되어 재 배포가 필요한 경우에는 task의 new revision을 선택하고 container를 눌러서 한번 업데이트를 누른다(현재 컨테이너가 같은 docker이름을 참조하더라도 재 참조를 하도록) 리비전이 생성되면 생성된 task를 서비스에 업데이트하게 되면 현재 구동되고 있는 task가 active로 업데이트된 task가 primary로 등록되며 시간이 조금 지나면 active가 내려가고 primary가  active되면서 서비스가 이어지게 된다.

## trouble shotting

###* cat /etc/ecs/ecs.config

```
Could not register module="api client" err="ClientException: Cluster not found.
```

* `/etc/ecs/ecs.config` 에 cluster 이름이 정확히 올라가 있는지 2번 확인한다.
* 그래도 안되면 ecs agent api call이 public ip를 요구하므로 public ip가 있어야한다.(또는 프록시 등의 다른 방법) ec2를 확인한다.


