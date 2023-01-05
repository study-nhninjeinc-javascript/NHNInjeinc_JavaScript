## 13장 스코프

### 참고 사항
전역 변수와 지역 변수
전역변수란 자바스크립트에서 제일 바깥 범위(함수 안에 포함되지 않게)에 변수를 만드는 것을 말한다.
```
var x = 'global';
function test() {
    var x = 'local';
}
```
같은 x여도 test 함수 바깥의 x는 전역변수이고, test 함수 안의 x는 지역변수이다. 지역변수는 함수 안에 들어있는 변수를 의미한다.
전역변수는 window 객체의 속성이 된다.

### 스코프란?
모든 식별자(변수 이름, 함수 이름, 클래스 이름 등)는 자신이 선언된 위치에 의해 다른 코드가 식별자 자신을 참조할 수 있는 유효 범위가 결정된다. 이를 스코프라 한다. 즉, 스코프는 식별자가 유효한 범위를 말한다.
```
var x = 'global';
function foo() {
    var x = 'local';
    console.log(x); //local
}

foo();
console.log(x); //global
```
자바스크립트 엔진은 같은 두개의 변수 중에서 어떤 변수를 참조해야 할 것인지를 결정한다. 이를 식별자 결정이라고 하며, 자바스크립트 엔진은 스코프를 통해 어떤 변수를 참조해야 할 것인지를 결정한다.

```
var x = 'global';
function foo() {
    x = 'local';
    console.log(x); //local
}

foo();
console.log(x); //local
```
자바스크립트는 변수의 범위를 호출한 함수의 지역 스코프부터 전역 변수들이 있는 전역 스코프까지 넓혀가며 찾기 때문에 foo 함수 안에 x가 없으면 전역 스코프로 올라가서 변수를 찾게 된다.

스코프 체인
전역변수와 지역변수의 관계에서 스코프 체인이란 개념이 나온다. 내부 함수에서는 외부 함수의 변수에 접근 가능하지만 외부 함수에서는 내부 함수의 변수에 접근할 수 없다. 모든 함수들은 전역 객체에 접근할 수 있다.
```
var name = 'test';
function outer(){
    console.log('외부', name);
    function inner() {
        var firstName = 'ttt';
        console.log('내부', name);
    }
    inner();
}
outer();
console.log(firstName); //undefined
```
inner 함수는 name 변수를 찾기 위해 먼저 자기 자신의 스코프에서 찾고, 없으면 한 단계 올라가 outer 스코프에서 찾고, 없드면 더 올라가 전역 스코프에서 찾는다. 만약 전역 스코프에도 없다면 변수를 찾지 못한다는 에러가 발생한다. 이렇게 계속 범위를 넓히면서 찾는 관계를 스코프 체인이라고 한다.

렉시컬 스코핑
스코프는 함수를 호출할 때가 아니라 선언할 때 생긴다. 정적 스코프라고 불린다.
```
var name = 'test';
function log() {
    console.log(name);
}

function wrapper() {
    var name = 'change';
    log();
}
wrapper();
```
스코프는 함수를 선언할 떄 생기기 때문에 log 안의 name은 wrapper 안의 지역변수 name이 아니라, 전역변수 name을 가리키고 있다. 

