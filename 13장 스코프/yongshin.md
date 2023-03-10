### 13장 스코프

## 스코프란?

- 스코프란 식별자가 유효한 범위를 말한다.
- 모든 식별자는 자신이 선언된 위치에 의해 다른 코드가 식별자 자신을 참조할 수 있는 유효 범위가 결정된다.
- var , let , const 키워드로 선언한 변수는 스코프(유효범위)도 다르게 동작한다.


```jsx
function add(x,y) {
  // 매개변수는 함수 몸체 내부에서만 참조 할 수 있다.
  // 즉 , 매개변수의 스코프(유효범위)는 함수 몸체 내부다.
  console.log(x,y); // ?
  return x+y;
}

add(2, 5);

//매개변수는 함수 몸체 내부에서만 참조할 수 있다.
console.log(x,y); // ?
```

- 위와 같이 결과가 나오는 이유는 함수의 매개변수는 함수 몸체 내부에서만 참조할수있고 몸체외부에서 참조할 수 없다 ! --> 매개변수의 스코프가 함수 몸체 내부로 한정되있기 떄문!

```jsx
var var1 = 1; // 코드의 가장 바깥 영역에서 선언한 변수

if (true) {
   var var2 = 2; // 코드 블록(if) 내에서 선언한 변수
   if (true) {
      var var3 = 3; // 중첩된 코드 블록(if -> if) 내에서 선언한 변수
   }
}

function foo() {
   var var4 = 4; // 함수 내에서 선언한 변수
   
   function bar() {
      var var5 = 5; // 중첩된 함수 내에서 선언한 변수
}

console.log(var1); // ?
console.log(var2); // ?
console.log(var3); // ?
console.log(var4); // -> ?
console.log(var5); // -> ?
```
- 변수는 자신이 선언된 위치에 의해 유요한 범위가 달라지기 때문!

```jsx
var x = "global"; 

function foo() {
   var x = "local"; 
   
   console.log(x); // ??
}

foo(); 

console.log(x); // ??
```
- 자바스크립트 엔진은 이름이 같은 두 개의 변수 중에서 어떤 변수를 참조해야할지 결정을 하는데 이를 식별자 결정 이라 한다.
- 자바스크립트 엔진은 스코프를 통해 어떤변수를 참조해야할지 결정한다. = 스코프를 통해 식별자 결정을한다.
- 따라서 스코프는 자바스크립트 엔진이 식별자를 검색할 때 사용하는 규칙이라고도 한다.
- 자바스크립트 엔진은 코드 실행시 코드의 문맥을 고려한다!
<aside>
💡 코드가 어디서 실행되고 주변에 어떤코드가 있는지를 렉시컬 환경 이라고 부르고 코드의 문맥은 렉시컬 환경에서 이루어지고 이를 구현한것을 실행 컨텍스트라고 한다. 모든 코드는 실행 컨텍스트에서 평가되고 실행이된다!
</aside>

<img src="https://blog.kakaocdn.net/dn/cn0zjT/btqZoydJdrc/vHquKk6Vb0mf4PaExIsZX1/img.png">

- foo 함수는 함수 스코프 이므로 foo 함수 안에서 참조를 할수 있고 , 바깥 영역에 선언된 x 변수는 전역 스코프 이므로 어디서든 참조가 가능하다! , 즉 식별자 이름은 동일하지만 유효한 범위 (스코프) 가 다르기 때문에 별개의 변수이다.
- 식별자는 어떤 값을 구별할 수 있어야 하므로 유일 해야 되고 이름은 중복될수 없다. 그러므로 하나의 값은 유일한 식별자에 연결되어야한다!
- 스코프는 네임스페이스다 -> 프로그래밍 언어에서는 스코프(유효 범위)를 통해 식별자인 변수 이름의 충돌을 방지해 같은 이름의 변수를 사용할수 있게한다.  즉 스코프 내에서 식별자는 유일해야 하지만 다른스코프에는 같은 이름의 식별자를 사용할 수 있게한다.

```jsx
// var 키워드는 같은 스코프 내에서 중복 선언이 허용된다. 대신 변수값이 재할당되어 변경되는 부작용을 발생
function foo(){
   var x = 1;
   // var 키워드로 선언된 변수는 같은 스코프 내에서 중복 선언을 허용한다!
   // 아래 변수 선언문은 자바스크립트 엔진에 의해 var 키워드가 없는 것처럼 동작한다.
   var x = 2; 
   console.log(x);
}

foo() // ??

// let, const 키워드는 같은 스코프 내에서 중복 선언을 허용하지 않는다.
function bar() {
   let x = 1;
   // let , const 키워드로 선언된 변수는 같은 스코프 내에서 중복 선언 허용 X !
   let x = 2; // ?? error : Identifier 'x' has already been declared
}

bar();
```

