---
title: AWS API Gateway - Lambda - Travis-CI prototype 작성
tags:
  - aws
  - api gateway
  - lambda
  - ci
  - travis
  - lambda 배포
date: 2016-09-26 00:00:00
---


> `aws serverless architecture` 의 핵심에 `api gateway` 와 `lambda` 가 있다. 두 서비스는 모드 서울 리전에서 사용이 가능하다. 이를 이용하여 프로토타입을 작성해 보자.

## architecture

`node`로 서버를 구성하는 것을 예로 들자면 아래와 같다.

| node application | express-router | logic |
|---|---|---|
| aws | api gateway | lambda |

aws에서 제공하는 아키텍쳐 예시를 보면 이해가 쉽다.

![serverless architecture](http://awsblogskr.s3-ap-northeast-2.amazonaws.com/articles/2016-04-serverless/serverless-amazon-s3.png)

*https://api.example.com* 부분을 통해 api gateway에 접근하면 라우팅을 통해 필요한 lambda함수를 콜하게 된다. 그리고 db 등에 데이터를 연산을 하고 다시 사용자에게 응답을 리턴한다.

이 포스트는 실무에 적용이 가능하도록 아래와 같은 내용을 담고있다.

 * lambda 함수 생성
 * api gateway - lambda 함수 연결(parameter, header 설정)
 * development, production 모드를 분리하기 위한 stage 활용
 * travis ci를 통한 배포 자동화

실제 구현 순서와는 상관없이 설명하기 편하게 상향식 접근 방법으로 작성한다.

## lambda 함수 구현

### 코드 구현 방식

lambda는 다음 3가지 방식으로 코드를 구현 할 수 있다.

  * inline editing
  * **upload zip file**
  * upload from s3

이 포스트는 배포 자동화까지가 목표이므로 2번째 안으로 진행한다. 

### 구현 언어

`lambda`는 `java`, `python` 등 몇가지 언어를 지원하는데 이 포스트는 `nodejs` 4.3 버전을 기준으로 설명한다. nodejs 4.3은 `es2015`(a.k.a es6) 를 부분적으로 지원하는데 훌륭하진 않으며 다행이 `Promise`, `Set` 정도는 지원을 하고 있다.
이외에 코드 작성은 방식은 자유지만 컴파일 또한 비용이며 polyfill등을 이용하게 되는 경우 용량이 늘어나므로 추천하진 않는다.

### lambda 함수 생성

간단하면서도 lambda와 api gateway가 연결되면서 parameter들이 어떻게 전달되는지를 볼 수 있도록 코드를 작성한다. 배포하기 전 빠른 진행을 위해 aws에서 제공하는 `inline editor`를 사용해 바로 소스를 작성하도록 하자.

[aws console](https://console.aws.amazon.com) 에서` lambda`를 선택해서 lambda 페이지에 진입한 후 왼쪽 사이드 바에서 `functions`를 누르고 `create a Lambda function`을 눌러 lambda 함수를 생성하도록 하자.

`Select blueprint` 화면이 나오면서 template 들이 보여지는데 `skip` 버튼을 눌러 다음 스텝으로 간다.

`configure triggers` 화면이 나오는데 우리는 api call을 통한 실행을 할 것이므로 `api gateway`를 선택하고 `next`를 누른다.
![](https://lh3.googleusercontent.com/Y5RrAUzYbGCZF1Ei9TI8G8r3rNkEDVtTJxp3unq9N3-CNRfo0oEeLVlD2bBDqmqOM0hNmD5Z4TO-aV7URlndgAjXAHxW5ADU98mW1W-mvbqVIu8mtvdKS8ujNd0Xsxez_jbosmy3zxWeRlyk9tSz3qpm1sNcY3UqWmEYoIyuDUUmhsHAtuW74XnHU93Vq49XQtL6sdOLjUI6QC2Ts3XMR2v6752ZIaISRPXpmxZRpOB6x6ZXXErJ_poJMDqjoCoZJDmtST2ry3-T9tdnz37S-SvuD03g6EmF2ogILD8amHdycwkQWEVKiSNLBiw2-i_gjhIgov7nKowMCPI0BuJRnxyTC7WGitmCjsdtj7nV_G13gvQVz-4CkdOp3EKTwgEjZxq3zzfbpA-wbhgYSoAdSVtzxpwRA1WMIPvhNk7FAp0Afte0hvTaX807p9qvmJgkTw0ebaLV5Aj_M-COz2Mo7vScyYC2Rauan_O3Iu6KlXSNG-5zzF4u0cFjCWnv38xbscccKYZ3kOoY40gt0WajQ0OHG3IFG0YuAS4DUXn_pItE_b_oPaqNRehKPgK1CkcHZlK_1dlYlHRCJZ0dkkWFMirpJ6KpWJbeNEfimzM6KKRkfjg9jg=w1218-h546-no)

`configuration function` 에서는 함수에 대한 설정을 한다. 아래와 같이 설정한다.

| config | value |
| --- | --- |
| Name | loopbackArgument |
| Description | |
| Runtime | Node.js 4.3 |
| code entry type | Edit code inline) |

```js
exports.handler = (event, context, callback) => {
    callback(null, {
        event: event,
        context: context
    });
};
```

`export`할 함수 이름을 정하고(여기선 `handler`) lambda의 signiture대로 3개의 인자를 받는다. 인자를 간략히 설명하면 다음과 같다.

| argument | description|
| --- | --- |
| event | event call에 대한 정보로 parameter, header등이 이 인자로 매핑된다. |
| context | lambda 함수 자체에 대한 정보 |
| callback | 리턴함수라고 생각하면된다, 첫 번째 인자는 Error 객체, 첫 번째 인자가 null 경우 성공으로 간주되고 2번째 인자가 응답값으로 사용된다. |

이 코드는 단순히 람다 함수의 인자를 그대로 응답하는 `handler`함수를 반환하고 있다.

이 외는 기본 설정을 이용하며 `role`을 정해줘야하는데 이전에 람다를 위한 role을 만들지 않았다면 생성 후 선택하고 `next`를 함수를 생성한다.

## api gateway - lambda 함수 연결(parameter, header 설정)

`api gateway`에서는 라우팅 테이블을 만들때 swagger를 지원한다. 기 구현된 서버가 swagger를 통해 문서화가 되어 있다라면 serverless architecture를 바로 적용할 수 있을 것으로 보인다.
특히 swagger에 [api gateway extensions](http://docs.aws.amazon.com/apigateway/latest/developerguide/api-gateway-swagger-extensions.html)가 존재하는데 이 부분을 swagger에 함께 작성하게 되면 파라메터나 응답값에 대한 정의를 문서 작성 단계에서 끝낼 수 있다.

이제 `api gateway`를 설정할 차례다.

### API 생성

[aws console](https://console.aws.amazon.com) 을 통해 api gateway 서비스에 접속한 후 `APIs` 메뉴에서 `Create API`를 누른다.

New API를 선택하고 `API name`에 `test` 라고 작성한다.
Actions를 눌러 `Create Method`를 선택하고 `post`를 선택한후 check 버튼을 눌러 생성한다.
이러면 생성한 `POST` method에 대해 setup하는 화면이 나오고 이미지와 같이 설정을 하여 위에서 작성한 `lambda` 함수를 연결한다.

![](https://lh3.googleusercontent.com/BF-csXtOhMEE6WspJMUm7eZ7c6cr7_aM0Swxx_DFf0JXhE2c1TwVsB3REoLH30XMkyqQ01kiqZzwNumbqLkbpJ8ZUvj1PlXBK599B7kdjy2w38j9OXxwjtyRWo1rrve2TtxcQfaB5RKJrXmMtRiqiTW4Ojx0gig0NblF-l1sefCvQQuAmHDwDihrN0CYvXgRgPIIkV42kH4CW_tUX2rJtdCisScuSp2iJfc8I6piFwXKIIIWtiUFm7aNRFWds9norKHmpZFg9tJNCW6NdGvA_NsB4phrz-i286cgxUqL_UPvrksJ-_vvRAEm4NKi0cvw3qoRa8cDHQJHe-RAZKgcJgrSuyKkv6j8-Qw3MWVAB_ISlObYFAiEL-kt7C4fJ67zRGFa_BQp9dX1EnJZhSLXJ6jA_9RI63uxxrSuMvc0TBOHCwUtH6R5GvjMp_RpSVdHVrCoPYmKJ-qRPZre7SHnz5n9tfgSUfY97hiFLze-EeqYD8Ydc_dYrzyREgMhe-E8xxOzhfTgqQc-jarA-v2HpTiovD8g5AG_gLEbUhvISQJGRaUZXiCeRW-EZ8phipGYN5h5Yz9A7250P6T_9gFvNSGTb_vJKUeTBEL16jM96GdZzLj3Ww=w783-h400-no)

물론 함수명과 리전은 작업중인 상황에 맞게 넣어야한다.

### API 설정

#### Method Request

설정할 것은 없지만 `header`와 `query string`이 들어오는 것을 확인하기 위해 이미지와 같이 추가해 둔다.
![](https://lh3.googleusercontent.com/vGNzrnq8mtX_glpQje0FTPMxqrXSkLpzEgWdDvfRe3oTCy-5dl40FLX9PhS1-i1tE6ECxszAc6U2jcWU3IvcP5tHLE20pmC4SNWx_uF7_bXlBWipFAENk11BSNIgFkws73UNRw2UycbSAp1q1uP7k69dWvuTGf3w2-w3uLcyUBKOSHukkFyFVBjd84nFHNm8b-sDr2RTE6sOdXQtJNsO0oQ6rOzgxu608HLW-iuS-LIdtMx6nNx92OONE-45P4Q05OlrT5QMNloXw2xr4xr-Ri5tq1EYcBbVm24PrePbUgpOhl9ZqKq_MZq-4pwMoYBWNSer4RCyJCuZxwATNn5ylEexzVLKaAYodFPwSFLbQ9ISL5-sZH0ZRVaAq0a1ZHM94bawcO50hOBjfagTF9Lamdc459-QLfk-TJlmHvg6BWWmyj17FAs8hT8Wn8FEKwBHOzmKj96JQOATff4grr2ZCFhM3H_XxF5JgFYkuPjUEOl91oJB3o14KLat8tIYcEmikC-lB6mafVXELgrYUVNDvqTDId3PFbKdDNdXYKevjk-XuSLgL3k7dticdFtCNhTfKSKpUA6MfX4A7wqgiYuJcK69gylm_vGUg1hoXzhWY7Ny2xo4Hw=w1192-h705-no)

#### Integration Request

여기선 `Method Request`에서 설정한 parameter 들을 받기 위한 설정인데 기본 템플릿을 적용하여 아래 이미지와 같이 설정한다.
![](https://lh3.googleusercontent.com/yrQo0p55cgPUD7xTWGpXznNhTalanKmSUtGMWKBTGRNFTuhd3xBZRtlKc9sxsvtYBQclLVeRMvsxhHMC29ONSJ_aXoIqKKSFxulBV2_EDCAd6QuDlu8z5S2dceF1Qj-Es8SjbAg8uE-S0ttxZZIAGvIISN4vK7Y623GnEKb2RhVKPBQW-JB5FN9qgTtYVmCSwDLny9-QFXg7lRZtOeB82Wgn2SiycGyd-wNTyxe6j4cQV12umkC9czZvn2QAwRX_Xl6mzK6nDtoqvXbebe31hxAUUc2DzVnywpIO0LH33a1-vg2KF2Uau9VMfsk0SlC2B-QkXTUdkQSuVDrXN0VXrAfSSiGT6RCF2XwMm9vbK81JV6IH_jlcwxN2jufquEgjMugBCNa0hPFIs4vyj-747FT_9mp-dAlKYWGUF0Cf3rZpE-B-n4nZdiV175Cz0KLVoIuF26cgR2W0SbK2HBxIY3xMdJBnHmwOmr40ailQXudRvPzHXbJk6bS2eYKpz53kAdOOHLHbqHPYgSIKLDtT3CDF4RKQGc-FSkzUcDM93zqYUnGLvcRu2AESbRqq2D6GIjEP_WUUhsUsOtnljyzIvt-BWYNzU8bgD3jUndCmN0cZz_cAQg=w960-h597-no)

### API Deploy

사용을 위해 Resources 옆 Actions 를 선택하고 `Deploy API`를 선택한다.

![](https://lh3.googleusercontent.com/wFO9ggX6eq_T46j0iEYnzGS1yMy74cNWPY9mGU9GhijI2hIxgfut6emscJ607db5BlGgZNPCbcQgBPCKUvYfKtCm3wpZx1xK0mE5-4-i7ABIpDH7GmSAv5o67gPalQTXsFwSvrW0havFns21c3H-PLd-rA80gm-typkeWRxbjKWgM54-gEjAiC2FVEGNdYaso9SQWsnujEUgschszxMhhRaJzC05zD8-L2Nd6ZW4b94ii_xzYjoz4bNrqQiGvMuDExiatSj8AcTg_KsGhm-FgctyyZX0zP8smvh89_sSyLE37y3baz5-9tq9Te42Mkfhj0pIO6zW-lP0ONVYyQhVjtBeZcW4RPzwsfXwunBE_t7vmPGC7yNtRPHChqtBP37XHjNkMNIR4ADhPr_Uyu_yhvk76Gzxw0JXuJATFg9DeT-Ct88_-nLG-9jzb4j1_RgZW0LAgL-BiUObujoweBDBcbmhAlpHP63DZZQ5FeBSyxAVx1u_0vKO8O7qENNDOmOFpiWlrzkjf9YS5OhkQsplCnSUQnjIpVm8K5aQCix8DcvxXhxfpkv_0ZLi5ym6GWLChP8T3-oru4ylc4lgSZwm7K9QUKvyzHbwWakSZGsrguDe2EcvfQ=w599-h412-no)

이미지와 같이 `[New Stage]` 로 `prod`를 입력 후 deploy 버튼을 누르면 `Stages` 화면으로 진입하면서 `endpoint`가 될 url이 `Invoke URL` 이라는 이름으로 보인다.
이제 이 url을 통해 api call 이 가능하며 당장 `postman`이나 `curl`등을 통해 확인이 가능하다.

1차적으로 `api gateway`와 `lambda`함수를 연결해서 실행을 확인해 볼 수 있는 상태가 되었다.

## development, production 모드를 분리하기 위한 stage 활용

작성한 코드를 실행해보면 아래와 같은 형태의 `json`이 응답으로 전달됨을 확인 할 수 있다.

```json
{
  "event": {
    "body-json": {
        ...
    },
    "params": {
      "path": {},
      "querystring": {
        "deptno-param": "deptno test"
      },
      "header": {
        "Authorization": "deptno test"
      }
    },
    "stage-variables": {},
    "context": {,
      "http-method": "POST",
      "stage": "prod",
      ...
    }
  },
  "context": {
    "memoryLimitInMB": "128",
    ...
  }
}
```

`Integration Request`에서 설정한 기본 템플릿을 통해 전달된 event 오브젝트와 context 객체의 내용이 구현한 함수의 기능대로 동작한다면 위와 같은 결과 값을 받을 수 있으며, 코드에 보여지는 부분들은 자주 참조 되는 영역만을 코드에 표시했다.

event 객체의 구조는 앞으로 사용할 lambda 함수에서 event 객체를 사용하는 `reference`가 된다.
보면 `event` 에도 `context`가 존재하는데 여기에 `stage` property가 있고 우리가 deploy했던 stage를 참조할 수 있는데 이를 통해 (프로덕션 코드와 개발코드가 분리되는데 맞지만) lambda 함수에서 개발 버전 또는 배포 버전의 일에 대해 분기를 할 수 있게 된다.

## travis ci를 통한 배포 자동화

code로 대신한다.

```yaml
deploy:
- provider: lambda
  function_name: loopbackArgument
  role: arn:aws:iam::...
  handler_name: handler
  region: ap-northeast-2
  access_key_id: ...
  secret_access_key:
    secure: ...
  runtime: nodejs4.3
  timeout: 30
  ```

조금 부연하자면

  * `role`은 lambda함수를 생성할 때 줬던 role(lambda 함수 실행을 위한) 이 들어가야된다.
  * 추가적으로 메모리 사용량 설정등도 가능하다.
  * s3를 통해 업로드하는 방식과 달리 폴더를 압축해서 zip파일을 업로드하게 되는데 이때 용량제한이 걸린다. 압축한 파일을 용량이 `50MB`를 넘어가면 deploy에 실패하게 되니 주의하자.
    * 이 때는 s3에 upload 하고 추가로 스크립팅을 통해 deploy 되야하므로 번거로워진다.

## next

> 실무 적용 및 효율 적인 업무를 위한 추가 적인 작업들은 아래와 같다.

todo

  * custom domain을 통한 배포
    * ssl 적용
  * api gateway
    * cors 적용
  * swagger를 통한 문서화 및 배포
  * slack을 통한 알림 처리