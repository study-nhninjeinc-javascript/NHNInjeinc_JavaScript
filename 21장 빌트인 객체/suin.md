## 21.1 자바스크립트 객체의 분류

자바스크립트 객체의 종류

- 표준 빌트인 객체
ECMAScript 사양에 정의된 객체
애플리케이션 전역의 공통기능을 제공
자바스크립트 실행환경 관계없이 언제나 사용할 수 있다
전역객체의 프로퍼티로서 제공되어 별도의 선언 없이 전역변수처럼 언제나 참조할수있다.
- 호스트 객체
ECMAScript 사양에 정의되어 있진 않음
자바스크립트 실행황경에서 추가로 제공하는 객체(node.js 브라우저환경..)
- 사용자정의 객체
표준 빌트인 객체와 호스트 객체처럼 기본제공되는 객체가 아닌 사용자가 직접 정의한 객체

## 21.2 표준 빌트인 객체

- Object, String, Number, Boolean, … 등 40여개의 빌트인 객체를 제공한다
- Math, Reflect, JSON을 제외한 표준 빌트인 객체는 모두 인스턴스를 생성할 수 있는 생성자 함수객체
생성자 함수 객체인 표준 빌트인 객체는 프로토타입 메서드와 정적 메서드를 제공하고
생성자 함수객체가 아닌 표준 빌트인 객체는 정적 메서드만 제공한다.
- 생성자 함수 객체인 표준 빌트인 객체가 생성한 인스턴스프로토 타입은 표준 빌트인 객체의 prototype 프로퍼티에 바인딩된 객체이다.
- 표준 빌트인 객체의 prototype 프로퍼티에 바인딩된 객체는 다양한 기능의 프로토타입 메서드를 제공, 표준 빌트인 객체는 인스턴스없이도 호출가능한 빌트인 정적 메서드를 제공

```jsx
const strObj = new String('Suin');//String{"Suin"}
console.log(typeof strObj);//object

const numObj = new Number(917);//Number{917}
console.log(typeof numObj );//object

//strObj객체의 프로토타입은 String.prototype이다
console.log(Object.getPrototypeOf(strObj) === String.prototype);//true

const numObj2 = new Number(91.7);//Number{91.7}
//반올림
console.log(numObj2.toFixed());//92
//정수인지 아닌지
console.log(numObj2.isInteger());//false
```

## 21.3 원시값과 래퍼 객체

```jsx
const str = 'hello';

//원시타입인 문자열이 프로퍼티와 메서드를 갖고 있는 객체처럼 동작한다.
console.log(str.length);
console.log(str.toUpperCase());//HELLO
```

- 원시값에 대해 마치 맥체처럼 마침표 표기법으로 접근하면 자바스크립트 엔진이 일시적으로 원시값을 연관된 객체로 변환해준다.
→ 자바스크립트 엔진이 암묵적으로 연관된 객체를 생성하여 생성된 객체로 프로퍼티에 접근하거나 메서드를 호출하고 다시 원시값으로 되돌린다.
- 래퍼객체 : 문자열 숫자 불리언 값에 대해 객체처럼 접근하면 생성되는 임시 객체
- ES6에서 새롭게 도입된 원시값인 심벌도 래퍼객체를 생성
일반적인 원시값과는 달리 리터럴 표기법으로 생성할수없고 Symbol함수를 통해 생성해야해서 차이가 있다. - 33장에서 알아보자
- null, undefined는 래퍼객체를 생성하지 않는다. 없는 값인 거니까? 활용할 구석이 없으니까?

```jsx
//str은 문자열
const str = 'hello';

//암묵적으로 생성된 래퍼 객체를 가진다
//식별자 str의 값dms [[StringData]] 내부슬롯에 할당된다.
//래퍼객체에 프로퍼티가 추가됨
str.name = 'Lee'

//다시 원래의 문자열로 갖는다.
//레퍼객체는 가비지 컬렉션 대상이된다.
//새롭게 생성된 래퍼객체를 가리킴.. 새로 만들어서 name프로퍼티는 없음
console.log(str.name);//undefined

//다시 원래의 문자열로
//래퍼객체는 가비지 컬렉션대상으로 된다
console.log(typeof str, str);//string hello
```

## 21.4 전역객체

- 전역객체 : 코드가 실행되기 어전 단계에서 자바스크립트 엔진에 의해 어떤 객체보다도 먼저 생성되는 특수한 객체 - 어떠한 객체에도 속하지 않은 최상위 객체
- 브라우저 환경에는 window, node.js환경에서는 global이 전역객체를 가리킨다.
- 전역객체는 표준 빌트인 객체와 환경에 따른 호스트 객체, var 키워드로 선언한 전역변수, 전역함수를 프로퍼티로 갖는다
- → 전역 객체는 계층적 구조상 어떤객체에도 속하지 않은 모든 빌트인 개체의 최상위 객체

전역객체의 특징

