# 21. 빌트인 객체

> 자바스크립트 실행 환경과 관계없이 언제나 별도의 선언 없이 전역 변수처럼 참조할 수 있다.
> 

<aside>
💡 호스트 객체는 자바스크립트 실행 환경에 따라 추가로 제공하는 객체를 말한다.

</aside>

```jsx
// 표준 빌트인 객체인 String 생성자 함수로 String 객체를 생성
const strObj = new String('Lee'); // String {"Lee"}

// 생성자 함수인 표준 빌트인 객체가 생성한 인스턴스의 프로토타입은 표준 빌트인 객체의 prototype 프로퍼티에 바인딩된 객체다.
console.log(Object.getPorotypeOf(strObj) === String.prototype); // true

// String.prototype 의 빌트인 메서드
console.log(strObj.toUpperCase()); // LEE

// 원시값인 문자열
let str = 'Lee';

// 원시값에 대해 마치 객체처럼 동작한다.
console.log(str.toUpperCase()); // LEE
```

## 래퍼 객체(Wrapper object)

```jsx
const str = 'im string';

console.log(typeof str); // string

// 원시 타입인 문자열이 래퍼 객체인 String 인스턴스로 변환된다.
console.log(str.toUpperCase()); // IM STRING

console.log(typeof str); // string

// 래퍼 객체의 프로퍼티를 동적으로 추가
str.name = str.toUpperCase();

// 프로퍼티를 추가한 래퍼 객체는 이미 가비지 컬렉션의 대상이기 때문에 
// 아래 소스에서 생성되는 래퍼 객체와는 다른 객체다..
console.log(str.name); // undefined
```

- 원시 값에 대해 객체처럼 접근하면 생성되는 임시 객체 래퍼 객체는 접근하는 순간에만 생성되어 해당 생성자 함수의 prototype 메서드를 상속받아 사용한 뒤 래퍼 객체의 처리가 종료되면 가비지 컬렉션의 대상이 된다.

<aside>
💡 가비지 컬렉션의 대상이 된다라는 것은 가비지 컬렉터가 해당 객체를 삭제한다 라는 것을 의미한다. 가비지 컬렉터는 자바스크립트 엔진 내에서 끊임없이 동작하면서 객체를 모니터링하고 참조하지 않는 객체를 삭제한다.

</aside>

![str.toUpperCase 메서드에 접근한 시점에 생성되는 문자열 래퍼 객체](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/689c1e0f-1268-4c99-afd0-316330f762b3/21-1.png)

str.toUpperCase 메서드에 접근한 시점에 생성되는 문자열 래퍼 객체

## 전역 객체(Global object)

> 코드가 실행되기 이전 단계에 자바스크립트 엔진에 의해 어떤 객체보다도 먼저 생성되는 특수한 객체를 말한다. → 최상위 객체이다.
> 

<aside>
💡 전역 객체는 자바스크립트 환경에 따라 지칭하는 이름이 제각각이다. 브라우저 환경에서는 window, Node.js 환경에서는 global이 전역 객체를 가리킨다.
ES11 에서 도입된 globalThis 는 서로 다른 환경에서 전역 객체를 가리키던 다양한 식별자를 통일한 식별자이다.

</aside>

- 전역 객체는 표준 빌트인 객체와 환경에 따른 호스트 객체, 그리고 var 키워드로 선언한 전역 변수와 전역 함수를 프로퍼티로 갖는다.
    - 표준 빌트인 객체와 호스트 객체가 전역 객체를 상속 받는 개념이 아닌 전역 객체가 표준 빌트인 객체와 호스트 객체 등을 프로퍼티로 소유한 것이다.
