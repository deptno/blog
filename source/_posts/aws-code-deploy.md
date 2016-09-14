---
title: aws code deploy - ci
tags: 'aws, code deploy, ci, travis ci, circle ci, slack'
date: 2016-08-15 17:55:25
---


## TODO

- [ ] 권한 이슈에 따른 이슈 (IAM role)
- [ ] 트러블 슈팅
- [ ] github - ci - slack 링크 연결

---

aws를 잘 모르는 상태에서 aws code deploy와 travis ci, slack을 통해 배포하기 위해 삽질한 기록.

입사한지 4개월이 조금 넘어가는데 프론트엔드를 리딩을 하다보니 배포에 대한 니즈를 느끼게 되었고 그 삽질한 과정이다. 첫번째 프로젝트는 ci까지는 연결하고 배포는 쉘 스크립트를 이용해 scp로 운영하는 서버에 배포했다. 현재는 두번째 프로젝트를 조용히(?) 진행하고 있는데 n대의 서버에 배포하는 프로젝트를 곧 진행하게 될 것 같아 이 기회에 진행했다.

회사는 현재 aws를 통해 서비스를 제공하고 있고, github enterprise를 통해 코드를 관리하고 있으며 travis-ci를 도입하는 단계에 있다. aws code deploy 는 배포 코드를 여러대의 aws 서버에 순차적으로 또는 한번에 배포를 할 수 있게 하는 시스템이라 생각하면 된다.

코드 작성 부터 배포 까지의 한 사이클은 다음과 같다.

> github

code 작성 => commit => push => pull request => merge

> ci

pull => test => deply

> code deploy

배포파일 download => 각 서버에 워하는 대로 배포

github => ci에 대한 것은 [github - travis ci - slack](XXX) 포스트를 참조한다.

---

## code deploy 설정

배포는 여러가지 방법이 있는데 `aws code deploy` 는 aws에서 여러대의 서버에 배포를 할때 쓰인다. aws를 잘 모르는 상태에서 시간이 꽤나 걸렸는데 권한 설정에 대한 이해가 발목을 잡았던 것 같다.

우리는 지금 코드가 푸시되면 ci를 돌리고 있고 그 결과를 slack으로 받고 있다. 이제 ci가 성공했을 때 deploy를 해야한다. code deploy가 의미를 가지려면 여러대의 서버를 사용해야하고 auto scaling등의 기능을 사용해야하겠지만 여기선 배제한다.

알아둬야할 사항은 다음과 같다.

* aws console 에서 우리가 접근 할 서비스는 아래와 같다.
    * EC2
    * CodeDeploy
    * Identity & Acceess Management(IAM)
    * S3(s3를 통해 배포된다면)
* ec2 instance 자체에 code deploy를 위한 `role` 을 설정해줘야하며 이는 instance 시작 후에 변경이 불가능(?) 하다(이미 돌아가고 있는 인스턴스에는 적용할 수 없다).
* codedeploy-agent 가 각 인스턴스 마다 설치 되어있어야 한다.
* code deploy는 배포 데이터를 s3 또는 github으로 부터 번들링 된 데이터를 가져와서 실행하게 된다.
* 번들링 된 데이터에는 code deploy가 무엇을 배포해야할지 알 수 있게 하는 설정파일이 `root` 에 포함되어야 한다. 파일 이름은 `appspec.yml` 이다.

### appspec.yml

[참고 문서](http://docs.aws.amazon.com/ko_kr/codedeploy/latest/userguide/app-spec-ref.html) 를 보면 단순하고 또 정리가 잘 되어 있으므로 참고하기가 편하다(읽기 귀찮으면 [example](http://docs.aws.amazon.com/ko_kr/codedeploy/latest/userguide/app-spec-ref-example.html) 로 직행).

단순하게 실 사용하는 파일 내용은 다음과 같다.

```yaml
version: 0.0
os: linux
files:
  - source: /
    destination: /var/www/renderer/
```

`appspec.yml` 에서 `files` 섹션을 보면 `source`와 `destination`이 있는데 `source` 는 번들링 파일의 구조를 기준으로 root를 설정한다. 쉽게 말하면 `appspec.yml` 이 있는 위치가 root(`/`) 가 된다.

`destination` 은 각 서버에 복사될 위치를 나타내며 서버안에서의 위치를 생각하면 된다. 

> code deploy는 각각의 서버에서 직접 번들링된 파일을 내려받아 실행되는 구조로 보인다. 이 때문에 인스턴스 생성 때문에 `code deploy` 에 대한 `role` 을 가지고 있어야 한다. 번들링 된 배포 파일의 위치가 `s3` 라면 이에 대한 권한 설정은 덤이다. 

---

대략적인 한 사이클에 대한 설명은 끝(?)났으니 실제로 따라해 보도록 한다. 글은 니즈의 순서대로 작성하고 올바른 순서는 표시를 하도록 하겠다.

---
* iam role 설정
    * ec2 isntance
    * deployment group
* codedeploy-agent 설치
* 배포서버 destination의 권한 확인
---

### IAM 설정

`aws console => IAM => roles` 에서 `Create New Role` 을 선택해서 code deploy 대한 설정을 해야한다. 생성 후에 `Trust Relationships => Edit Trust Relationship` 에서 다음과 같이 설정한다

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": [
          "ec2.amazonaws.com",
          "codedeploy.[:region].amazonaws.com"
        ]
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