- 개발자가 의도적으로 생성할수없다. → 전역객체를 생성할수있는 생성자 함수가 제공되지 않는다
- 프로퍼티를 참조할때 window를 생략할 수 있다
- 전역 객체는 Object, String, Number… emd 같은 모든 표준 빌트인 객체를 프로퍼티로 가지고 있다.
- 자바스크립트 실행 환경에 따라 추가적으로 프로퍼티와 메서드를 갖는다. 브라우저 환경에서는 DOM,BOM,Canvase등과 같은 클라이언트 사이드 Web API를 호스트 객체로 제공하고 Node.js환경에서는 고유의 API를 호스트 객체로 제공한다
- var키워드로 선언한 전역변수와 선언하지 않은 변수에 값을 할당한 암묵적 전역 그리고 전역함수는 전역객체의 프로퍼티가 된다.

```jsx
//프로퍼티를 참조할때 window를 생략할 수 있다
window.parseInt('F',16);//15
parseInt('F',16);//15
window.parseInt === parseInt

//var키워드로 선언한 전역변수와 선언하지 않은 변수에 값을 할당한 암묵적 전역
//그리고 전역함수는 전역객체의 프로퍼티가 된다.
var foo = 1;
console.log(window.foo);//1
function bar(){return 3;}
console.log(window.bar());

//let은 전역객체의 프로퍼티가 되지않는다
let foo2 = 1;
console.log(window.foo2);//undefined
```

### 21.4.1 빌트인 전역 프로퍼티

Infinity : 무한대를 나타내는 숫자값 Infinity를 갖는다

```jsx
console.log(window.Infinity === Infinity);//true
console.log(3/0);//0으로 나누기 때문에 무한대가 된다 - 양의 무한대 Infinity
console.log(-3/0);//0으로 나누기 때문에 무한대가 된다 - 음의 무한대 -Infinity
console.log(typeof Infinity);
```

NaN : 숫자가 아님을 나타내는 숫자값을 갖는다

```jsx
console.log(window.NaN);//NaN
console.log(Number('aa'));//NaN
console.log(1 * 'ee');//NaN
console.log(typeof NaN);//number
```

undefined : 원시타입 undefined를 값으로 같는다

```jsx
console.log(window.undefined);//undefined
var foo;
console.log(foo);//undefined
```

### 21.4.2 빌트인 전역함수

eval

자바스크립트 코드를 나타내는 문자열을 인수로 전달 받는다. - 전달받은 문자열이 표현식이라면 문자열코드를 런타임에 평가하여 값을 생성, 문이라면런타임에 실행

eval 함수가 호출된 위치에 해당하는 기존의 스코프를 런타임에 동적으로 수정
엄격모드에서 eval 함수는 기존의 스코프를 수정하지 않고 eval 함수자신의 자체적인 스코프를 생성보안에 취약, 최적화가 수행되지않아 처리속도가 느리다 eval함수의 사용은 금지해야한다.

```jsx
eval(code);

eval('alert()');
eval('1+2');//3

const x = 1;
function foo(){
	eval('var x = 2;');
	console.log(x);//2
}
foo();
console.log(x);//1

function foo2(){
	'use strict';
	eval('var x = 2;console.log(x);');//2
	console.log(x);//1
}
foo();
console.log(x);//1
```

isFinite

인수가 유한수인지 검사 유한수이면 true, 무한수이면 false

```jsx
isFinite(val)

isFinite(0);//true
isFinite(null);//true null->0
isFinite('10');//true
isFinite(Infinity);//false
isFinite('hi');//false NaN도 false로 반환
```

isNaN

인수가 NaN인지 아닌지

```jsx
isNaN(val)

isNaN(NaN);//true
isNaN(10);//false
isNaN('test');//true
isNaN('')//''->0 false
```

parseFloat

전달받은 문자열 인수를 실수로 해석하여 반환

```jsx
parseFloat(str)

parseFloat('3.14');//3.14

//공백으로 구분된 문자열은 첫번째 문자열만 변환
parseFloat('34 35 66')//34

//첫번째 문자열이 숫자변환이 안되면 NaN
parseFloat('Hi I`m 28')//NaN

// 공백무시
parseFloat(' 60 ')//60
```

parseInt

전달받은 문자열 인수를 정수로 해석하여 반환

```jsx
parseInt(str,radix)

parseInt('123');//123
parseInt('3.14');//3
parseInt(123);//123
parseInt(3.14);//3

parseInt('f',16)//15
parseInt('0xf')//15

//2진수, 8진수 리터럴은 재대로 해석하지 못한다.
parseInt('0b10')//0
parseInt('0o10')//0

parseInt('11',2)//3
parseInt('11',8)//9

//첫문자가 지수의 숫자로 변환할수 없다면 NaN반환
parseInt('A0')//NaN
parseInt('20',2)//NaN

//첫문자가 숫자, 그뒤 숫자가 아닌문자면 무시
parseInt('1A10')//1
parseInt('102',2)//2
```

encodeURI / decodeURI

encodeURI : URI를 이스케이프 처리하여 인코딩, decodeURI : 이스케이프 처리 이전으로 디코딩함

### 21.4.3 암묵적전역

암묵적전역 : 선언을하지 않았어도 전역객체에 프로퍼티를 동적 생성하여 전역변수처럼 동작하는것

```jsx
//x는 선언하여 호이스팅 발생
console.log(x);
console.log(y);//호이스팅 발생x

var x = 10;
function foo(){
	y = 20; //window.y = 20;
}
foo();
console.log(x+y);//30

delete x;//전역변수는 삭제되지 않는다
delete y; //프로퍼티 삭제된다

console.log(x);//10
console.log(y);//undefined

```