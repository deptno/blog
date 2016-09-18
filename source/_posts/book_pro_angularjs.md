---
layout: post
title: 프로 AngularJS
category: [summary]
date: 2016-02-01
---

> 이 문서는 AngluarJS를 빠르게 읽히기 위해 예제코드를 눈으로만 읽으며 정리한 개인 노트다.

<!-- toc -->

## 환경 셋업

### 서버

[deployd](http://deployd.com)를 사용한다.

``` bash
$ dpd create sportsStore
$ dpd -p 5500 sportsstore\app.dpd dashborad
```
---

## Chapter 7: 스포츠 상점: 내비게이션 및 결제

### directive

* ng-show
* ng-hide
* ng-include[src]
* ng-view

### filter

* orderBy
* unique

### Provider

* routeProvider: ngRoute
  * ng-view에 들어가게되는 라우팅 테이블을 관리

프로바이더 명명법은 **서비스명 + Provider**

## Chatper 8: 스포츠 상점: 주문 및 관리자 기능

### directive

* ng-model
* form[novalidate]: 브라우저의 검증 기능을 사용하지 않는다.
  * input[required]: 값이 있어야한다
* button[ng-disabled]
* ng-class="{'classname': item === true }"
* ng-repaet
  * $index

#### 폼 유효성

``` html
<form name='myForm'>
  <input ng-model='object.model' require />
myForm.$invalid
object.model.$error
```


---

> 2016-01-16 추가

# AngularJS 활용

## Chapter 9: AngularJS 앱 해부

### 팩터리 함수와 작업자 함수

컴포넌트를 설정하고 싶을 때 팩터리 함수를 호출하고, 컴포넌트를 적용하는 시점에 작업자 함수가 호출된다.

``` js
app.directive("directiveName", function 팩터리함수() {
  return function 작업자함수(scope, element, attrs) {};
});
```

### 컴포넌트 별 작업자 함수 인자

``` js
// directive
return function(scope, element, attrs) {};
// filter
return function(input) {
  return 'modified input data';
};
```

정의된 필터도 팩터리 함수에서 **$filter** 를 인젝션하여 사용할 수 있다.

### 서비스

> 서비스는 **싱글턴** 이다.

서비스를 정의 하는 방법에는 세 가지 메서드가 존재한다.

* service
* factory
* provider

### 값 정의

> 상수 정의를 위해 constant가 사용되는데 아직은 차이를 모른다. 오버 스펙느낌인듯...

Module.value('valueName', 'value') 를 통해 정의된다.

### 모듈의 의존성

``` js
angular.module('testApp', ['testApp.filter', 'testApp.directive']);
```

위와 같이 testApp 모듈을 정의 하고 의존성으로 'testApp.filter', 'testApp.directive' 모듈을 선언하게 되면 **'testApp.filter'**, **'testApp.directive'** 모듈간 상호 참조가 가능하게 되는 것으로 보인다.(서로간의 정의에서 의존성 주입이 가능하다는 말)

모듈의 의존성을 선언하게 되면

### config, run 메서드

#### Module.config 메서드

현재 모듈이 **로드** 될 대 호출된다.

#### Module.run 메서드

모든 모듈이 로드될 때 해출된다.

말이 애매한데 run은 모든 의존성이 해결된 뒤에 호출되는 것으로 보인다. 타 모듈에 대한 의존성 주입이 해결된 후에 호출되며 config는 그 이전에 호출되는 차이가 있는 것으로 보인다.

### 상수 정의

``` js
Module.constant('name', 'value');
```

값 정의와는 달리 Module.config에서 주입이 가능하다.

## Chapter 10: 바인딩 및 템플릿 디렉티브 활용

### 디렉티브 적용 방법

``` html
//어트리뷰트
<span ng-bind="todos.length"></span>
//클래스
<span class="ng-bind: todos.length"></span>
```

### 데이터 바인딩 디렉티브

* ng-bind
* ng-bind-html
* ng-bind-template
* ng-model
* ng-non-bindable

### 템플릿 디렉티브

* ng-clock
* ng-include
* ng-repeat
* ng-repeat-start
* ng-repeat-end
* ng-switch

> ng-repeat

{% raw %}
{{ item in array }}, {{ (key, value) in object }} 등의 방식으로 쓰일 수 있으며 내장변수는 아래와 같다.
{% endraw %}

* $index
* $first
* $middle
* last
* $even
* $odd

> ng-repeat-start

엘리먼트 하나를 기준으로 반복하는 것이 아니라 여러 형제 엘리먼트(Sibling)가 있어 이를 함께 반복해야 하는 경우라면 사용한다.
**ng-repeat-end** 를 만날때까지 반복된다.

> ng-include

외부 파일을 로드 할 수 있게 한다. 몇 가지 어트리뷰트를 설정해서 사용한다.

* src
* onload
* autoscroll

> ng-switch

컨텐츠를 상황에 따라 전환할 때 사용한다.

* on

어트리뷰트를 통해 변수를 설정하고(object타입만 가능 할 수 있다.)

``` html
<ng-switch on='object.data'>
<div ng-switch-when='value1'></div>
<div ng-switch-when='value2'></div>
<div ng-switch-default></div>
```

를 통해 변수값이 when과 맞으면 그 뷰를 보여주도록 설정한다.

> ng-clock

앵귤라가 처리하는 동안 인라인 템플릿이 그대로 노출되는 시간이 아주 짧게 존재하는데 이 시간동안 css를 통해 보이지 않도록 처리해주는 어트리뷰트형 디렉티브다.

---

> 2016-01-20 추가

## Chapter 11: 엘리먼트 및 이벤트 디렉티브 활용

### 엘리먼트 디렉티브

* ng-if
* ng-class
* ng-class-even
* ng-class-odd
* ng-hide
* ng-show
* ng-style

> ng-if

ng-hide와는 다르게 실제로 DOM에서 엘리먼트를 제거한다.
ng-repeat과 함께 사용할 수 없다

### 이벤트 디렉티브

* ng-blur
* ng-change
* ng-click
* ng-copy
* ng-cut
* ng-paste
* ng-dblclick
* ng-focus
* ng-keydown
* ng-keyup
* ng-mousedown
* ng-mouseenter
* ng-mouseleave
* ng-mousemove
* ng-mouseover
* ng-mouseup
* ng-submit

#### 내장변수

* $event

### 불리언 어트리뷰트 디렉티브

* ng-checked
* ng-disabled
* ng-open
* ng-readonly
* ng-selected

### 기타 어트리뷰트

* ng-href
* ng-src
* ng-srcset

---

> 2016-01-21 추가

## Chapter 12: 폼 활용

> form tag

**from** 태그는 유효성 검증을 필요로 요구된다.

어트리뷰트

* novalidate
* ng-submit

유효성 검증 변수

* $pristine
* $dirty
* $valid
* $invalid
* $error

유효성 검증 변수는 form 엘리먼트에 지정한 name 어트리뷰트의 값을 통해 접근이 가능하다
상호작용 태그에 클래스가 자동으로 적용된다.

클래스 명

* ng-pristine
* ng-dirty
* ng-valid
* ng-invalid

ng-invalid-email 과 같은 클래스 언급이 있는데 이 부분도 확인해봐야한다.


> input

type에 매핑되는 어트리뷰트 값

* checkbox
* email
* number
* radio
* text
* url

AngularJS에서 제공되는 어트리뷰트 디렉티브(**input**, **textarea**)

* ng-model
* ng-change
* ng-minlength
* ng-maxlength
* ng-pattern
* ng-required

input 엘리먼트의 타입이 email, number, url일 경우 내부적으로 ng-pattern이 적용되므로 이 둘은 함께 사용하지 말아야한다.

>> type이 checkbox 인 경우 사용가능한 어트리뷰터 디렉티브

* ng-model
* ng-change
* ng-true-value
* ng-false-value

ng-true-value와 ng-false-value는 체크 박스의 선택에 따라 ng-model에 들어갈 값을 결정한다.

>  select

* ng-options

ng-options="'item.action for item in items'" 와 같은 형태로 사용되며 **item.action** 부분이 라벨이 된다.
ng-options="'item.id as item.action for item in items'" 와 같은 형태로 사용하면 **item.id** 부분이 라벨이 된다.
ng-options="'item.action group by item.place for item in items'" 와 같은 형태로 사용하면 item.place로 그룹 정렬이 된다.
**as** 와 **group by** 는 병행이 가능하다.

## Chapter 13: 컨트롤러 및 스코프 활용

> ng-controller

디렉티브를 통해 지정되며

``` js
Module.controller('controllerName', function($scope) {
  $scope.fx = function() { alert(); };
});
```

와 같이 사용된다. **$scope** 아래로 달리는 함수는 뷰(DOM)에서 디렉티브를 통해 호출이 가능하다. **ng-controller** 디렉티브에이 의해서 컨트롤의 팩터리함수가 호출되고 인스턴스화된다.

컨트롤러에서 사용되는 내장 서비스

* $rootScropt
* $scope

> $scope

구현상 $scope는 서비스는 아니며 $rootScope에서 제공하는 하나의 객체다

> $rootScope

내장 서비스로 $scope 들과 연결되어 있어 $scope 간에 통신을 가능하게 한다.

이벤트 메서드

* $broadcast(name, args)
* $emit(name, args)
* $on(name, handler)

$broadcast는 아래로 $emit은 위로 이벤트를 전달하며 $on은 받는 역할을 한다.

controller는 돔 구조와 동일하게 부무, 자식 관계가 정립되며 이 순수에 의해 그대로 상속되며 오버라이딩이 가능하다.

### 스코프리스 컨트롤러

``` js
Module.controller('scopelessCtrl', function() {
  this.fx = function() { alert(); };
});
```

``` html
<div ng-controller="scopelessCtrl as ctrl">
```

### 명시적 스코프 업데이트

* $apply(expression)
* $watch(expression, handler)
* $watCollection(object, handler)

AngularJS 외부에서의 업데이트를 통해 스코프가 업데이트 되지 않을 경우 명시적으로 업데이트 할 수 있다.

``` js
angluar.element('#selector').scope().$apply('variableInScope()');
```

## Chapter 14: 필터 활용

필터는 데이터가 뷰에 표시되기전에 거치게 된며 파이프를 사용해 다중으로 적용 가능하다.

### 단일 값 내장 필터

* currency
* date
* json
* number
* uppercase
* lowercase

> currency

``` html
{% raw %}
{{ value | currency }}
{{ value | currency:"£" | number:0 }}
{{ value | date:"dd MMM yy" }}
{{ value | date:"shortDate" }}
{{ value | uppercase }}
{{ value | lowercase }}
{{ object | json }}
{% endraw %}
```

### 컬렉션 필터링

> limitTo

음수를 지정할수도 잇는데 이 경우에는 뒤부터 정렬된다.

``` html
<div ng-repeat="p in products | limitTo:'5'">
```

> filter

filter가 필터명이다. 오프젝트에서 키 밸류가 일치하는 값으로 필터링한다.

``` js
$scope.filterFunction = function(item) {
  return item.category === 'Fish';
}
```

``` html
<div ng-repeat="p in products | filter: { key: 'value' }">
<div ng-repeat="p in products | filter: filterFunction">
```

### 항목 정렬

> orderBy

``` js
$scope.myCustomSorter = function(item) {
  return item.expiry < 5 ? 0 : item.price;
}
```

``` html
<div ng-repeat="p in products | orderBy:'price' }">
<div ng-repeat="p in products | orderBy:'-price' }">
<div ng-repeat="p in products | orderBy:myCustomSorter }">
<div ng-repeat="p in products | orderBy:[myCustomSorter, '-price'] }">
```

정렬값이 같을 경우 배열을 이용해 두번째 소트옵션을 활용해 정렬할 수 있다.

### 필터 체인

파이프를 이용해 순차적, 연속적으로 필터를 적용한다.

``` html
<div ng-repeat="p in products | orderBy:[myCustomSorter, '-price'] | limitTo: 5 }">
```

### 커스텀 필터 구현

첫번째 인자는 이름 두번째 인자는 팩터리함수로 작업자 함수를 리턴한다.

``` js
Module.filter('filterName', function() {
  return function(input, argument) {
    return 'output';
  };
});
```

필터를 정의하면서 기존의 필터를 가져와 사용이 가능하다.

``` js
Module.filter('filterName2', function($filter) {
  return function(input, argument) {
    var firstFilter = $filter('filterName')(input, argument);
    return $filter("limitTo")(firstFilter, 2);
  };
});
```

> 2016-01-22 추가

## Chapter 15: 커스텀 디렉티브 구현

Module.directive를 통해서 선언 가능하며 카멜 케이스로 선언하고 DOM에서 사용할 때는 **-** 로 구분하고 소문자로 쓰게 된다.
디렉티브의 링크 함수중 element인자는 jqLite 객체다.

``` js
Module.directive('directiveName', function() {
  return function(scope, element, attrs) {
    var value = attrs['directiveName']; // 'test'
  };
})
```

``` html
<div directive-name='test'></div>
<div data-directive-name='test'></div>
```

### 표현식 평가

디렉티브의 사용 유저가 필터등을 사용할때는 디렉티브내에서 표현식을 평가해야한다.

``` html
<div directive-name='price | currency'></div>
```

``` js
Module.directive('directiveName', function() {
  return function(scope, element, attrs) {
    var expression = scope.$eval(attrs['directiveName']);
  };
})
```

### 데이터 변화 처리

링크 함수 내에서 와처류의 함수를 통해 변화를 업데이트하도록 한다.

### jqLite 활용

jQuery와 유사하니 대부분 패스하고 이벤트 처리등 몇가지 메서드만 정리한다.

* on(events, handler)
* off(events, handler)
* triggerHandler(event)
* data(key, value)
* data(key)
* removeData(key)

> jqLite를 통한 AngularJS 기능 접근

* controller()
* controller(name)
* injector()
* isolatedScope()
* scope()
* inheritedData(key)

### jqLite -> jQuery

jqLite로 불편하면 angularjs 파일 로딩 전 jQuery를 로딩하면 대체됨.

---

> 2016-01-23 추가

## Chapter 16: 고급 디렉티브 구현

커스텀 디렉티브를 커스터마이징하기 위해 팩터리 함수(링크 함수)를 반환하는 대신 정의 객체를 반환해야한다. 링크 함수를 반환할 경우는 어튜리뷰트 방식의 적용만 가능한 디렉티브가 된다.

> 정의 객체 속성

* comiple
* controller
* link
* replace
* require
* restrict
* scope
* template
* templateUrl
* transclude

> restrict

``` js
return {
  restrict: "EACM"
}
```

* E: 엘리먼트 형태의 적용
* A: 어트리뷰트 형태의 적용
* C: 클래스 형태의 적용
* M: 주석 형태의 적용

``` html
<unordered-list list-source="products" list-property="price | currency" />
<div unordered-list="products" list-property="price | currency" />
<div class="unordered-list: products, price | currency" />
<!-* directive: unordered-list products -->
```

CM은 거의 사용되지 않는다. M의 경우 링크 함수에서 nodeName이 '#comment'인 것을 찾아 부모 엘리먼트에 생성하고자 하는 엘리먼트를 붙이는 방식으로 진행된다.

> template

링크에서 엘리먼트를 생성하지 않고 template 프로퍼티에 엘리먼트의 **innerHTML** 에 대입하는 것과 마찬가지로 HTML코드를 대입할 수 있고 함수를 지정할 수도 있다.

``` js
{% raw %}
return {
  template: "<ul><li ng-repeat='item in data'>{{item.price}}</ul>"
}
// or
return {
  template: function() {
    return angular.element(document.querySelector("#listTemplate")).html();
  }
}
{% endraw %}
```

> templateUrl

``` js
return {
  templateUrl: "itemTemplate.html"
}
// or
return {
  templateUrl: function(elem, attrs) {
    return attrs["template"] == "table" ? "a.html" : "b.html";
  }
}
```

일전에 책을 15, 16, 17장(커스텀 디렉티브 관련)만 읽고 <https://github.com/deptno/autocomplete_hangul> 를 연습삼아 만들어봤는데 templateUrl을 사용하게 되면 배포시에는 경로가 index.html(메인 파일)을 기준으로 정렬되면서 링크가 깨지고 만다. 빌드 툴 등을 이용해서 **template** 로 최종 배포시에는 넣는 것이 맞아보인다.

> replace

``` js
return {
  replace: true
}
```

생성되는 html은 아래와 같이 달라진다.

``` html
// replace undefined
<div user-directive>
  <table></table>
</div>
// replace true
<table user-directive></table>
```

ng-repeat등과 유용하게 쓸 수 있다고 책은 기술하고있다.

### 디렉티브 스코프 관리

기본적으로 상위 컨트롤러의 스코프를 상속한다.

> 디렉티브 스코프 생성

``` js
return {
  scope: true
}
```

``` html
<div ng-controller='userCtrl'>
  <div user-directive></div>
</div>
```

위와 같은 경우의 스코프는 **디렉티브 스코프 < 컨트롤러 스코프 < 루트 스코프** 순이 된다.

> 고립 스코프 생성

``` js
return {
  scope: {
    local: "@onewaybinding",
    local2: "=twowaybinding",
    localFn: "&userFunction"
  }
}
```

``` html
{% raw %}
<div user-directive onewaybinding="{{data.name}}" twowaybinding="data.other" userFunction="functionNameInScope(userArgument)">
  {{ localFn({ userArgument: local }) }}
</div>
{% endraw %}
```

@ 단방향, = 양방향, & 함수바인딩을 제공하는데 &가 살짝 애매하다 실제 사용할때는 레퍼런스를 찾아봐야할 듯하다.

## Chapter 17: 고급 디렉티브 기능

### 트랜스클루전 활용

트랜스클루전이라는 용어는 참조를 통해 문서 영역의 일부를 다른 문서에 삽입하는 것이라고 되어있다.
책을 읽어보면 대단히 특이한 기능으로 보이는데 디렉티브를 선언한 엘리먼트가 디렉티브로 교체되고 디렉티브 내의 **ng-transclude** 디렉티브가 디렉티브를 선언했던 디렉티브로 교체된다.

``` html
{% raw %}
<script type="text/ng-template" id="template">
  <div class="panel panel-default">
    <div class="panel-heading">
      <h4>This is the panel</h4>
    </div>
    <div class="panel-body" ng-transclude></div>
  </div>
</script>
...
<body ng-controller="upperCtrl">
<div user-directive>
  {{forScopeTest}}
</div>
{% endraw %}
```

디렉티브에서 template를 통해 참조할 돔트리를 작성하고 transclude를 확인할 **user-directive** 를 선언해다.

``` js
Module.directive('userDirective', function() {
  return {
    template: function() {
      return document.querySelector("#template").html();
    },
    link: function(scope) {
      scope.forScopeTest = 'directive';
    },
    scope: true,
    restrict: "E",
    transclude: true,
    //transclude: element
  }
})
```

js파일에서는 디렉티브를 정의한다.

``` html
{% raw %}
...
<body ng-controller='upperCtrl'>
<div class="panel panel-default">
  <div class="panel-heading">
    <h4>This is the panel</h4>
  </div>
  <div class="panel-body">
    {{forScopeTest}}
  </div>
</div>
{% endraw %}
```

책을 읽고 이해한 바로는 위와 같은 형태의 기능을 트렌스클루젼이라 한다. forScopeTest라는 인라인 바잉딩 표현식이 있는데 디렉티브 선언시에 scope를 true로하여 디렉티브 스코프를 생성한 경우에도 forScopeTest 데이터는 upperCtrl에서 가져오게 된다. false로 한 경우에는 forScopeTest 표현식에서 'directive'가 들어가 있는 것을 확인할 수 있다.

정리하면 **transclude가 true인 디렉티브** 를 선언한 엘리먼트의 컨트롤러 스코프 데이터만을 사용한다는 것이다. 이것은 transclude를 사용하지 않았을 경우와 동일하며 디렉티브 스코프를 false로 한경우 디렉티브의 링크 함수는 컨트롤러의 스코프를 가져오게되므로 표현식에 영향을 미칠 수 있게 된다.

transclude를 true로 하면 엘리먼트의 컨텐츠만 감싸게 되는데 이를 **element** 로 하게되면 엘리먼트 자체를 감싸게 된다.

### 컴파일 함수 활용

책을쓴 저자는 개인적으로 컴파일 함수를 사용하는 것을 선호하지 않는다고 밝히고 있다. 컴파일 함수를 정의하게 되면 링크함수의 정의는 무시되고 컴파일 함수가 리턴하는 함수가 링크 함수로 대체된다.

``` js
Module.directive('userDirective', function() {
  return {
    compile: function(element, attrs, transcludeFn) {
      return function($scope, $element, $attrs) {
        var parent = $element.parent();
        transcludeFn($scope.$new(), function(clone) {
          parent.append(clone);
        });
      }
    }
  }
})
```

compile에는 3번째 함수로 transclude에 사용되는 함수가 오게되는데 책의 내용만으로는 이해가 어렵다. 책에서는 디렉티브 선언에 template가 빠져있다. 첫번째인자는 스코프, 두번째 인자는 트랜스클루젼이 적용된 돔이 오게 될 것 같은데 정보가 온전치 않다.

### 디렉티브 내 컨트롤러 활용

``` js
Module
  .directive('userDirective', function() {
    return {
      transclude: true,
      controller: function($scope, $element, $attrs) {
        this.updateTotal = function() {
          $scope.total = 1;
        };
      }
    }
  })
  .directive('userDirective2', function() {
    return {
      require: "^userDirective",
      link: function(scope, element, attrs, ctrl) {
        ctrl.updateTotal();
      }
    }
  })
```

디렉티브에 컨트롤러를 정의할 수 있다. **require** 속성에서 디렉티브 의존성을 선언하게 되며 의존성 앞에 접두어를 사용할 수 있다.

* 접두어가 없는 경우 두 디렉티브가 같은 엘리먼트에 적용된다고 가정한다.
* ^: 디렉티브가 적용된 엘리먼트의 부모 일레먼트에서 다른 디렉티브를 찾는다.
* ?: 디렉티브를 찾을 수 없더라도 에러를 보고하지 않는다(사용에 주의를 요한다).

link함수에서의 **ctrl** 인자는 의존성 인자가 아니므로 다른 이름을 사용해도 상관없다.  
컨트롤러 함수에서 넘겨받는 **$scope** 인자는 컨트롤러를 정의하는 디렉티브의 스코프인데 이 부분은 실제 적용을 하면서 확인을 해봐야할 듯 하다.

### 기타 디렉티브 추가

**requrie** 속성을 통해 종속성을 선언할때 기존 내부 디렉티브를 지정할 수도 있다.

> ngModel

기본 메서드 및 속성

* $render(): 데이터 바인딩 값이 변할때 UI를 업데이트하기 위해 호출된다.
* $setViewValue(value): 데이터 바인딩 값을 업데이트한다.
* $viewValue: 디렉티브를 통해 표시할 포매팅 된 값.
* $modelValue: 스코프로 부터 포매팅되지 않은 값.
* $formatters: $modelValue를 $viewValue로 포매팅하는 포매터 함수 배열.

> ngModel **컨트롤러** 에서 제공하는 유효성 검증 메서드 및 속성

* $setPristine(): 컨트롤러의 유효성 검증 상태를 초기로 되돌려 유효성 검증 수행이 되지 않게 한다.
* $isEmpty(): 컨트롤에 값이 없을때 이를 알리기 위해 디렉티브에서 설정할 수 있다.
* $parsers: 모델 값의 유효성을 검증하는데 사용할 함수 배열
* $error: 유효성 검증 에러에 대응되는 속성을 지닌 객체를 반환한다.
* $pristine: 사용자가 컨트롤을 수정하지 않은 경우 true
* $dirty: 사용자가 컨트롤을 수정한 경우 true
* $valid: 모델이 유효한 경우 true
* $invalid: 모델이 유효하지 않은 경우 true

``` js

Module.directive("triButton", function() {
  return {
    replace: true,
    require: "ngModel",
    template: "tags in here",
    link: function(scope, element, attrs, ctrl) {
      var validateParser = function(value) {
        var valid = (value == "Yes" || value == "no");
        ctrl.$setValidity("confidence", valid);
        return valid ? value : undefined;
      }
      ctrl.$parsers.push(validateParser);
    }
  }
})
```

``` html
<form name="myForm" novalidate>
  <div><tri-button name="decision" ng-model="dataValue"/></div>
</form>
```

---

전반적으로 내용이 어렵고 실제로 해보기전에는 파악이 어렵다. 모듈을 생산할 때는 필수적인 내용이니 만큼 실제 코딩을 할 때는 한번 더 정독이 필요하다.

---

> 2016-01-24 추가

## Chapter 18: 모듈 및 서비스 활용

서비스는 횡단 관심사(cross-cutting concerns)를 구현하는데 쓰인다라는 SW공학책에서나 쓰일 것 같은 이해도 한번에 안가면서 모호해 보이는 언어를 사용하는데 결국 모델이나 뷰에 비종속적인 어느 곳에서나 쓰일 수 있는 프로세스의 집합체(작게는 함수)를 말하는 것으로 보인다.

### 서비스 생성

서비스를 생성하는 방법에는 세 가지가 존재한다. 방법에 상관없이 리턴된 서비스 객체는 싱글턴으로 하나의 인스턴스가 유지된다.

* factory
* service
* provider

> factory

팩터리 함수를 통해 싱글턴 객체를 리턴하는 구조로 팩터리 함수는 최초한번 실행후 리턴된 객체를 사용한다.

``` js
Module.factory('serviceName', function() {
  var privateVariable = 10;
  return {
    api01: function() {
      return privateVariable;
    }
  }
})
```

> service

책에서는 **new** 키워드가 일반적으로 쓰이지 않는다고 설명하고 있지만 **타입스크립트** 와 같은 상위 셋의 언어를 사용하거나 ES6의 추세를 봤을 땐 언어 자체가 클래스 기반으로 움직이고 있기 때문에 상속이 가능한 구조로서 활용도가 매우 높을 것으로 생각하고 있다.

팩터리 함수위치에 있는 함수를 **new** 키워드로 생성하여 사용하게 된다.

``` js
Module.service('serviceName', function() {
  var privateVariable = 10;
  this.api01 = function() {
    return privateVariable;
  }
})
```

> provider

프로바이더는 조금 특이한데 리턴된 객체의 $get 속성을 팩터리함수로 이용하여 객체를 반환한다. 이 반환하는 객체가 factory방식인지 service방식인지는 조금 더 공부를 해봐야 알겠으나 책의 예제 상으로는 factory방식으로 보이며 이런 경우라면 prototype 상속을 통한 구조화를 할 수 없으므로 활용폭이 제한되거나 팩터리 함수에서 객체를 생성해서 리턴하는 방식을 통해 가능할 것으로 생각된다. 그럼에도 불구하고 Module.config 메서드를 통해 설정을 지원하므로 유용해 보인다.

``` js
Module.provider('serviceName', function() {
  var debug = false;
  return {
    setDebugMode: function(set) {
      if (set) {
        debug = true;
      } else {
        debug = false;
      }
    }
    $get: function() {
      var privateVariable = 10;
      return {
        api01: function() {
          if (debug) {
            console.log(privateVariable);
          }
          return privateVariable;
        }
      }
    }
  }
})
.config(function(serviceNameProvider) {
  serviceNameProvider.setDebugMode(true);
});
```

### AngularJS 내장 서비스

* $anchorScroll
* $animate
* $compile
* $controller
* $document
* $exceptionHandler
* $filter
* $http
* $injector
* $interpolate
* $interval
* $location
* $log
* $parser
* $provide
* $q
* $resource
* $rootElement
* $rootScope
* $routeProvider
* $routeParams
* $sanitize
* $sce
* $swipe
* $timeout
* $window

---

> 2016-01-26 추가

## Chapter 19: 전역 객체, 에러, 표현식을 위한 서비스

### DOM API 전역 객체 접근

* $window
* $document
* $interval
* $timeout
* $location
* $log
* $anchorScroll

> $window, $document

단순 브라우저 전역객체인 window, document의 jqLite 래핑객체다.

> $interval, $timeout

window.setInterval, window.setTimeout을 접근하게 해주는 API로 AngularJS와의 연동을 위해 추가 기능이 정의되어 있다.

* $interval, $timeout 서비스와 함께 사용하는 인자

* fn: 실행을 지연할 함수
* delay: 지연 값 ms
* count: $interval에서 반복 횟수를 정의 0은 무한반복을 의미하며 기본값이다.
* invokeApply: true가 기본값이며 fn이 scope.$apply메서드 내에서 실행하게 된다.

> $location

window.location의 래퍼로. 현재 URL에 접근 가능하게 해주며 인덱스 URL(시작 URL)이 픽스되고 뒤의 **#** 이후를 제어할 수 있다.

http://domain.com/index.html#**user/can/control?this=yes#youcan**

URL은 경로, 검색어, 해시라는 3개의 구성 요소로 이루어져있다.

* 경로: http://domain.com/index.html#**user/can/control**?this=yes#youcan
* 검색어: http://domain.com/index.html#user/can/control?**this=yes**#youcan
* 해시: http://domain.com/index.html#user/can/control?this=yes#**youcan**

$location 서비스에서 제공하는 메서드는 아래와 같다.

* absUrl(): 전체 URL을 반환한다.
* hash(target?): 해시를 가져오거나 설정한다
* hoset(): 호스트네임 컴포넌트를 반환한다.
* path(target?): 경로 컴포넌트를 가져오거나 설정한다.
* port(): 포트번호를 반환한다.
* protocol(): URL의 프로토컬 컴포넌트를 반환한다.
* replace(): URL이 바뀔 때 새브라우저 히스토리가 생기는 대신 가장 최신 희스토리 항목을 대체한다.
* search(term?, params?): 검색어를 가져오거나 설정한다.
* url(target?): 경로, 쿼리 문자열, 해시를 한꺼번에 가져오거나 설정한다.

$location 서비스에서 정의하는 이벤트

* $locationChangeStart: URL이 변경되기 전에 호출된다. Event객체에서 preventDefault 메서드를 호출하면 URL 변경을 막을 수 있다.
* $locationChangeSuccess

``` js
Module.controller("defaultCtrl", function($scope, $location) {
	$scope.$on("$locationChangeSuccess", function(event, newUrl) {
		$scope.url = newUrl;
	});
	$location.path("");
	$location.hash("");
	$location.search("");
	$location.path("/cities/london");
	$location.hash("north");
	$location.search("select", "hotels");
	$location.url("/cities/london?select=hotels#north");
});
```

#### HTML5 URL 활용

저자는 위 API들이 브라우저가 새 HTML문서를 로드하지 않게끔 #문자 이후의 URL 컴포넌트 영영을 매번 복제해야하는 것을 우아하지 않다고 지적한다. 이를 위해 HTML5 히스토리 API가 있으며 더 우아한 방식으로 문서를 재로드하지 않고 URL을 변경할 수 있게 해준다고 기술하고 있다. **주요 브라우저의 최신 버전에서** 는 모두 이 같은 히스토리 API를 지원하며, AngularJS에서 이 기능을 활용하려면 $location 서비스의 프로바이더인 $locationProvider를 통해 지원 기능을 활성화하면 된다.

HTML5 모드를 활성 화 하고 위 코드를 다시 실행시켰을때 URL이 더 깔끔하게 변한다고 기술하고 있으나 책이 잘못된 것인지 차이를 알 수 없게 기술되어 있어 실제로 테스트가 필요하다.

``` js
Module.config(function($locationProvider) {
	if (window.history && history.pushState) {
		$locationProvider.html5Mode(true);
	}
});
```

위 코드에서 $window 서비스를 주입하지 않는 이유는 config 함수로는 상수 값이나 프로바이더만이 주입될 수 있기 때문이다.

> $anchorScroll

이 서비스는 $location.hash 메서드에서 반환한 값과 id가 일치하는 엘리먼트가 보이게끔 브라우저 창의 스크롤을 조정해준다. 사용 방법이 독특한데 $anchorScroll 서비스는 서비스 객체를 참조할 필요가 없으며 단지 의존성만을 선언하면 된다. 서비스 객체가 생성되면(의존성 선언을 통해) $location.hash 값의 모니터링을 시작하고, 값이 변할 때 자동으로 스크롤을 조정한다.

``` js
Module.controller("defaultCtrl", function($scope, $location, $anchorScroll) {
	$scope.scrollToTop = function() {
		$location.hash('top');
	};
});
```

``` html
...
<body>
	<p id='top'>This is the top</p>
	<button ng-click='scrollToTop()'>Go</button>
</body>
```

클릭을 하면서 호출된 함수에서 $location.hash 함수를 호출하게되고 자동으로 스크롤이 일어나게 된다. 자동 스크롤이 일어나지 않게 하려면 프로바이더를 통해 선언을 하면된다.

``` js
Module
	.config(function($anchorScrollProvider) {
		$anchorScrollProvider.disableAutoScrolling();
	})
	.controller("defaultCtrl", function($scope, $location, $anchorScroll) {
		$scope.scrollToTop = function() {
			var wantToScroll = true;
			$location.hash('top');
			if (wantToScroll) {
				$anchorScroll();
			}
		});
});
```

> $log

$log는 console객체의 래퍼로 debug, error, info, log, warn 메서드를 정의하고 있다. $log는 기본적으로 debug 메서드가 활성화되지 않으며 컨피그를 통해 활성화 시켜야한다.

> $exceptionHandler

실행 도중 일어난 에러를 처리한다. 기본 구현체에서는 $log 서비스에 정의 된 error 메서드를 호출하며 이 메서드는 console.error 를 호출하게 된다.

$exceptionHandler는 기본적으로 처리되지 않은 예외만을 대상으로한다. 라고 되어있는데 예제가 별로 쓸모 없는 것 같다. 강제로 익섹션을 내는 용도의 예제가 실려있다. 좀 보기 편하게 $exceptionHandler 서비스를 재정의 할 수 도 있으나 주의를 요한다. 정도로만 알고 있으면 될 것으로 보인다.

### 위험한 데이터 처리

해커들이 폼을 통해 악의적인 코드를 주입하는 이를 막기 위한 서비스들이 있다. AngularJS에서는 위험한 문자열이 ng-model, 인라인 바인딩등에서 표현되지 못하도록 자동 이스케이프한다. 위험한 문자열은 태그등을 생각하면된다.

> $sanitize

$sanitize는 AngularJS에서 ngSanitize 모듈을 추가로 받아 로드하여 사용할 수 있으며 모듈 선언에서 ngSanitize를 주입 해야한다. $sanitize 서비스는 HTML 문자열에서 위험한 문자열을 이스케이프된 대체 문자열로 대체하게된다. 아까 자동으로 적용이 된다고 했는데 아래와 같은 차이점이 있다.

ng-bind-html 디렉티브등을 이용해서 html을 태그상태로 표현하려고 할때 이스케이핑을 하지 않고 HTML 태그 상태로 보여주되 보여주는 과정에서 script 및 css 엘리먼트, 인라인 자바스크립트 핸들러 및 스타일 어트리뷰트등 문제를 일으킬 만한 요소를 모두 제거한다. ngSanitize 모듈을 인젝션하게되면 ng-bind-html 디렉티브를 사용할때 자동으로 적용되게 된다. 이를 책에서는 **위생화** 로 표현하고있다.

ng-bind-html이 아닌 다른 곳에 표현될 때 이런 위생화가 표현하다고 한다면 컨트롤러에서 **$sanitize(targetData)** 메서드를 명시적으로 호출할 수 있다.

> $sce

$sce는 명시적으로 데이터를 신뢰할 때 사용하나 저자는 사용할 일이 거의 없을 뿐더러 위험하니 사용하지 않기를 권하고 있다. 이는 HTML태그를 사용할 수 있게 허용한다.

``` js
angular
	.module('itIsModule', ['ngSanitize'])
	.controller("defaultCtrl", function($scope, $sce) {
		$scope.htmlData = "<p>This is <b onmouseover=alert('Attack!')>dangerous</b> data</p>";
		$scope.$watch("htmlData", function(newValue) {
			$scope.trustedData = $sce.trustAsHtml(newValue);
		});
	});
});
```

``` html
<p><input class="form-control" ng-model="htmlData"/></p>
<p ng-bind-html="trustedData"></p>
```

위 태그의 인라인 이벤트 코드는 명시적으로 신뢰를 선언했으므로 실행된다.

---

> 2016-01-27 추가

### AngularJS 표현식 및 디렉티브 활용

> $parse

AngularJS 표현식을 인자로받아 스코프 객체를 사용해 표현식을 평가하는데 사용할 수 있는 **함수** 로 변환한다.

``` js
Module
	.controller("defaultCtrl", function($scope) {
		$scope.price = "100.23";
	})
	.directive("evalExpression", function($parse) {
		return function(scope, element, attrs) {
			scope.$watch(attrs["evalExpression"], function(newValue) {
				try {
					var expressionFn = $parse(newValue);
					var result = expressionFn(scope);
					if (result == undefined) {
						result = "No result";
					}
				} catch(ex) {
					result = "Cannot evaluate expression";
				}
				element.text(result);
			});
		}
	});
```

``` html
<p><input class="form-control" ng-model="expr" /></p>
<div>Result: <span eval-expression="expr"></span></div>
```

스코프를 인자로 받는 함수를 받는 것만 눈여겨 보면 활용도가 있을 것으로 보인다. **price | currency** 라고 input 태그에 입력하게 되면 표현식이 평가되어 span태그에 결과가 나타나게된다. 표현식이 유효하지 않은 경우를 대비해 try-catch 문은 필수적이다.  
로컬 데이터를 제공할 수 있는데

``` js
var expressionFn = $parse("total | currency");
...
var localData = {
	total: Number(newValue) + (number(newValue) * (number(scope.tax) / 100))
};
element.text(expressionFn(scope, localData));
```

스코프에 존재하지 않는 **total** 속성에 대한 참조를 localData에 대입하여 사용했다.

> $interpolate

$interpolate 서비스는 단순히 표현식 자체가 아닌 표현식이 들어 있는 문자열과 연동이 가능해 $parse 보다 더 유연하다.

``` js
{% raw %}
$parse("total | currency");
$interpolate("The total is {{total | currency}}");
{% endraw %}
```

{% raw %}
이런식의 차이다. 인라인 바인딩을 처리하며 **{{** 같은 문자열을 보간문자라 하는데 이 문자가 다른 프레임워크와의 사용으로 바꿔야할 필요성이 있다면 $interpolateProvider를 통해 해결할 수 있다.

* startSymbol(symbol): {{를 대체한다.
* endSymbol(symbol): }}를 대체한다.
{% endraw %}

이 설정은 HTML 마크업을 포함해 AngularJS 영향 영역 전체에 영향을 미치니 주의해야한다.

> $compile

바인딩 및 표현식이 들어있는 HTML 스트링을 처리해 스코프로부터 콘텐처를 생성할 수 있는 함수를 제공하며 디렉티브를 지원한다는 차별점이 있다.

``` js
Module
	.controller("defaultCtrl", function($scope) {
		$scope.cities = ["London", "Paris", "Seoul"];
	})
	.directive("evalExpression", function($compile) {
		return function(scope, element, attrs) {
			var content = "<ul><li ng-repeat='city in cities'>{{city}}</li></ul>";
			var listElem = angular.element(content);
			var compileFn = $compile(listElem);
			compileFn(scope);
			element.append(listElem);
	});
```

## Chapter 20: Ajax 서비스 및 프로미스

> $http

* get(url, config?)
* post(url, data, config)
* delete(url, config)
* put(url, data, config)
* head(url, config)
* jsonp(url, config)

W3C에 따르면 get은 안전한 post는 안전하지 않은 상호작용으로 정의되어 있으며 get은 읽기 전용 정보 조회, post는 앱의 상태 변경 작업에 사용해야 한다.

> 2016-01-30 추가

ajax 통신에 따른 리던값으로는 **프로미스** 객체를 받게 된다. 프로미스 객체가 정의하는 메서드는 아래와 가다.

* success(fn)
* error(fn)
* then(fn, fn)

기존에 개인적으로 쓰던 프로미스 라이브러에서는 error가 아닌 **catch** 라는 메서드가 존재했으나 스펙상 어떤 것이 맞는 것인지는 모르겠다. 응답이 JSON형식일 경우 파싱된 상태로 콜백 함수에 전달된다.
then 메서드에서는 단순히 success와 error콜백을 함께 등록하는 것 뿐만이 아니라 추가적인 상세 정보에 접근 할 수 잇는 인자가 넘어오는데 정의하고 있는 속성은 아래와 같다.
>> error는 편의상 추가된 것이라고 이 장의 뒷 부분에서 언급하고 있다.

* data
* status
* header
* config

JSON의 경우 AngularJS에서 내부적으로 직렬화 하여 요청을 하고 들어온 데이터를 파싱하여 객체상태로 넘겨주는 등의 역할을 해주지면 나머지는 그대로 들어오게 된다.

### Ajax 요청 설정

$http의 메서드들은 config 인자를 옵션으로 지원하여 다양한 설정이 가능하다. config인자의 속성은 아래와 같다.

* data
* header
* method
* params
* timeout
* transformRequest
* transformResponse
* url
* withCredentials
* xsrfHeaderNamexsrfCookieName

> transformRequest, transformResponse

사용자에게 프로미스로 인자로 값이 전달되기 전의 프로미스 콜백함수라고 보면 될 듯 하다. 책에서는 중요한 듯 설명하고 있으나 응답 데이터가 XML일 경우 이를 파싱하여 필요한 정보만 사용자에게 건내는 등의 사전 작업 함수를 등록하는 일 정도다. 뒤에 프로바이더에서 한번 더 설명하므로 여기선 생략한다

> $httpProvider

### Ajax 기본값 설정

* defaults.headers.common
* defaults.headers.post
* defaults.headers.put
* defaults.transformResponse
* defaults.transformRequest
* interceptors
* withCredentials

기본 헤더, post 메서드, put 메서드 사용시의 헤더 정의가 있고 transfromRe* 함수는 배열로써 존재하는데 이 곳에서 JSON데이터를 기본으로 파싱하여 자바스크립트 오브젝트를 넘겨주는 것으로 예상된다. 응답시에 리턴되는 프로미스 콜백의 이전 체인 함수라고 생각하고 등록하면서 된다. 배열이니 push를 통해 넣는다.

**defaults** 객체는 $http.defaults를 통해서 접근이 가능하며 기본 값이기 때문에 전역적으로 영향을 준다.

### Ajax 인터셉터 활용

> interceptors

변형 함수(transformRe*)를 정교한 로직으로 대체할 수 있는 기능으로 사용된다.

``` js
.config(function($httpProvider) {
	$httpProvider.interceptors.push(function() {
		return {
			request: function(config) {
				config.url = "fix_url.json"
				return config;
			},
			response: function(response) {
				console.log("count: " + response.data.length);
				return response;
			}
		}
	});
});
```

이런식이다 요청을 하게 되면 최종적으로 config 함수에서 url을 바꿔버리니 요청이 "fix_url.json"으로만 가게 될 것이 응답 시에는 로그를 찍을 것이다. 팩터리 함로 특정 속성을 포함하는 객체를 리턴하는 것을 눈여겨보고 이 함수가 변형 함수 이 전에 호출되는지 이 후에 호출되는지를 확인할 필요가 있다. 책에서는 변형함수 이후에 호출 되는 뉘앙스를 풍긴다. 리턴하는 객체에 정의할 수 있는 속성은 아래와 같다.

* request
* requestError
* response
* responseError

에러는 프로미스의 error 메서드 콜백으로 들어갈 함수를 등록한다고 보면 될 듯 하다.

### 프로미스의 활용

> ##promise에 대한 내용을 공부하고 싶다면 한빛미디어에서 무료로 제공하고 있는 [책(JavaScript Promise)](http://www.hanbit.co.kr/ebook/look.html?isbn=9788968487293)이 있으니 이를 참고하면 도움이 될 듯 하다.

프로미스는 스펙이며 여러 구현체가 존재하고 AngularJS에서는 $q 서비스를 통해 프로미스를 제공한다.

> $q

* all(promises[]): 프로미스 배열의 모든 프로미스가 리졸브되거나 하나의 에러가 발생할 경우 리졸브된 프로미스가 반환된다.
* defer(): 지연 객체를 생성한다.
* reject(reason): 거부된 프로미스를 반환한다.
* when(value): 항상 리졸브되는 프로미스를 사용해 값을 감싼다(이해불능)

>> defer()

지연객체를 반환하는데 지연 객체에서 정의하는 멤버는 아래오 ㅏ같다.

* resolve(result)
* reject(reason)
* notify(result)
* promise

notify는 처음보는데 지연활동으로 부터 중간 결과를 제공한다고 되어 있다.

프로미스가 정의하는 메서드는 아래오 ㅏ같다.

* then(success, error, notify)
* catch(error)
* finally(fn)

프로미스의 콜백 함수에는 지연 객체가 인자로 들어오게된다. then은 프로미스 객체를 리턴하므로 체인이 가능하다.

## Chapter 21: REST 서비스

> REST(Representational State Transfer)는 API 스타일이다.

### $http 서비스의 활용

$http 서비스를 이용해서 RESTful을 처리하는 장들을 보여주고 로컬 데이터와 서버 데이터간의 동기화를 해야하기 때문에 신경을 많이 써야하며 AngularJS가 추구하는 방식과 대치된다고 말하고 있다.

### Ajax 요청 숨기기

ngResource모듈을 설치해야한다. AngularJS 홈페이지에서 받을 수 있다.

> $resource

``` js
var baseUrl = 'http://localhost:5500/products/';
var 접근객체 = $resource(basUrl + ':id', { id: "@id"});
```

접근 객체가 지원하는 메서드는 아래와 같다.

* query()
* get(id)
* delete(params, product)
* remove(params, product)
* save(product)

>> query()

GET. baseUrl 부분을 호출한다. 리턴값은 컬레션이며 비어있다. 쿼리 완료에 따라 내용이 업데이트된다. 컬렉션에 **$promise** 속성이 존재한다.
http://localhost:5500/products

>> get(id)

GET.
http://localhost:5500/products/id

>> delete, remove(param, product)

DELETE.
http://localhost:5500/products/id

>> save(product)

POST.
http://localhost:5500/products/id

### 데이터 객체 수정

query 메서드는 Resource 객체를 사용해 컬렉션을 채운다(컬렉션 개별 요소가 Resource 객체라는 듯). Resource 객체는 서버에서 반환한 데이터에 지정된 송석을 모두 정의하며, 컬렉션 배열을 사용하지 않고도 데이터를 조작할 수 있는 메서드를 정의한다.

> Resource 객체

* $get(): 서버로부터 객체를 가져와 갱신하며, 커밋하지 않은 로컬 변경 사항은 모두 제거된다.
* $delete(): 서버에서 객체를 삭제한다.
* $remove(): 서버에서 객체를 삭제한다.
* $save(): 객체를 서버에 저장한다.

모두 리턴값으로 프로미스 객체를 리턴한다. 주의할 점은 $delete, $remove의 경우 삭제요청을 보내지면 컬렉션 배열에서 스스로 제거될 수 는 없다는 점을 알아야한다.

### 새 객체 생성

사실 코드로 이해가 안가는 부분인데 $scope.productsResource는 $resource 서비스를 통해 반환받은 접근 객체인데 예제 코드는 일단 아래와 같다.

``` js
function(product) {
	new $scope.productsResource(product)
				.$save()
				.then(function(newProduct) {
					$scope.products.push(newProduct);
				});
}
```

new $scope.productsResource(product) 이 부분이 new에 해당하는 코드인것 같은데 책에서는 접근객체의 메서드 정의에 바로 호출이 가능하다라는 부분이 없는데 일단은 new 키워드와 데이터를 통해 새 Resource 객체 생성이 가능하다고 봐야할 것 같다.

### $resource 서비스 행동 설정

RESTful은 API 스타일이므로 서버에 따라 정의가 다를 수 있다. 그래서 CRUD에 맞춰 요청 방식을 직접 지정할 수 있다.

``` js
$scope.productsResource = $resource(baseUrl + ":id", { id: "@id" }, {
	create: { method: "POST", save: { method: "PUT"} }
})
```

Resource 객체의 메서드들($로 시작)을 액션이라 칭하는데 지금 $resource의 3번째 인자를 통해 액션을 정의하고 있다.

* method: HTTP 방식 결정
* params: $resource 서비스 함수의 첫번째 인자로 전달되는 영역 변수 값 지정
* url: 이 액션의 기본 URL을 오버라이드.
* isArray: true일 경우 JSON데이터 배열이 응답으로 온다고 지정한다. 기본값은 false고 요청응답이 단일 객체라고 지정한다.

추가로

* transformRequest
* transformResponse
* cache
* timeout
* withCredentials
* responseType
* interceptor

속성을 사용해 액션에서 생성하는 Ajax요청을 설정할 수 있다.

``` js
function(product) {
	new $scope.productsResource(product)
				.$create()
				.then(function(newProduct) {
					$scope.products.push(newProduct);
				});
}
```

커스텀 액션인 $create()를 호출하고 있다.

### resource 활용에 적합한 컴포넌트 구현

$resource 서비스에서 제공하는 데이터와 연동할 수 있는 컴포넌트를 작성할 대는 RESTful 지원 기능의 on/off 설정 옵션부터 서버에서 업데이트하는데 필요한 메서드 및 HTTP 방식등을 지정할 수 있게끔 구현해야한다.

### 비동기적 데이터 함정 피하기

한글의 탈을쓴 외계어로 작성된 듯 하다. 이해가 가지 않으나 스코프 와처와 이벤트 핸들러를 사용해 자동으로 클라이언트 뷰와 서버와 동기화 되는 코드를 짜려고할때 조심해야한다고 말하고 있는 듯 하다.

---

> 2016-01-31 추가

## Chapter 22: 뷰를 위한 서비스

### ngRoute

AngularJS 홈페이지에서 추가로 route파일을 다운로드 받아야한다. ng-view 디렉티브가 이 곳에 정의되어 있는 것으로 보인다.

``` js
.config(function($routeProvider, $locationProvider) {
	$locationProvider.html5Mode(true);
	$routeProvider.when("/list", {
		templateUrl: "/tableView.html"
	});
	$routeProvider.when("/conservative/:id", {
		templateUrl: "/tableView.html"
	});
	$routeProvider.when("/eager/:id/:data*", {
		templateUrl: "/tableView.html"
	});
	$routeProvider.otherwise("/list", {
		templateUrl: "/tableView.html"
	});
})
controller(function($location) {
	$location.path("/list");
});
```

when에서 정의된 URL은 $location.path(URL)을 통해 이동가능하며 코드가 아닌 브라우저에 실제 주소를 입력할 경우에는 요청으로 간주되어 라우터는 동작하지 않는다. ng-view 디렉티브가 선언된 곳에 templateUrl에서 지정한 위치의 파일이 로드되게 된다.

### 라우트 및 라우트 파라미터 접근

``` js
.controller("ctrl", function($scope, $http, $resource, $location, $route, $routeParam, baseUrl) {
	$scope.$on("$routeChangeSuccess", function() {
		if ($location.path().indexOf("/edit/") == 0) {
			var id = $routeParams["id"];
			for (var i = 0; i < $scope.products.length; i++) {
				if ($scope.products[i].id == id) {
					$scope.currentProduct = $scope.products[i];
					break;
				}
			}
		}
	})
})
```

라우팅 주소가 /edit/ 으로 시작하는 경우 id 파라미터에 접근하고 있다.

> $route

메서드 및 속성

* current
* reload()
* routes

이벤트

* $routeChangeStart
* $routeChangeSuccess
* $routeUpdate
* $routeChangeError

### 라우트 설정

> $routeProvider

위의 예제에서는 단순히 templateUrl만 정의했는데 추가적인 설정이 가능하다.

* controller: 컨트롤러 설정
* controllerAs
* template
* templateUrl
* resolve: 의존성
* redirectTo
* reloadOnSearch
* caseInsensitiveMatch

>> resolve

컨트롤러에 주입할 의존성을 지정할 수 있다. 의존성으로 서비스를 지정할 수도 있지만, resolve속성은 뷰를 초기화하는 데 필요한 작업을 수행하는 데 더 도움이 되며 resolve 속성의 의존성으로 프로미스 객체를 반환할 경우, 라우트에서 의존성이 리졸브되기 전까지 컨트롤러에 인스턴스화하지 않기 때문이다. 말이 어렵게 되어있어 이해 불능이니 한번 더 살펴봐야할 듯하다.

``` js
.factory("productsResource", function($resource, baseUrl) {
	return $resource(basUrl + ":id", { id: "@id" }, {
		create: { method: "POST" }, save: { method: "PUT" }
	});
})
.config(function($routeProvider) {
	$routeProvider.otherwise({
		templateUrl: "/tableView.html",
		controller: "tableCtrl",
		resolve: {
			data: function(productsResource) {
				return productsResource.query();
			}
		}
	});
})
.controller("tableCtrl", function($scope, data) {
	$scope.data = data;
});

resolve 속성에 data 를 정의하고 productsResource 를 주입하고 프로미스를 리턴한다. 프로미스가 리졸브 될 때까지 인스턴스화가 미뤄져서 tableView.html가 늦게 보일 것이라고 말하고 있다. 컨트롤러에서는 정의한 data 속성을 주입하고 있다. 라우트와 연계해 컨트롤러를 사용할 때는 컨트롤러/스코프 상속 규칙이 적용됨을 책은 강조하고 있다.
```

## Chapter 23: 애니메이션 및 터치를 위한 서비스

### ngAnimation모듈 설치

AngularJS홈페이지에서 내려받아서 사용하면 된다. css를 통해 애니메이션을 처리한다고 했는데 예제를 봤을 땐 이 파일이 왜 필요한지 이해가 가지 않는다.

### 애니메이션의 정의 및 적용

애니메이션을 지원하는 내장 디렉티브 및 애니메이션과 관련한 이름

* ng-repeat: enter, leave, move
* ng-view: enter, leave
* ng-include: enter, leave
* ng-switch: enter, leave
* ng-if: enter, leave
* ng-class: add, remove
* ng-show: add, remove
* ng-hide: add, remove

### 병렬적 애니메이션의 위험성

ng-view의 경우 새 뷰를 DOM에 추가한 후기존 뷰를 DOM에서 제거한다.

---

애니메이션 예제가 스스로 css를 통해 애니메이션을 만들고 그 클래스를 직접 입히고 있어 모듈의 왜 필요한지 다른 자료를 참고해야할 듯 하다.

### 터치 이벤트 지원

ngTouch모듈이 필요하며 AngularJS 홈페이지에서 받을 수 있다. ng-click 디렉티브가 터치 환경에서 적합하게 동작할 수 있도록 대체되며 ng-swipe-left, ng-swipe-right 디렉티브를 통해 스와이프가 발생했을 때의 동작을 지정할 수 있다.

---

> 2016-02-01 추가

## Chapter 24: 프로비전 및 주입을 위한 서비스

프로비전이라는 말이 다소 어려운데다가 책에 설명도 없다 사전적인 의미로 검색해보니 제공하다, 규정하다 등이 나오는데 이와 유사한 의미로 이해하고 AngularJS 내부에서 동작하는 함수로 알아두면 될 것 같다.

### AngularJS 컴포넌트 등록

> $provide

$provide 서비스는 의존성을 충족할 수 있게끔 서비스 같은 컴포넌트를 주입할 수 있게 등록하는 데 사용된다. 대부분의 경우 $provide 서비스에서 정의하는 메서드는 Module 타입을 통해 외부로 노출되고 접근할 수 있지만 Module에서 제공하지 않는 특수 메서드도 하나 존재한다

$provide 서비스가 정의하는 메서드

* value(name, service)
* constant(name, value)
* factory(name, service)
* provider(name, service)
* service(name, service)
* decorator(name, service)

Module 타입을 통해 외부로 노출되지 않는 메서드는 decorator 메서드다.

``` js
.config(function($provide) {
	$provide.decorator("$log", function($delegate) {
		$delegate.originalLog = $delegate.log;
		$delegate.log = function(message) {
			$delegate.originalLog("Decorated: " + message);
		};
		return $delegate;
	});
});
```

decorator는 원본 서비스명을 첫 번째 인자로하며 두번째 콜백에 반드시 $delegate 의존성을 선언하여 원본 서비스의 인스턴스를 받게 된다. 여기서 리턴되는 객체가 원본 객체를 대신하여 의존성을 리졸브 한다고 이해하면 될 듯 하다.

### 주입 관리

> $injector

$injector 서비스는 함수에서 선언하는 의존성을 판단하고, 의존성을 리졸브하는 책임을 진다.

$injector 서비스가 정의하는 메서드

* annotate(fn)
* get(name)
* has(name)
* invoke(fn, self, locals)

>> annotate(fn)

함수 인자를 스트링으로 가져오는 역할을 한다. AngaulrJS의 의존성 주입에 대해 역할을 할 것으로 보이는데 아마도 코드로는 이런식이 아닐까 한다.(AngularJS의 코드를 확인해보지는 않았다.)

``` js
function annotate(fn) {
	var str = fn.toString();
	var regexp = "()안으로부터 인자의 이름을 가져와서 ','로 split하여 배열로 리턴하는 RegExp";
	return regexp.exec(str);
}
```

>> get(name)

name에 맞는 서비스를 리턴한다.

>> has(name)

인자로 스트링을 넣으면 그 인자가 의존성 객체인지를 리턴한다.

>> invoke(fn, self, locals)

fn은 호출하고자 하는 함수가 되고 self 인자는 context(this)가 될 객체다.
locals는 객체인데 의존성 인자가 아닌, 즉 $injector.has(name)에 의해 false 리턴된 인자의 이름에 맞는 객체를 속성으로 제공하게 되는 역할을 하게 된다.

``` js
var logClick = function($log, $exceptionHandler, message) {
	return false;
}
$injector.invoke(logClick, null, {
	message: "Button Clicked"
});
```

> $rootElement

$rootElement는 ng-app 디렉티브를 가진 엘리먼트의 jqLite 객체의 확장이며 $injector 서비스를 반환하는 injector 메서드를 제공한다.

``` js
$rootElement.injector().invoke(logClick, null, {
	message: "Button Clicked"
});
```

이런식의 접근을 할 이유가 전혀 없지만 이를 통해 AngularJS 내부의 모습을 볼 수 있다.

---

의존성을 리졸브한다는 난해한 말을 하고 있는데 AngularJS 내부에서 읜존성 주입을 어떻게 처리하는 지를 보여주는 함수라고 생각되며 딱히 쓸일은 없어보인다.

---

## Chapter 25: 단위 테스트

## source code

<http://www.apress.com/9781430264484?gtmf=sZ> 이동 후 **Source Code/Downloads**