:region은 ec2가 존재하는 region을 설정하면된다.

//TODO: role

### [2] EC2 설정

> IAM 설정

[1] 에서 생성한 role을 선택해서 instance를 생성 하도록 한다.

> codedeploy-agent 설치

문서화가 잘 되어 있어 그대로 따라하면 된다. [문서](http://docs.aws.amazon.com/ko_kr/codedeploy/latest/userguide/how-to-run-agent-install.html)

### [3] AWS CodeDeploy

`codedeploy` 설정에 진입하고 `Create New Application` 버튼을 눌러 배포할 `application` 에 대한 설정을 한다.

> Application Name

배포할 어플리케이션의 이름이다.

> Deployment Group Name

같은 설정을 통한 배포로 엮일 그룹 이름을 지정한다.

> Add Instances

인스턴스를 생성하면서 만든 태그를 통해 배포의 타겟이 될 인스턴스 설정을 한다.

> Deployment Configuration

n개의 서버에 한번에 deploy 할 것인지 하나씩 순차적으로 할 것인지에 대한 정책이다. 한대이니까 설정오류가 나지 않도록 `CodeDeployDefault.AllAtOnce`를 설정한다(배포시에 서빙가능한 서버가 없어도 된다).

> Service Role
    //TODO: codedeploy.[:region name].amazonaws.com 에 대한 설정이 되어 있는 role을 필요로 한다.
    [1] 에서 생성한 deployment group 에 대한 role을 선택한다.

### CI_ 설정

CI가 slack에 알리는 것 외에 배포를 트리거 할 수 있도록 `.travis.yml` 에 `codedeploy` 관련 설정을 추가하고 번들링해서 업로드할 경로를 함께 설정한다.

```yml
before_deploy:
[: 여기서 build를 한다]
before_deploy:
- mkdir -p [:업로드할 번들링 된 파일이 존재하는 디렉토리]
- cp appspec.yml [:번들링 할 루트가 되는 디렉토리]
- cd [:번들링 할 루트가 되는 디렉토리] zip -r ../[:업로드할 번들링 된 파일이 존재하는 디렉토리]/[:file key(file name)] .
- cd ..
deploy:
- provider: s3
  access_key_id: [:your access key]
  secret_access_key: [:your secret access key]
  local_dir: [:업로드할 번들링 된 파일이 존재하는 디렉토리]
  skip_cleanup: true
  bucket: [:bucket name]
  on:
    branch: master
- provider: codedeploy
  access_key_id: [: your access key]
  secret_access_key: [:your secret access key]
  region: [:code deploy application이 존재하는 region]
  bucket: [:bucket name]
  key: [:file key(file name)]
  application: [:code deploy에 설정된 application]
  deployment_group: [:code deploy에 설정된 deployment group]
```

> [:번들링 할 루트가 되는 디렉토리]

컴파일 된 소스코드만 배포하면 되니 빌드된 바이너리나 코드가 있는 곳을 지정.

이제 ci가 deploy 전 단계까지 성공한 다음에는 s3에 번들링된 파일을 업로드하게 되고 `code deploy` 를 트리거하게 된다. 잘 되었는지 확인하기 위해서 aws console의 aws codedeploy로 진입하여 대상 application을 선택하면 배포 상태를 확인 할 수 있다. 실패한 경우에는 로그와 함께 어떤 상태에서 실패했는지를 확인 할 수 있다. download 에서 실패하면다면 s3에 대한 접근 권한 문제일 가능성이 크다.
