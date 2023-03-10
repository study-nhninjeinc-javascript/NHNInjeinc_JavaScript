# subin

# 14 전역 변수의 문제점

- 전역 변수의 무분별한 사용은 위험하다.
- 전역 변수를 반드시 사용해야 할 이유를 찾지 못한다면 지역 변수를 사용해야 한다.

## 14.1 변수의 생명 주기

### 14.1.1 지역 변수의 생명 주기

- 변수는 선언에 의해 생성되고 할당을 통해 값을 갖는다. - 그리고 언젠가 소멸한다.
- 즉 변수는 생성되고 소멸되는 생명 주기가 있다.
    - 변수에 생명 주기가 없다면 한번 선언된 변수는 프로그램을 종료하지 않는 한 영원히 메모리 공간을 점유하게 된다. - 메모리 부하가 쉽게 올 수 있다는 뜻?
- 변수는 자신이 선언된 위치에서 생성되고 소멸한다.
    - 함수 내부에서 선언된 지역 변수는 함수가 호출되면 생성되고 함수가 종료하면 소멸한다.

<aside>
💡 function foo() {
    var x = ‘local’;
    console.log(x);    // local
    return x;
}

foo();    
console.log(x);    // ReferenceError: x is not defined

- 지역 변수 x는 foo함수가 호출되기 이전까지는 생성 되지 않는다.
- 즉 함수 선언 시에 변수 선언문이 실행되는 것이 아닌 함수가 호출 되어야 변수가 선언된다는 뜻 ?
</aside>

- 변수 선언은 선언문이 어디에 있든 상관없이 가장 먼저 실행된다.
- 다시 말해 변수 선언은 코드가 한 줄씩 순차적으로 실행되는 시점인 런타임에 실행되는 것이 아니라 런타임 이전 단계에서 자바스크립트 엔진에 의해 먼저 실행된다.
    - **중요** 위 설명은 전역 변수에 한정된 것
    - 함수 내부에서 선언한 변수는 함수가 호출된 직후에 함수 몸체의 코드가 한 줄씩 순차적으로 실행되기 전에 자바스크립트 엔진에 의해 먼저 실행된다.
        - 즉 위의 예제를 봤을 때 foo() 함수가 호출 될 시 foo() 함수 안의 코드가 순차적으로 호출되는 것이 아닌 x 변수가 선언되고 undefined로 초기화 되는게 가장 우선적이다.
        - 그 후 순차적으로 실행된다.
    - 함수가 종료되면 함수 내부의 변수들도 소멸되어 생명 주기가 종료된다.
        - 즉 지역 변수의 생명 주기는 함수의 생명 주기와 일치한다.

![14-1.png](subin%2030056e7737f54ea8a015b5de3d2bbec7/14-1.png)

- 함수 몸체 내부에서 선언된 지역변수의 생명 주기는 함수의 생명 주기와 대부분 일치하지만 지역 변수가 함수보다 오래 생존하는 경우도 있다.
    - 변수의 생명 주기는 메모리 공간이 확보된 시점부터 메모리 공간이 해제되어 가용 메모리 풀에 반환되는 시점까지다.
    - 함수 내부에 선언된 지역변수는 함수가 생성한 스코프에 등록된다.
        - 변수는 자신이 등록한 스코프가 소멸(메모리 해제)될 때까지 유효하다
        - 누군가 스코프를 참조하고 있으면 스코프는소멸하지 않고 생존하게 된다.(메모리와 동일)
        - 일반적으로 함수가 종료하면 함수가 생성한 스코프도 소멸하지만 누군가 해당 스코프를 참조할 시 해당 스코프는 해제되지 않고 생존하게 되는데 이는 24장 **클로저에서 자세히 알아볼 것!!!**

 ****

<aside>
💡 var x = 'global';

function foo() {
// 호이스팅은 스코프 단위로 동작
// 지역 변수 x를 참조해 출력 -> 변수 할당문이 실행되기 이전에 undefined 값을 갖음
console.log(x); // undefined
var x = 'local';
}
foo();
console.log(x); // global

