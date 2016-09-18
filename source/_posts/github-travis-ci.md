---
title: github - travis ci - slack
layout: post
category: [dev, ci]
date: 2016-08-15
tags: [ci, travis ci, circle ci, slack, github, devops, github, notification]
---


코드를 커밋하고 ci가 검증하고 이에 대한 결과를 슬랙으로 받고자 한다.
circle ci, travis ci 등 ci cloud service가 많은데 open source 를 지원해주는 `travis-ci`를 기준으로 해서 작성한다. circle ci도 개인 레포지터리 하나에 대해선 지원을 해준다.

## travis-ci, github 연결

순서대로 진행을 하면 github과 ci를 연결해야한다. 일단 github에서 travis-ci를 사용하기 위해선 [travis-ci.org](https://travis-ci.org/)에 계정을 만들고 github계정과 싱크를 한다. 그러면 개인 레포지터리가 보이게 되고 여기서 활성화를 하면 된다.

![webhooks&services](https://lh3.googleusercontent.com/bmMSu2O9kqUhIogjUduw6f9aaBVA9-JlWt3YLcQhZD8E2FEjw6bw1ez5uxx8O82ZCHpyiG8bjRYushydZVmCMH7SY4KgQKWOBzLe_wfhnPjSzDtGT1DhShhHV8i7ayPsJ_yr5t_Qoh0VA-eC4cK7aWZVe4BrVAXgm4go9IOC2uKns-02b2jeF5y7jB7MYMrA4vA6Tlzc8pRZbiB1EqZCD4_knU3Vm7K7_XSL_tUpNefrqTtCQyvgrMNc3pIfKl12BKgLZacLIVrUmux83RzfzX9JIIiOZHI_qmhTsp8qDVCr0_B3LqkkxLjmErrrEoDF3mCzH6y0A275-Khd1vtO2Hf23J1Nc74mSgkoK_WHxoEZgQXJOIgztkPNlgCWU8v5ci60l1QhjpywB0Al0I3oIT1wt7cpuWG6RPAB5FlEabOqjS_9w2JT1HsO5Ks6teVMR_BfyIZJgtGthuWDPeMxfMZMtM3WGGqD7ov9QdTyFHrIrzbYZQJCugaBaCteGy_q4CMp3p6Z7FepPWR2V3l7ECj83wiNpCwVAuro63jDxPHXQSLmQAsAprH6BYjpHkiJ67FuvejzchghetZhroiC7YCyirshftab=w1366-h558-no)

활성화를 하면 다음과 같이 `github => repo => Settings => Webhooks & services` 에 `Travis CI` 가 들어 온 것을 확인 할 수 있다.

소스코드에서는 travis에서 읽어갈 설정 파일을 만든다. travis-ci는 cli를 제공하므로 이를 설치하도록 한다. `ruby` 종속성이 있으므로 없으면 이를 먼저 설치한다.

```sh
gem install travis
```

이제 코드가 있는 디렉토리로 가서 `.travis.yml` 파일을 생성한다.


```sh
travis init
```

이때 github에서 clone된 폴더로 작업을 하고 있었다면 자동으로 레포 정보를 가져오고 언어는 현재 사용하는 언어를 해주면 알아서 생성해준다.

`npm`을 사용한 프로젝트라면 ci가 한번 돌 때마다 `npm install`에서 소요되는 시간이 상당하므로 캐쉬 설정을 해준다. `install:`에서 `npm install`을 한 경우에만 캐싱되는 것으로 보인다(또는 다른 곳에서 npm install을 하지 않은 경우)

```yaml
cache:
  directories:
  - node_modules
```

## slack 연결

slack에 계정이 없다면 가입을 해서 생성을 하고 [travis-ci를 설정하도록 한다](https://deptno.slack.com/apps/A0F81FP4N-travis-ci) 여기서 `install`을 눌러 설치를 하고 `Post to Channel`에서 원하는 채널이나 본인(`Privately to @deptno (you)`)에게 알려주도록 설정한다. `token` 은 노출되면 안되므로 `Encrypting your credentials`에 써있는 코드를 복사해서 위에서 생성한 `.travis.yml` 이 있는 디렉토리에서 실행한다. 그러면 `.travis.yml`에 slack에 알림 설정이 된다.

이제 코드를 푸시해서 `master` 에 들어오게 되면 알림이 뜨게된다.

![slack-notification](https://lh3.googleusercontent.com/gNwGS-5IXSmlS4orWWp8RJozLxZhAYkNYN5EyINcr_5lqYyNQWNkwfXmhkaAHHIXYZog3P_hFSeOdkl7hiGGl1Wy0SP5YCxtSMFhwl_y7U4XgNb7bl1iP7DeBcsbBRlCKk3sd1tjBYvewduDjd4janAVnh51ZFNLgFe8d2j5t7022gjRbgDcMg4YIngKnXuCOSXQE_kPYmKACR78tZfPtLhAeXRADMDf7fqb0f35sGMTR5U6TNzTNTM4Fs4Z9IjvHQVEJJ1j_0IeWSimzPk7KICGLN7jyBPAYtJu9DxZeSUA-c28zF7fCh0n28tKDOG0m-RHShRjg22oVqQY1wvl-ncBnxcdGGldFOoqgTBnAxR5WAOl5RFWkCxDGgUNz-CCJRgmq3vzbljhfzxSiIzQ5B0lGFQrumB7UFFcriq893Xse_vVpFpo_l3_LVLi8GmyeXmgZ3CGAuIfM3KLiusHj029W5wAAwQOWeRJwqTBHvrd5vo6nc6zJTVGORXJVGtcZym1Wc5A_EAg8uO34QMfO6sO8MLAqtN_A2CqpDvLIBxKba1v8LJ1poW140-xo7Qv4GHj41m4dJh5pi73iHX16lmKXddBNzgF=w1050-h456-no)

배포가 남았는데 배포는 서버 환경에 의해 좌우되므로 나눠서 기약없는 추후에 다루기로 한다.