## 스코프의 종류
- 스코프는 크게 전역 스코프와 지역 스코프로 나눌수 있다.
### 전역 스코프
- 코드의 가장 바깥 영역부분이다.
- 전역 스코프를 갖는 변수를 전역변수라 부른다.
- 전역 변수는 어디서든지 참조할 수 있다.
### 지역 스코프
- 함수 몸체 내부의 영역부분이다.
- 지역 스코프를 갖는 변수는 지역 변수라 한다.
- 지역 변수는 자신의 지역 스코프와 하위 지역 스코프에서 유효하다.
```jsx
var x = "global x";
var y = "global y";

function outer() {
   var z = "outer's local z";

   console.log(x); // ??
   console.log(y); // ??
   console.log(z); // ??
   
   function inner() {
      var x = 'inner local x';

     console.log(x); // ??
     console.log(y); // ??
     console.log(z); // ??
   }

   inner();
}

outer();
console.log(x); // ??
console.log(z); // ??
```
## 스코프 체인
- 함수는 전역 , 함수 몸체 내부의서 정의할수있고 함수 몸체 내부에서 정의된 함수를 함수의 중첩(중첩 함수)이라 하고 중첩 함수를 포함하는 함수를 '외부 함수' 라고 한다.
- 함수는 중첩될 수 있으므로 스코프도 중첩이 될수 있고 , 스코프가 함수의 중첩에 의해 계층적 구조를 갖는걸 의미한다. => 중첩 함수의 지역 스코프는 중첩 함수를 포함하는 외부함수의 지역 스코프와 계층적 구조를 갖는데 이때 외부 함수의 지역 스코프를 중첩 함수의 상위 스코프라 한다!
- 스코프 체인이란 스코프가 계층적으로 연결된것을 말한다.
- 변수를 참조시 자바스크립트 엔진은 스코프 체인을 통해 변수를 참조하는 코드의 스코프에서 시작하여 상위 스코프 방향으로 이동해 선언된 변수를 검색한다. 이를통해 상위스코프에 선언한 변수를 하위 스코프에도 사용가능하다.

## 스코프 체인에 의한 함수 검색

```jsx
function foo() {
   console.log('global function foo');
}

function bar() {
   // 중첩 함수
   function foo() {
      console.log('local function foo');
   }

   foo();
}

bar();
```
- 함수 선언문으로 함수 정의시 런타임 이전에 함수 객체가 생성되고 자바스크립트 엔진은 함수 이름과 동일한 이름의 식별자를 암묵적으로 선언하고 생성된 함수 객체를 할당합니다.
- 이를 통해 함수도 식별자에 할당하기때문에 스코프를 갖는데 이를 보면 함수는 식별자에 함수 객체를 할당할것 외에 일반 변수와 다를것 없다는걸 보여준다.

## 함수 레벨 스코프
- 코드 블록이 아닌  함수에 의해서만 지역 스코프가 생성된다.
-  C나 자바 등 프로그래밍 언어는 모든 코드 블록(if, for, while, try/catch등)이 지역 스코프를 만들고 이러한 특성을 블록 레벨 스코프라 하고
-  var 키워드로 선언된 변수는 오로지 함수의 코드 블록(함수 몸체)만을 지역 스코프로 인정하고 이를 함수 레벨 스코프라 한다.

```jsx
var x = 1;

if (true) {
   // var 키워드로 선언된 변수는 함수 코드 블록(함수 몸체)만을 지역스코프로 인정
  // 함수 밖에서 var 키워드로 선언된 변수는 코드 블록 내에서 선언되었다 해도 모두 전역변수이다.
   // 따라서 이미 선언된 전역변수에 중복 선언을해 변수 값이 변경되는 부작용을 발생 시킨다.
   var x = 10;
}

console.log(x); // ?
```
- var 키워드로 선언된 변수는 오로지 함수의 코드 블록만을 지역 스코프로 인정하고 , ES6에 도입된 let , const 키워드는 블록 레벨 스코프를 지원한다.

## 렉시컬 스코프

``jsx
  var x = 1;

  function foo() {
    var x = 10;
    bar();
  }

  function bar() {
    console.log(x);
  }

  foo(); // ?
  bar(); // ?
```
- 함수를 어디서 호출했는지에 따라 함수의 상위 스코프 결정
- 함수를 어디서 정의 했는지에 따라 함수의 상위 스코프 결정
- 첫번째 방식은 bar 함수의 상위 스코프는 foo 함수의 지역스코프와 전역 스코프이다.
- 첫번쨰와 같이 함수를 정의하는 시점에는 함수가 어디서 호출될지 알수없어 함수가 호출되는 시점에 동적으로 상위 스코프를 정하는걸 동적 스코프라한다.
- 두번쨰 방식은 bar의 상의 스코프는 전역변수이다.
- 두번째 방식같이 상위 스코프가 동적으로 변하지 않고 함수 정의가 평가되는 시점에서 상위 스코프가 정적으로 결정되기떄문에 정적 스코프 또는 렉시컬 스코프라고 부른다.
- 자바스크립트는 렉시컬 스코프를 따르므로 함수를 어디서 호출했는지가 아니라 함수를 어디서 정의했는지에 따라 상위 스코프를 결정한다.
- 함수가 호출된 위치는 상위 스코프에 결정에 어떠한 영향도 주지않는데 즉 함수의 상위 스코프는 언제나 자신이 정의된 스코프를 의미한다.