- 전역 객체는 개발자가 의도적으로 생성할 수 없다. (생성자 함수가 제공되지 않는다.

```jsx
var globalVar = 1;
console.log(globalThis.globalVar); // 1

// 암묵적 전역에 의해 선언된 변수는 전역 객체의 프로퍼티다.
bar = 2;
console.log(globalThis.bar); // 2

// 전역 함수도 전역 객체의 프로퍼티가 된다. (메서드가 된다고 해야하나?)
function globalFunc() { return 3; }
console.log(globalThis.globalFunc()); // 3

// let, const 키워드로 선언한 전역 변수는 전역 객체의 프로퍼티가 되지 않는다.
let globalLet = 4;
console.log(globalThis.globalLet); // undefined
```

```jsx
// 문자열을 16진수로 해석하여 10진수로 변환한 값을 반환하는 메서드 parseInt
console.log(globalThis.parseInt('F', 16)); // 15

// 전역 객체의 프로퍼티는 globalThis, window, global  등을 생략할 수 있다.
console.log(parseInt('F', 16)); // 15
```

### 빌트인 전역 프로퍼티

> 전역 객체의 프로퍼티를 의미하며 주로 애플리케이션 전역에서 사용하는 값을 제공한다.
> 

```jsx
// 무한대를 나타내는 숫자값
console.log(globalThis.Infinity); // Infinity
console.log(3/0); // Infinity
console.log(-3/0); // -Infinity
console.log(typeof Infinity); // number

// 숫자가 아님을 나타내는 숫자값 NaN
console.log(globalThis.NaN); // NaN
console.log(Number('Im number')); // NaN
console.log(typeof NaN); // number

// 원시 타입인 undefined
console.log(globalThis.undefined); // undefined
var undefinedVar;
console.log(undefinedVar); // undefined
console.log(typeof undefined); // undefined
```

### 빌트인 전역 함수

### eval

<aside>
💡 eval 함수를 통해 사용자로부터 입력받은 콘텐츠를 실행하는 것은 보안에 취약하고 자바스크립트 엔진에 의해 최적화가 수행되지 않으므로 일반적인 코드보다 처리속도가 느리다.

</aside>

- 자바스크립트 코드를 나타내는 문자열을 인수로 전달받아 표현식이라면 문자열 코드를 런타임에 평가하여 값을 생성한다.
    - 표현식이 아닌 문이라면 문자열 코드를 런타임에 실행한다. 또한 여러 개의 문으로 이루어져 있다면 모든 문을 실행한다.

```jsx
// 표현식인 경우
eval('1 + 2;'); // 3

// 표현식이 아닌 경우
eval('var y = 5;'); // undefined

// 객체 리터럴과 함수 리터럴은 반드시 괄호로 둘러싸야 한다.
const evalObj = eval('({ a: 1 })');
const evalFunc = eval('(function() { return 1; })');

// 여러 문으로 이루어진 경우 모든 문을 실행하고 마지막 결과값을 반환한다.
eval('1 + 2; 3 + 4;'); // 7

// ----------------------------------------- //

// eval 함수는 자신이 호출된 위치에 해당하는 스코프를 런타임에 동적으로 수정한다.
const x = 1;

function foo() {
	// foo 스코프가 평가되고 나서 eval 함수가 동적으로 스코프를 수정한다.
	eval('var x = 2;');

	console.log(x); // 2
}
foo();

console.log(x);

// 그러나 엄격 모드에서는 기존 스코프를 수정하지 않고 함수 자신의 자체적인 스코프를 생성한다.
function strictFunc() {
	'use strict';

	eval('var x = 2; console.log(x);'); // 2
	eval('console.log(x)'); // 1 -> eval 스코프는 개별이다.
	console.log(x); // 1
}
strictFunc();

// 인수로 전달받은 문자열 코드가 let, const 키워드를 사용하는 경우 암묵적으로 엄격 모드가 적용된다.
function letEval() {
	eval('const x = 2; console.log(x);'); // 2

	console.log(x); // 1
}
letEval();
```

### isFinite

> 전달받은 인수가 정상적인 유한수인지 검사하는 함수 (무한수인 경우 false)
> 

```jsx
console.log(isFinite(0)); // true

// 인수의 타입이 숫자가 아닌경우 숫자로 타입을 변환한 후 검사 -> 인수가 NaN 으로 평가되는 값이면 false
console.log(isFinite('10')); // true ( '10' -> 10 )

console.log(isFinite(null)); // true ( null -> 0 )

console.log(isFinite(Infinity)); // false

console.log(isFinite(undefined)); // false ( undefined 는 undefined 타입이라서 그런 것 같다. )
```

### isNaN

> 전달받은 인수가 NaN 인지 검사하여 결과를 반환한다.
> 

```jsx
console.log(isNaN(NaN)); // true

console.log(isNaN(10)); // false

// 전달받은 인수의 타입이 숫자가 아닌 경우 숫자로 타입을 변환 후 검사
console.log(isNaN('i am NaN')); // true -> 'i am NaN' => NaN

console.log(isNaN('10')); // false -> '10' => 10

console.log(isNaN('')); // false -> '' => 0
```

### parseFloat

> 전달받은 문자열 인수를 부동 소수점 숫자 (실수) 로 해석하여 반환
> 

```jsx
console.log(parseFloat('3.14')); // 3.14

console.log(parseFloat('10.00')); // 10

// 문자열을 숫자로 변환할 수 없다면 NaN을 반환한다.
console.log(parseFloat('Hi')); // NaN

// 앞 뒤 공백은 무시된다.
console.log(parseFloat(' 1 ')); // 1
```

### parseInt

> 전달받은 문자열 인수를 정수로 해석하여 반환한다.
> 

```jsx
console.log(parseInt('10')); // 10

console.log(parseInt('10.1')); // 10

// 두 번째 인수로 진법을 나타내는 기수를 전달할 수 있다.
console.log(parseInt('10', 2)); // 2진수로 해석한 결과를 10진수 정수로 반환 -> 2

// 16진수 리터럴은 사용 가능 (0x)
console.log(parseInt('0xf')); // 15

// 2진수 리터럴(0b)과 8진수 리터럴(0o)은 제대로 해석하지 못한다.
console.log(parseInt('0b10')); // 0
console.log(parseInt('0o10')); // 0
```

### encodeURI / decodeURI

> URI 를 문자열로 전달받아 이스케이프 처리를 위해 인코딩한다.
> 

![21-2.PNG](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/45a513e8-63dc-4b6c-8907-f2d1fc450e85/21-2.png)

```jsx
const uri = 'https://www.thisisdomain.com/thisispath?thisisquerystring=쿼리스트링값#thisisfragment';

const encUri = encodeURI(uri);
console.log(encUri); // https://www.thisisdomain.com/thisispath?thisisquerystring=%EC%BF%BC%EB%A6%AC%EC%8A%A4%ED%8A%B8%EB%A7%81%EA%B0%92#thisisfragment

// decodeURI 는 인코딩된 URI를 이스케이프 처리 이전으로 디코딩한다.
const decUri = decodeURI(encUri); // https://www.thisisdomain.com/thisispath?thisisquerystring=쿼리스트링값#thisisfragment
console.log(decUri);
```

### encodeURIComponent / decodeURIComponent

> URI 구성 요소를 인수로 전달받아 인코딩된 값을 반환
> 

```jsx
const uriComp = 'name=하늘&job=programmer';

let encComp = encodeURIComponent(uriComp);
console.log(encComp);
// name%3D%ED%95%98%EB%8A%98%26job%3Dprogrammer
// 인수로 전달받은 문자열을 URI의 구성요소인 쿼리스트링의 일부로 간주한다.
// -> 쿼리 스트링 구분자로 사용되는 =, ?, & 까지 인코딩됨

let decComp = decodeURIComponent(encComp);
console.log(decComp);
// name=하늘&job=programmer
```

### 암묵적 전역

```jsx
var x = 10; // 전역 변수

function yFunc() {
	y = 20; // 선언하지 않은 식별자에 값을 할당 not error
	// 자바스크립트 엔진이 window.y = 20 으로 해석한다. 즉 전역 객체의 프로퍼티로 추가된다.
}

yFunc();

// 선언하지 않은 식별자 y를 전역에서 참조
console.log(x + y); // 30
```