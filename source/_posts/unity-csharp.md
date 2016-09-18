---
title: "LEARN: Unity & C#"
layout: post
date: "2015-07-22"
category: summary
tags: [유니티와 C#으로 배우는 게임 개발 교과서, unity, unity3d, 게임 개발, C#, 씨샵]
---

# 교재

>유니티와 C#으로 배우는 게임 개발 교과서

## 002. 디지털 프로토타입 제작

### 18장: HELLO WORLD: 첫 번째 프로그램

Rigidbox의 mass는 질량으로 단위는 1KG가 된다.
Rigidbox 속성을 추가해야지만 물리각 작동된다.
prefab은 instance화가 가능한 GameObject다.
code에서 Instantiate를 통해 연결된 prefab을 생성할 수 있다.
프로젝트 파네에 instance화가 가능한 prefab은 항상 큐브 모양의 아이콘이 왼쪽에 표시된다.

### 19장: 변수와 컴포넌트

float은 double보다 정확도가 떨어지지만 속도가 빠르기 때문에 모든 기본 제공 유니티 함수는 double이 아닌 flaot형을 사용한다.
float은 1이하 수천 이상에서 심각하게 정확도가 떨어지기 때문에 게임의 요소는 1이상 수천 이하에 유지되는 것이 효율적이다.
char는 '' string은 ""으로 감싸져야지만 된다.

#### 주요 유니티 변수 타입

변수 | 설명
-|-
Vector3 | 3개의 flaot 변수의 집합
Color | 투명 정보가 포함된 색
Quaternion | 회전 정보
Mathf | 수학 함수 라이브러리
Screen | 디스플레이에 대한 정보
SystemInfo | 장치에 대한 정보
GameObject | 씬에 포함된 모든 게임오브젝트 형식

#### 유니티 게임오브젝트와컴포넌트

컴포넌트 | 설명
-|-
트랜스폼 | 위치, 회전 배율
MeshFilter | 화면에 보이는 모델
Renderer | 게임오브젝트 표시
Collinder | 게임오브젝트의 물리적 존재
Rigidbody | 물리 시뮬레이션
스크립트 | 여러분이 작성하는 C# 스크립트

### 20장: 부울 연산과 조건문

<pre>
&&, || 는 단축 연산자로써 첫 번째 구문에서 답이 나온경우 두번째 구문을 실행하지 않는다.
&, | 는 첫 번째 구문과 상관없이 두번째 구문까지 평가한다.
&는 | 보다 우선순위가 높다.
</pre>

switch문은 리터럴의 동등 비교만 허용

### 21장: 루프

### 22장: 리스트와 배열

### 23장: 함수와 매개 변수
**params** keyword를 통해 동일한 형식의 매개 변수를 수에 제한 없이 받도록 할 수 있다.

``` csharp
int Add(params int[] ints) {
  int sum = 0;
  foreach (int i in ints) {
    sum += i;
  }
  return (sum);
}
```


### 24장: 디버깅

* 클래스 명과 파일 명은 동일해야 한다.
* 재생중이거나 일시정지 상태에서의 변경사항은 적용되지 않는다.

### 25장: 클래스

set 접근자에는 암시적 변수 `value`가 존재한다.
`[SerializeField]` 특성은 private 변수를 인스펙터에서 확인하고 편집할 수 있게 해준다.

``` csharp
int Add(params int[] ints) {
[SerializeField]
private int _num = 0;
```

### 26장: 객체 지향적 사고
### 27장: 애자일 사고방식

> 너무 귀찮아서 더 이상 읽을 수가 없다.

## 003. 게임 프로토타입 예제와 실습

### 28장: 프로토타입 1: 사과 받기

코드보다 인스펙터에서의 public 변수 수정이 우선하게 된다.
`Destroy(this);`는 Script 컴포넌트 자체를 삭제하게 되며 인스턴스 삭제를 위해서는 `Destroy(this.gameObject);`를 사용한다.

``` csharp
// MonoBehaviour Class
Awake() // Start() 보다 먼저 호출된다. 데이타 세팅등에 사용된다.
Start() // 시작할 때 한번 호출 된다.
Update() // 프레임 마다 호출 된다.

// 외
Camera.main // main camera 접근 전역 변수
PlayerPref // javascript localStorage 역할을 한다.
Application.LoadLevel("Scene Name"); // Scene을 로드한다.
```

### 29장: 프로토타입 1: 미션 데몰리션

magnitude는 Vector의 length다
``` csharp
transform.Find("Object Name"); // 하위 오브젝트를 찾을때
<GameObject>gameObject.SetActive(false); // GameObject가 비활성화 되면 이벤트, 렌더링 함수가 호출 되지 않는다.
```
