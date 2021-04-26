JavaScript 개발은 웹 사이트의 규모가 커질수록 소스를 관리하고 배포하는 비용이 커지는 경향이 있습니다. 또한 오래된 소스의 의존성 파악이 어려워 섣불리 수정하지 못하는 상황에 처하기도 합니다. 더 나은 웹 사이트 혹은 웹앱을 위해서는 해결해야 할 과제이며, 이는 [RequireJS](http://requirejs.org/)를 사용하여 라이브러리 차원에서 보완할 수 있습니다.

이 글에서는 RequireJS의 바탕이 되는 [AMD(Asynchronous Module Definition)](https://github.com/amdjs/amdjs-api/wiki/AMD)의 기본 개념을 살펴보고 RequireJS를 이용한 개발 가이드를 제시합니다.

## AMD

AMD는 동적 로딩, 의존성 관리, 모듈화가 톱니바퀴처럼 아름답게 맞물린 API 디자인을 제시한다. AMD의 자세한 배경과 연관 기술에 관해서는 "[JavaScript 표준을 위한 움직임: CommonJS와 AMD](https://d2.naver.com/helloworld/12864)"를 참고한다. 이 글에서는 AMD의 근간이 되는 3가지 개념을 살펴보겠다.

### 동적 로딩

<script> 태그는 페이지 렌더링을 방해한다. <script> 태그의 HTTP 요청과 다운로드, 파싱(Parsing), 실행이 일어나는 동안 브라우저는 다른 동작을 하지 않는다. 브라우저 입장에서는 당연하고 안전한 동작 방식이지만 사용자 입장에서는 빨리 화면이 보이고 버튼이 동작하기를 바랄 뿐이다. 그래서 최적화 기법 중의 하나로 <script> 태그를 가능한 한 <body> 태그의 마지막에 배치하는 방법이 있다.

하지만 사용자의 첫 인터랙션이 가능할 때까지 걸리는 시간이 줄어들지는 않는다. 페이지를 더 빨리 렌더링할 수는 있어도 첫 렌더링과 첫 인터랙션에 필요하지 않은, 페이지에 필요한 모든 JavaScript를 로딩하기 때문이다. 화면이 복잡하고 AJAX로 점철된 웹앱 수준의 규모에서는 이 시간이 큰 폭으로 커진다. 웹앱은 AJAX로 전환되는 여러 뷰(View)를 가지고 있는 경우가 흔하다. 더 최적화를 하자면 첫 렌더링과 인터랙션에 필요한 JavaScript만 먼저 로딩하고 후에 사용자의 반응에 따라 나머지를 로딩하는 점진적인 방식이 필요하다.

동적 로딩(Dynamic Loading, Lazy Loading 이라고도 부른다)은 페이지 렌더링을 방해하지 않으면서 필요한 파일만 로딩할 수 있다. 이를 구현하는 방법 중 하나로 <script> 태그의 동적 삽입이 있다. 이는 JavaScript로 <script> 태그를 생성하여 추가하는 방법이다. 이 외에도 XMLHttpRequest, document.write(), defer 같은 방법이 있지만 범용적으로 사용하기에는 치명적인 단점이 하나씩은 있어서 <script> 태그의 동적 삽입이 제일 안전하고 합리적이다. 간단한 구현은 다음과 같다.

```javascript
var scriptEl = document.createElement('script');  
scriptEl.type = 'text/javascript';  
scriptEl.src = 'example.js';  
document.getElementsByTagName('head')[0].appendChild(scriptEl);  
```

이를 응용하면 JavaScript 파일의 URL을 매개변수로 받아 범용적인 동적 로딩 함수를 만들 수 있다. 그리고 로딩 완료 이벤트 처리가 가능하므로 안전하게 해당 파일의 변수나 함수를 사용할 수 있다. 즉, 비동기로 동작하며 로딩 완료 이벤트 핸들러는 콜백 함수이다.

```javascript
function loadScript(url, callback) {  
    var scriptEl = document.createElement('script');
    scriptEl.type = 'text/javascript';
    // IE에서는 onreadystatechange를 사용
    scriptEl.onload = function () {
        callback();
    };
    scriptEl.src = url;
    document.getElementsByTagName('head')[0].appendChild(scriptEl);
}

loadScript('example.js', function () {  
    // example.js가 로딩 완료한 시점에 실행
});
```

하지만 보통 파일이 여러 개 필요하고 각 파일의 삽입 순서를 지켜야 하기 때문에 위 함수만으로는 아래와 같은 콜백 지옥(?)에 빠질 수 있다.

```javascript
loadScript('file1.js', function () {  
    loadScript('file2.js', function () {
        loadScript('file3.js', function () {
            loadScript('file4.js', function () {
                // 콜백 지옥에 빠졌다.
            });   
        });   
    });   
});
```

다행스럽게도 AMD는 이를 자연스럽게 해결했다.

### 의존성 관리

JavaScript는 스크립트 간의 의존성을 파악하기 힘들다. 왜냐하면 언어 차원에서 #include나 package/import같은 명시적이고 강제적인 키워드, 패키징 정책을 지원하지 않기 때문이다. 파일 상단에 JSDoc의 @requires라도 적혀 있으면 다행이지만 결국 주석일 뿐이다.

이전 장에서 설명한 loadScript() 함수는 C/C++의 #include 기능을 흉내 낼 수 있다. 필요한 파일 목록과 로딩 순서를 파악하는 데 도움이 되긴 하지만 파일 이름으로 사용하려는 함수나 객체의 이름을 유추할 수 있다는 보장은 없다. 코딩 규칙만으로는 부족하고 불안전하다. 결국 Java의 package/import같은 기능이 필요하고 이것이 특정 기능을 불러와서 사용할 수 있는 유일한 방법이어야 한다. 이를 위한 기본 조건은 특정 기능의 스크립트가 이름을 붙일 수 있는 하나의 단위로 묶여야 한다. 그래야 다른 스크립트에서 그 이름으로 스크립트를 불러올 수 있는 방법이 생기기 때문이다.

예를 들어 유틸리티성 함수를 모아놓은 객체가 있다고 가정하자. 보통 그 객체를 전역변수 util로 할당하고 사용할 것이다. 하지만 이 객체를 불러오는 강제적이고 유일한 방법을 구현해야 하므로 먼저 객체 이름과 정의를 비밀 공간에 넣을 수 있는 함수가 필요하다. 그 함수를 사용해서 객체를 정의하는 방법은 다음과 같을 것이다.

```javascript
defineModule('util', {  
    trim: function () { 
        //
    },
    extend: function () {
        //
    }
});
```

정의한 객체를 사용하기 위해서는 반대로 호출 함수가 있어야 한다. 정의한 객체가 비밀 공간에 있기 때문이다. 이 호출 함수가 다음과 같이 객체를 불러오는 강제적이고 유일한 방법이 된다.

```javascript
var util = loadModule('util');  
util.trim();  
```

### 모듈화

스크립트 내부에서만 사용하는 변수, 함수들은 전역 공간에 둘 필요가 없고 두어서도 안 된다. 전역변수 남발과 이로 인한 충돌은 유지 보수에 막대한 영향을 끼쳐서 개발자의 심신을 괴롭히기 때문이다. 스크립트의 모듈화는 이런 문제를 방지한다.

기본적인 모듈 패턴은 다음과 같다. return으로 외부에서 접근할 변수와 함수만 골라서 노출할 수 있으며, 외부에 노출할 필요 없는 변수와 함수는 클로저(Closure)를 이용하여, 전역 공간에 위치시키지 않고도 접근할 수 있다.

```javascript
var foo = (function () {  
    var i = 0;

    function init() {
        reset();
    }

    function reset() {
        i = 0;
    }

    function increase() {
        i++;
    }

    function decrease() {
        i--;
    }

    function get() {
      return i;
    }

    return {
        init: init,
        increase: increase,
        decrease: decrease,
        get: get
    };
}());

foo.increase();  
console.log(foo.get()); // 1  
foo.decrease();  
console.log(foo.get()); // 0

console.log(foo.i); // undefined  
foo.reset(); // Error  
```

위의 foo 모듈은 결과적으로 단순 객체를 반환하므로 싱글턴(Singleton)으로 볼 수 있는데, 이를 조금 응용하면 모듈을 일종의 클래스(Class)처럼 사용할 수도 있다.

```javascript
var Foo = (function () {  
    var NAME = 'Foo';

    // 생성자 함수
    function Foo() {
        this.i = 0;
    }

    Foo.prototype.getClassName = function () {
        return NAME;
    };

    Foo.prototype.increase = function () {
        this.i++;
    };

    Foo.prototype.decrease = function () {
        this.i--;
    };

    return Foo; 
}());

var foo = new Foo();  
```

## RequireJS

[RequireJS](http://requirejs.org/)는 AMD API 명세를 구현한 구현체 중 하나이다. 여기에 조금 더 편리하게 사용할 수 있도록 몇 가지 기능들을 추가했다. RequireJS의 자세한 사용법은 [http://requirejs.org/docs/api.html#usage](http://requirejs.org/docs/api.html)를 참고한다. 이 글에서는 실제 개발에 도움이 될만한 노하우를 설명한다.

### 모듈 정의와 사용

모듈을 정의하는 기본 형태는 다음과 같다.

```javascript
/* js/foo.js */
// 모듈 정의의 기본 형태
define([ // 의존 모듈들을 나열한다. 모듈이 한 개라도 배열로 넘겨야 한다.  
    'js/util',
    'js/Ajax',
    'js/Event'
], function (util, Ajax, Event) { // 의존 모듈들은 순서대로 매개변수에 담긴다.
    // 의존 모듈들이 모두 로딩 완료되면 이 함수를 실행한다.
    // 초기화 영역
    var i = 0;

    function increase() {
        i++;
    }

    function get() {
      return i;
    }

    // 외부에 노출할 함수들만 반환한다.
    return {
        increase: increase,
        get: get
    };
});

/* js/main.js */
require([  
    'js/foo'
], function (foo) {
    console.log(foo.get()); // 0
    foo.increase();
    console.log(foo.get()); // 1
});
```

모듈의 이름을 명시적으로 설정할 수도 있지만 이름 없는 모듈로 정의하는 것을 권장한다. 이름 없는 모듈은 호출될 때 모듈의 위치에 따라 이름을 결정한다. 개발할 때 파일의 이름이나 위치는 자주 변경되므로 유연한 상태로 둘 필요가 있다.

의존 모듈은 배열로 나열하긴 했지만 로딩 순서를 보장한다는 뜻은 아니다. 순서에 상관없이 병렬로 네트워크를 통해 다운로드되거나 브라우저의 캐시에서 꺼내진다. 어떤 모듈이 먼저 로딩되어 실행될지 모른다. 따라서 로딩 순서가 중요하다면 아래와 같이 require를 중첩해서 사용하는 방법이 있다.

```javascript
require(['js/first'], function (first) {  
    require(['js/second'], function (second) {
        //
    });    
});
```

모듈은 처음 호출할 때만 초기화된다. 모듈이 처음 호출되어 로딩 완료되면 모듈 정의 함수(위 코드에서는 두 번째 매개변수)를 실행하고 그 결과 값을 RequireJS 내부의 비밀 공간에 저장한다. 이후 어디에서건 같은 모듈을 호출할 때는 저장된 결과값을 반환하며, 모듈 정의 함수를 매번 실행하지 않는다. 그래서 모듈 정의 함수가 처음 생성한 클로저(Closure)로 초기화 영역 내의 변수, 함수들을 계속 사용할 수 있는 것이다. 즉 모듈의 상태는 유지된다. 위 예제의 foo 모듈을 사용하여 아래에서 확인해 보자.

```javascript
/* js/first.js */
define([  
    'js/foo'
], function (foo) {
    foo.increase();

    return {
        getFooValue: function () {
            return foo.get();
        }
    };
});

/* js/second.js */
define([  
    'js/foo'
], function (foo) {
    return {
        getFooValue: function () {
            return foo.get();
        }
    };
});

/* js/main.js */
require([  
    'js/first'
], function (first) {
    console.log(first.getFooValue()); // 1

    require([
        'js/second'
    ], function (second) {
        console.log(second.getFooValue()); // 1
    });
});
```

모든 모듈이 foo 모듈처럼 싱글턴(Singleton) 구현은 아닐 것이다. 다음과 같이 인스턴스 객체를 생성할 수 있는 클래스(Class) 형태의 구현도 필요하다.

```javascript
/* js/Layer.js */
define(function() {  
    function Layer(el) {
        this.el = el;
    }

    Layer.prototype.open = function () {
        //
    };

    Layer.prototype.close = function () {
        //
    };

    // 객체가 아닌 생성자 함수를 반환한다.
    return Layer;
});

/* js/main.js */
require([  
    'js/Layer'
], function (Layer) {
    var someLayer = new Layer(document.getElementById('some-layer'));
    someLayer.open();
});
```

모듈의 상태를 유지할 필요가 없다면 다음과 같이 객체 리터럴만으로 간단히 정의할 수도 있다.

```javascript
/* js/util.js */
define({  
    trim: function () {
        //
    },
    extend: funciton () {
        //
    }
});
```

### 설정 옵션

RequireJS는 여러 설정 옵션들을 제공한다. 대표적인 옵션은 다음과 같다.

```javascript
<script>  
    // RequireJS 설정 객체
    // require.js가 로딩되면 이 객체를 자동으로 읽어 들여 반영한다.
    var require = {
        // 모듈의 기본 위치를 지정한다.
        baseUrl: '/js/app', 

        // 모듈의 단축 경로 지정 또는 이름에 대한 별칭(Alias)을 지정할 수 있다.   
        paths: { 
            'lib': '../lib' // "/js/lib" 과 동일하다. baseUrl 기준
        },

        // AMD를 지원하지 않는 외부 라이브러리를 모듈로 사용할 수 있게 한다.
        shim: {
            'modernizr': { // Modernizr 라이브러리
                exports: 'Modernizr' 
            }
        },

        // 모듈 위치 URL뒤에 덧붙여질 쿼리를 설정한다.
        // 개발 환경에서는 브라우저 캐시를 회피하기 위해 사용할 수 있고, 
        // 실제 서비스 환경이라면 ts값을 배포한 시간으로 설정하여 새로 캐시하게 할 수 있다.
        urlArgs : 'ts=' + (new Date()).getTime()
    };
</script>  
<script src="/js/lib/require.js"></script>  
<script>  
    //
</script>  
```

참고로 위의 var require = {}; 설정은 require.js 파일의 로딩 전에 사용하는 방법이다. require.js 파일을 로딩한 후에는 require.config() 함수를 사용하여 설정할 수 있다.

### 모듈 위치

RequireJS는 호출하는 모듈의 위치를 찾을 때 baseUrl과 이름을 결합하여 찾는다. baseUrl이 "/js"이고 모듈 이름이 "common/util"이라면 모듈의 위치는 "/js/common/util.js"가 된다. baseUrl이 설정되어 있지 않다면 baseUrl은 현재 페이지의 경로가 된다. baseUrl이 유동적이라면 결국 모듈 위치도 유동적이 된다는 이야기이므로 특별한 경우가 아니라면 다음과 같이 baseUrl을 설정하는 편이 좋다.

```javascript
/* /index.html */
<script>  
    var require = {
        baseUrl: '/js/app'
    };
</script>  
<script src="/js/lib/require.js"></script>  
<script>  
  require([ 
      'common/relative',            // (1) 위치: "/js/app/common/relative.js"
      'dotjs.js',                   // (2) 위치: "/dotjs.js"
      '/js/lib/absolute.js',        // (3) 위치: "/js/lib/absolute.js"
      'http://another.com/foo.js'   // (4) 위치: "http://another.com/foo.js"
  ], function (relative, dotjs, absolute, foo) {
      //
  });
</script>  
```

(1)의 경우가 일반적인 사용이며, (2), (3), (4)는 특별한 경우가 아니면 사용할 일이 없지만 알아 둘 필요는 있다. (2)의 경우 baseUrl이 설정을 무시하고 현재 페이지의 경로를 사용한 결과이다. (3), (4)는 절대 경로로 이름을 지정한 경우이며 꼭 이름의 뒤에 '.js'를 붙여야 한다.

### 모듈이 아닌 외부 라이브러리 사용

AMD를 지원하지 않는 외부의 좋은 라이브러리를 사용하려면 paths와 shim 설정 옵션을 사용한다. 외부 라이브러리를 모듈처럼 사용할 수 있게 한다.

```javascript
// 설정
var require = {  
    paths: {
        // 이 설정으로 모듈 이름을 호출하면 값의 위치를 요청한다.
        // ".js"는 자동 추가
        'jquery': 'http://code.jquery.com/jquery-1.10.2',
        'modernizr': 'http://modernizr.com/downloads/modernizr-latest',
        'jindo': '/js/lib/jindo_component'
    },
    shim: {
        'modernizr': { 
            // Modernizr는 전역변수 "Modernizr"를 사용한다. 
            exports: 'Modernizr' 
        }
        'jindo': {
            deps: ['/js/lib/jindo.desktop.all.ns'],
            // Jindo(네임스페이스 버전)는 전역변수 "jindo"를 사용한다.
            exports: 'jindo' 
        }
    }
};

// 사용
require([  
    'jquery',
    'modernizr',
    'jindo'
], function (jquery, modernizr, jindo) {
    console.log(jquery); // (1) jQuery    
    console.log(modernizr); // (2) Modernizr
    console.log(jindo); // (3) Jindo
});
```

(1) jQuery의 경우 매개변수 jquery에 jQuery가 올바르게 담겨 있다. paths 설정만으로 이것이 가능한 이유는 jQuery가 사실 AMD를 지원하기 때문이다. jQuery 소스의 마지막 부분을 살펴보면 define으로 모듈을 반환하는 코드를 볼 수 있다. CommonJS 명세로 반환하는 부분도 존재한다. 따라서 jQuery는 전역변수, AMD, CommonJS를 동시에 지원하고 있다.

(2) Modernizr의 경우는 jQuery와 다르게 AMD를 지원하지 않기 때문에 paths 옵션 외에 shim 옵션이 필요하다. 외부 라이브러리가 사용하는 전역변수를 명시해서 RequireJS가 그 전역변수를 호출 반환의 매개변수로 넘겨 준다. 대부분의 외부 라이브러리는 이런 형식으로 설정하면 된다.

(3) Jindo의 경우는 약간 복잡하다. [Jindo 라이브러리](http://dev.naver.com/projects/jindo/)는 코어 라이브러리와 컴포넌트 라이브러리의 두 부분으로 나눌 수 있다(jQuery & jQuery UI와 비슷하다). 코어 라이브러리만 사용해도 되겠지만 유용한 컴포넌트들이 많으므로 보통 같이 사용한다. 그래서 모듈을 호출할 때 'jindo'만으로 코어와 컴포넌트를 모두 가져오도록 설정했다. 먼저 paths 설정을 살펴보면 코어 라이브러리가 아니라 컴포넌트 라이브러리가 지정되어 있다. 하지만 shim 옵션이 지정되어 있기 때문에, RequireJS가 '/js/lib/jindo_component.js'를 가져오기 전에 deps에 설정된 코어 파일인 '/js/lib/jindo.desktop.all.ns.js'를 먼저 가져오게 된다. 정리하면 "코어 파일 가져오기 -> 컴포넌트 파일 가져오기 -> 전역변수 jindo 추출"의 방식으로 동작한다. 여담으로, 컴포넌트 라이브러리의 경우 수십 개의 객체로 구성되어 있기 때문에 각 객체의 의존성을 파악하여 사용하는 개별 컴포넌트만 호출하도록 세세한 설정이 가능하지만 버전을 업데이트할 때마다 매번 새로 파악해야 하므로 적절하지 않다.

위의 세 가지 경우를 활용하면 대부분의 외부 라이브러리들은 사용할 수 있을 것이다.

사실 이렇게 옵션을 지정하지 않더라도 외부 라이브러리는 전역변수로 접근할 수 있기 때문에 다음과 같이 파일 URL을 호출해도 된다.

```javascript
require([  
    'http://code.jquery.com/jquery-1.10.2.js'
], function (dummy$) {
    console.log(dummy$); // undefined
    console.log($); // jQuery는 전역변수 "$"와 "jQuery"를 사용한다.
});
```

하지만 아름답지는 않은 것 같다.

### 텍스트 로딩과 HTML 템플릿 관리

RequireJS는 몇 가지 [플러그인](http://requirejs.org/docs/api.html)을 제공한다. 이 중 [text 플러그인](https://github.com/requirejs/text)은 JS 파일 외에도 CSS와 HTML같은 텍스트 파일도 불러올 수 있게 한다. 간단히 다음과 같이 사용한다.

```javascript
define([  
    'text!/template.html' // 플러그인은 플러그인 이름 뒤에 !를 붙인다.
], function (templateHTML) {
    //
});
```

이를 사용하면 현재 페이지에 노출되진 않지만 갖고 있어야 되는 HTML 조각들(Fragments)을 동적으로 관리할 수 있다.

```javascript
require([  
    'jquery',
    'text!/sections/layer1.html',
    'text!/sections/layer2.html',
    'text!/sections/layer3.html'
], function ($, layer1html, layer2html, layer3html) {
    var $body = $(document.body),
        $layer1 = $(layer1html),
        $layer2 = $(layer2html),
        $layer3 = $(layer3html);

    $body.append($layer1);
});
```

위 HTML에 해당하는 CSS 파일도 동적으로 가져올 수 있지만 CSS는 한 번만 정의하면 페이지에 적용할 수 있고 , 용량도 작은 편이기 때문에 전통적인 방법으로 관리하는 것이 더 합리적이다.

## 마치며

AMD는 다음과 같은 특징이 있다.

- 동적 로딩
- 의존성 관리
- 모듈화

이들 조합은 JavaScript 개발의 대안을 제시하고 있고, RequireJS 라이브러리는 이를 충실히 구현하고 있다.

## 참고 자료

- [AMD API 명세](https://github.com/amdjs/amdjs-api/wiki/AMD)
- [RequireJS 공식 웹사이트](http://requirejs.org/)