// 즉 호이스팅은 변수 선언이 스코프를 선두로 끌어 올려진 것처럼 동작하는 자바스크립트 고유의 특징을 말한다.

</aside>

### 14.1.2 전역 변수의 생명 주기

- 함수와 달리 전역 코드는 명시적인 호출 없이 실행된다.
- 즉 코드가 로드되자마자 곧바로 해석되고 실행된다.
- 전역 변수는 반환문이 없기 때문에 마지막 문이 실행되어 더 이상 실행할 문이 없을때 종료된다.
- var 키워드로 선언한 전역 변수는 전역 객체 프로퍼티가 된다.
    - 이는 전역 변수의 생명주기가 전역 객체의 생명 주기와 일치한다는 것을 말한다.
        - 예제) 브라우저 환경일 경우 전역객체는 Window(node.js = global)이며 전역객체 window는 웹페이지를 닫기 전까지 유효하다.

![14-2.jpeg](subin%2030056e7737f54ea8a015b5de3d2bbec7/14-2.jpeg)

## 14.2 전역 변수의 문제점

1. 암묵적 결합
    - 전역 변수를 선언한 의도는 전역, 즉 코드 어디서든 참조하고 할당할 수 있는 변수를 사용하겠다는 것
    - 모든 코드가 전역 변수를 참조하고 변경할 수 있는 암묵적 결합을 허용하는 것
    - 변수의 유효범위가 크면 클수록 코드의 가독성이 나빠지고 의도치않게 상태가 변경될 수 있는 위험성이 높아진다.
2. 긴 생명 주기
    - 전역 변수는 생명 주기가 길다. - 메모리 리소스도 오랜시간 소비한다.
    - 지역 변수는 전역 변수보다 생명주기가 훨씬 짧다.
        - 상태를 변경할 수 있는 시간도 짧고 기회도 적다.
        - 전역변수보다 상태 변경에 의한 오류가 발생할 확률이 작다는 것을 의미한다.
3. 스코프 체인 상에서 종점에 존재
    - 전역 변수는 스코프 체인 상에서 종점에 존재한다.
    - 변수를 검색할 때 전역 변수가 가장 마지막에 검색된다.
        - 즉 전역 변수의 검색 속도가 가장 느리다
        - 차이는 크지 않지만 속도의 차이는 분명히 있다.
4. 네임스페이스 오염
    - 자바스크립트 문제 중 가장 큰 문제점 중 하나는 파일이 분리되어 있다 해도 하나의 전역 스코프를 공유한다는 것이다.
    - 따라서 다른 파일 내에서 동일한 이름으로 명명된 전역 변수나 전역 함수 같은 스코프 내에 존재할 경우 예상치 못한 결과를 가져올수 있다.
    - **(참고를 위해 예제 만들 것 !!!!)**

## 14.3 전역 변수의 사용을 억제하는 방법

- 전역 변수를 반드시 사용해야 할 이유를 찾지 못한다면 지역변수를 사용해야 한다.
- 변수의 스코프는 좁을수록 좋다. - 효율적인 코드
- 전역 변수를 절대 사용하지 말라는 것이 아닌 무분별한 남발을 억제해야 한다.

### 14.3.1 즉시 실행 함수

- 함수 정의와 동시에 호출되는 즉시 실행 함수는 단 한번만 호출된다.
- 모든 코드를 즉시 실행 함수로 감싸면 모든 변수는 즉시 실행 함수의 지역변수가 된다.

<aside>
💡 (function () {
    var foo = 10;    // 즉시 실행 함수의 지역 변수
    // …
}());

console.log(foo);    // ReferenceError: foo is not defined

</aside>

- 전역 변수를 생성하지 않으므로 라이브러리등에 자주 사용

### 14.3.2 네임스페이스 객체

