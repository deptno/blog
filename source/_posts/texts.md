[Texts][0] 구현
===

![][3]

> 기획자 디자이너, 퍼블리셔와의 협업시 문구 변경으로 인한 피로를 덜기 위해 ID기반의 문구 관리를 앱

## 동기
* 부동산 계열에서 일을 하다보니 너무많은 term이 존재한다.
* 사소한 문구 변경이 데일리로 일어난다.
* 퍼블리셔 쪽에서도 html에 대한 이력관리를 원한다.
* 문구 변경과 markup의 변경이 구분되지 않는다.
* react로 퍼블리싱하지 않는 이상 태그를 그대로 사용하지 못하므로 daily diff이슈가 존재한다.
* 여러 곳에서 동일하게 쓰이는 문구 변경이 일어난 경우 diff에 혼란이 있다.

## 개발
> 일단 퍼블리셔 쪽에서 `ID`를 사용하용하여 퍼블리싱 할 수 있게 끔 하는 것을 최우선으로 했다.

* 독립적으로 사용할 수 있도록 [serverless][1]를 통해 DB까지 한번에 deploy한다.
* backend 엔드포인트만 사용자 별로 바꾸면 되니까 fe는 직접 만들어서 배포했다. **[Texts][0]**

## 운영
### 기획(스트링 관리 주체)
* <https://texts.surge.sh>?endpoint=https://[BACKEND_ENDPOINT] 접속
* 스트링 마음껏 만들고 잘 됬으면 deploy한다.

### 퍼블리셔
* 퍼블리셔 쪽에서는 `<div data-text-id="TEXT_ID"/>`와 같은 형식으로 퍼블리싱을 할 수 있게한다.
* 퍼블리셔는 눈으로 이를 확인할 수 있어야하므로 이를 치환해주는 태그를 서비스해준다. **[Texts Translator][2]**
* 퍼블리셔가 동적인 동작을 위해 `jQuery`태그를 심듯이 심어주면 동적으로 확인이 가능해준다.

### 개발
* 개발 버전에서는 퍼블리셔와 같이 태그를 심어서 스트링 관리주체가 변경하는 것을 즉각적으로 확인할 수 있게끔 한다. 
* 프로덕션 릴리즈시에 아래 형식으로 저장해서 CI빌드시에 넣으면된다.

```sh
$ http post https://[END_POINT]/json > language_ko.json
```

## todos
* 퍼블리셔쪽에 구문 변경으로 인한 스트레스에서 벗어나고 싶지않냐고 설득한다.
* 기획자분이 스트링을 관리할 수 있도록 제안하고 설득한다.

## 히스토리
* 표준을 이용할 수 있게 끔 `Intl`을 붙이려고 디자이너쪽에서 태그를 어떻게 사용하게 해야할지 감이 오지 않아 뒤로 미뤘다.

## 장점
* 퍼블에서도 모델을 분리한다.
* 모델, 디자인, 컨트롤의 물리적인 관리주체(부서)가 명확해 진다.

## 단점
* 서비스가 아니라 aws 설치 형태라 결국 나만의...

[0]: https://texts.surge.sh
[1]: https://www.serverless.com
[2]: https://texts-translator.surge.sh "texts"
[3]: https://lh3.googleusercontent.com/YYl9N9drJIG7mVbA20BcYJT9RLrDSNaAnNd7sStIiVHF6PQ0ERfY7WbFpKzavJQ7vuIH4bNRJDsmVCWweGh3WPzj0flIX0mJF-yK7mSdOqfXJZAYEqgjC9mcr-9d9r_iPzVFJ9rCO_mQfHUzWtDf66lBsBp9AUqpOUiT6uEXvuc82T8ZtvnhdrEovnuOyyqOawZr5v9mkGMP8uiGxUprAMbKBYlppkXY6dCsBbH06QRbKDMnXBTQ0TFRA8ZWmtE0IOaU7Ddld2xM9SG5dMA8L_U40dYmO4cPOH4YtRBlPyxQk-joWqX3azGKLISuwSE8NlztTICtcGKrKBFMHaEYvVAd26AnmLH5LgTnU8hVIOEnSCeYD-bf2v8wCqKWKEMld2l6L3Ne8tJvoCOTXC3k47hgkXuJcUopDbrBBsCkVl87j1GdOK47HmtEnNiQF91NsgOHwXSPvSRWEjq1VhGVDiLeMIfc39y_yVRPxLbDHgn4_zYDjWUbyhaWuSX4Izyjvi_R5jImdyCsCh_sMsw7HaVW-vAW7FotbHhtX96LUrjeojM9AfdMFQIfnSingZs5avpo262kGMr_FroiAMbcsIFVFKxca6kBs8y23Cn-L-bOA4DF6Sy02A=w1139-h410-no "texts image"