- 전역에 네임스페이스 역할을 담당할 객체를 생성하고 전역 변수처럼 사용하고 싶은 변수를 프로퍼티로 추가하는 방법

<aside>
💡 var MYAPP = {};    // 전역 네임스페이스 객체

MYAPP.person = {
    name: ‘lee’,
    address: ‘seoul’
};

console.log(MYAPP.person.name);    // lee

</aside>

- 네임 스페이스를 분리해서 식별자 충돌을 방지하는 효과는 있으나 네임 스페이스 객체 자체가 전역 변수에 할당되므로 그다지 유용해 보이지는 않는다.

### 14.3.3 모듈 패턴

- 클래스를 모방해서 관련이 있는 변수와 함수를 모아 즉시 실행 함수로 감싸 하나의 모듈을 만든다.
- 자바스크립트의 강력한 기능인 클로저를 기반으로 동작한다.
- 모듈 패턴의 특징은 전역 변수의 억제는 물론 캡슐화까지 구현할 수 있다는 것이다.
    - 캡슐화는 객체의 상태를 나타내는 프로퍼티와 프로퍼티를 참조하고 조작할 수 있는 동작 인 메서드를 하나로 묶는 것을 말한다.
    - 캡슐화는 객체의 특정 프로퍼티나 메서드를 감출 목적으로 사용하기도 하는데 이를 정보 은닉이라고 한다.
- 대부분의 객체지향 프로그래밍 언어는 클래스를 구성하는 맴버에 대해 public, private, protected등의 접근 제한자를 사용해 공개 범위를 한정할 수 있다. - 자바스크립트는 접근 제한자를 제공하지 않는다.
    - 클래스 외부에는 제한된 접근 권한을 제공하며 원하지 않는 외부의 접근으로부터 내부를 보호하는 기능을 한다.
    - **모듈 패턴은 전역 네임스페이스의 오염을 막는 기능은 물론 한정적이기는 하지만 정보 은닉을 구현하기 위해 사용                    - ※ 왜 한정적일까???**

<aside>
💡 var Counter = (function () {
    // private 변수
    var num = 0;

    // 외부로 공개할 데이터나 메서드를 프로퍼티로 추가한 객체를 반환한다.
    return {
        increase() {
            return ++ num;
        },
        decrease() {
            return — num;
        }
    };
}());

// private 변수는 외부로 노출되지 않는다.
console.log(Counter.num);    // undefined

console.log(Counter.increase());    // 1
console.log(Counter.increase());    // 2
console.log(Counter.decrease());    // 1
console.log(Counter.decrease());    // 0

</aside>

- 위 예제는 즉시 실행 함수는 객체를 반환한다.
- 이 객체에는 외부에 노출하고 싶은 변수나 함수를 담아 반환한다.
- 이때 반환되는 객체의 프로퍼티는 외부에 노출되는 퍼블릭 맴버다.
- 외부로 노출하고 싶지 않은 변수나 함수는 반환하는 객체에 추가하지 않으면 외부에서 접근할 수 없는 프라이빗 맴버가 된다.
- 자세히는 24장 클로저에서 살펴보기로 하자.

### 14.3.4 ES6 모듈

- ES6 모듈은 파일 자체의 독자적인 모듈스코프를 제공한다.
- 모듈 내에서 var 키워드로 선언한 변수는 더는 전역 변수가 아니며 window 객체의 프로퍼티도 아니다.                    - ※ 그럼 해당 키워드로 선언한 변수를 무엇이라고 부르는가? 지역변수?
- 해당 script 태그에 type=”module”를 추가하고 파일 확장자를 mjs로 권장한다.
- ES6 모듈은 IE를 포함한 구형 브라우저에서는 동작하지 않는다.
- 브라우저의 ES6 모듈 기능을 사용하더라도 트랜스파일링이나 번들링이 필요하기 때문에 아직까지는 브라우저가 지원하는 ES6 모듈 기능보다는 Webpack등의 모듈 번들러를 사용하는 것이 일반적이다.