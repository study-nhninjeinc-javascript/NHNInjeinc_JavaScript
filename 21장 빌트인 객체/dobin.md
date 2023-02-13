# 21장 빌트인 객체

### 자바스크립트의 3가지 객체

- 표준 빌트인 객체
    
    표준 빌트인 객체는 애플리케이션 전역의 공통 기능을 제공한다.
    
    ECMAScript 사양에 정의된 객체이므로 실행환경(브라우저나 Node.js)과 관계없이 언제나 사용할 수 있다. 표준 빌트인 객체는 전역 객체의 프로퍼티로서 제공된다. 따라서 별도의 선언없이 전역변수처럼 언제나 참조가 가능
    
- 호스트 객체
    
    호스트 객체는 실행환경에서 추가로 제공하는 객체를 말한다. 브라우저 환경에서는 Web API를 호스트 객체로 제공하고, Node.js 환경에서는 Node.js 고유의 API를 호스트객체로 제공한다.
    
- 사용자 정의 객체
    
    사용자 정의 객체는 표준 빌트인 객체와 호스트 객체처럼 기본 제공되는 객체가 아닌 사용자가 직접 정의한 객체를 말함. 
    

### 표준 빌트인 객체

자바스크립트는 Object, String, Number 등 40여개의 표준 빌트인 객체를 제공한다.

Math, Reflect, JSON을 제외한 표준 빌트인 객체는 모두 인스턴스를 생성할 수 있는 생성자 함수 객체다. 생성자 함수 객체인 표준 빌트인 객체는 프로토타입 메서드와 정적 메서드를 제공하고 생성자 함수 객체가 아닌 표준 빌트인 객체는 정적 메서드만 제공한다.

생성자 함수인 표준 빌트인 객체가 생성한 인스턴스의 프로토타입은 표준 빌트인 객체의 prototype 프로퍼티에 바인딩된 객체다.

💡 예제

```jsx
// String 생성자 함수에 의한 String 객체 생성
const strObj = new String('Shin'); // String {"Shin"}

// String 생성자 함수를 통해 생서한 strObj 객체의 프로토타입은 String.prototype이다.
console.log(Object.getPrototypeof(strObj) === String.prototype); // true
```

### 원시값과 래퍼 객체

문자열이나 숫자, 불리언 등의 원시값이 있는데도 문자열,, 숫자, 불리언 객체를 생성하는 String, Number, Boolean 등의 표준 빌트인 생성자 함수가 존재하는 이유는 무엇일까?

💡 아래와 같은 예제가 있다.

```jsx
const str = "hello";

// 원시 타입인 문자열이 프로퍼티와 메서드를 갖고 있는 객체처럼 동작한다.
console.log(str.length); // 5
console.log(str.toUpperCase()); // HELLO
```

원시값은 객체가 아니므로 프로퍼티나 메서드를 가질 수 없는데도 원시값인 문자열이 마치 객체처럼 동작한다. 위 예제처럼 마침표 표기법(또는 대괄호 표기법)으로 접근하면 자바스크립트 엔진이 일시적으로 원시값을 연관된 객체로 변환해 주기 때문이다.

원시값을 객체처럼 사용하면 JS 엔진이 암묵적으로 연관된 객체를 생성하여 생성된 객체로 프로퍼티에 접근하거나 메서드를 호출하고 다시 원시값으로 되돌린다.

이처럼 문자열, 숫자, 불리언 값에 대해 객체처럼 접근하면 생성되는 임시 객체를 래퍼 객체라 한다.

래퍼 객체인 생성자 함수의 인스턴스가 생성되면 원시값은 래퍼 객체의 내부 슬롯에  할당된다.

그리고 위 예제의 문자열 래퍼 객체인 String 생성자 함수의 인스턴스는 String.prototype의 메서드를 상속받아 사용할 수 있다.

![https://dev-dobin.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fc36724fc-e63f-4ac4-a0f1-aa38dece5520%2FUntitled.png?id=479592d8-3eb6-4593-a543-61a76f4b9c7f&table=block&spaceId=f800ef36-5d6f-4e98-a2f1-f4b2d48cc3fe&width=940&userId=&cache=v2](https://dev-dobin.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fc36724fc-e63f-4ac4-a0f1-aa38dece5520%2FUntitled.png?id=479592d8-3eb6-4593-a543-61a76f4b9c7f&table=block&spaceId=f800ef36-5d6f-4e98-a2f1-f4b2d48cc3fe&width=940&userId=&cache=v2)

그 후 래퍼 객체의 처리가 종료되면 래퍼 객체의 [[StringData]] 내부 슬롯에 할당된 원시값으로 원래의 상태, 즉 식별자가 원시값을 갖도록 되돌리고 래퍼 객체는 가비지 컬렉션의 대상이 됨.

이처럼 문자열, 숫자, 불리언, 심벌은 암묵적으로 생성되는 래퍼 객체에 의해 마치 객체처럼 사용할 수 있으며, 표준 빌트인 객체인 String, Number, Boolean, Symbol의 프로토타입 메서드 또는 프로퍼티를 참조할 수 있다. 따라서 String, Number,Boolean 생성자 함수를 new 연산자와 함께 호출하여 문자열, 숫자, 불리언 인스턴스를 생성할 필요가 없으며 권하지도 않는다.

Symbol은 생성자 함수가 아니므로 이 논의에서는 제외

### 전역 객체

어떤 객체보다도 먼저 생성되는 특수한 객체이며, 어떤 객체에도 속하지 않는 최상위 객체를 말함.

> globalThis

globalThis는 ES11 부터 도입되어 브라우저, Node.js 환경에서 가리키던 다양한 식별자(window, global 등)를 통일한 식별자다.
> 

- 전역 객체는 표준 빌트인 객체와 환경에 따른 호스트 객체, 그리고 var 키워드로 선언한 전역변수와 전역 함수를 프로퍼티로 갖는다.

즉, 전역 객체는 계층적 구조상 어떤 객체에도 속하지 않은 모든 빌트인 객체의 최상위 객체다.

(단, 프로토타입 상속 관계상에서 최상위 객체라는 의미가 아님)

전역 객체는 자신은 어떤 객체의 프로퍼티도 아니며 객체의 계층적 구조상 표준 빌트인 객체와 호스트 객체를 프로퍼티로 소유하는 것을 말함

### 빌트인 전역 프로퍼티

빌트인 전역 프로퍼티는 전역 객체의 프로퍼티를 의미한다. 주로 애플리케이션 전역에서 사용하는 값을 제공한다.

💡 Infinity

- Infinity 프로퍼티는 무한대를 나타내는 숫자값 Infinity를 갖는다.

💡 NaN

NaN 프로퍼티는 수자가 아님을 나타내는 숫자값을 갖는다. Number.NaN 프로퍼티와 같음

💡 undefined

undefined 프로퍼티는 원시 타입 undefined를 값으로 갖는다.

### 빌트인 전역 함수

빌트인 전역 함수는 애플리케이션 전역에서 호출할 수 있는 빌트인 함수로서 젼역 객체 메서드다.

💡 eval

eval 함수는 자바스크립트 코드를 나타내는 문자열을 인수로 받아 문자열 코드가 표현식이면 문자열 코드를 런타임에 평가하여 값을 생성하고, 인수가 표현식이 아닌 문이라면 문자열 코드를 런타임에 실행한다. 그리고 eval 함수는 기존의 스코프를 런타임에 동적으로 수정한다. 또한, eval 함수를 통해 실행되는 코드는 JS 엔진에 최적화가 수행되지 않으므로 일반적인 코드 실행에 비해 처리 속도가 느리며, 여러 문제점을 갖고 있는 이유로 사용을 금지해야 한다.

💡 isFinite

전달 받은 인수가 정상적인 유한수인지 검사하여 유한수이면 true 반환, 무한수이면 false를 반환한다. (NaN 도 false 반환)

💡 isNaN

전달받은 인수가 NaN인지 검사하여 그 결과를 불리언 타입으로 반환한다. 전달받은 인수의 타입이 숫자가 아닌 경우 숫자로 타입을 변환한 후 검사를 수행

💡 parseFloat, parseInt 

parseFloat는 전달 받은 문자열을 부동 소숫점 숫자, 즉 실수로 해석하고

parseInt는 전달 받은 문자열 인수를 정수로 해석하여 반환한다. 

💡 encodeURI / decodeURI

encodeURI 함수는 완전환 URI를 문자열로 전달받아 이스케이프 처리를 위해 인코딩함

![https://dev-dobin.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F076d9596-b1cf-4c9c-8a14-ec73af542cfc%2FUntitled.png?id=90dc5941-a963-470d-80be-6be407d9c7dd&table=block&spaceId=f800ef36-5d6f-4e98-a2f1-f4b2d48cc3fe&width=1310&userId=&cache=v2](https://dev-dobin.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F076d9596-b1cf-4c9c-8a14-ec73af542cfc%2FUntitled.png?id=90dc5941-a963-470d-80be-6be407d9c7dd&table=block&spaceId=f800ef36-5d6f-4e98-a2f1-f4b2d48cc3fe&width=1310&userId=&cache=v2)

인코딩이란 URI의 문자드을 이스케이프 처리하는 것을 의미한다. 이스케이프 처리는 네트워크를 통해 정보를 공유할 때 어떤 시스템에서도 읽을 수 있는 아스키 문자 셋으로 변환하는 것이다.

decodeURI 함수는 인코딩된 URI를 인수로 전달받아 이스케이프 처리 이전으로 디코딩한다.

💡 encodeURIComponent / decodeURIComponent

encodeURIComponent 함수는 URI 구성요소를 인수로 전달받아 인코딩하낟. 여기서 인코딩이란 URI의 문자들을 이스케이프 처리하는 것을 의미한다. 단, 알파벳 0 ~ 9의 숫자, - _ · ! ~ * ‘ ( ) 문자는 이스케이프 처리에서 제외된다. decodeURIComponent 함수는 매개변수로 전달된 URI 구성 요소를 디코딩한다. 반면 encodeURI 함수는 매개변수로 전달된 문자열을 완전한 URI 전체라고 간주한다. 따라서 쿼리 스트링 구분자로 사용되는 =, ?, &은 인코딩하지 않는다.

💡 암묵적 전역

```jsx
var x = 10; // 전역 변수

function foo () {
	// 선언하지 않은 식별자에 값을 할당
	y = 20; // window.y = 20;
}

// 선언하지 않은 식별자 y를 전역에서 참조할 수 있다.
console.log(x + y); // 30
```

foo 함수 내의 y는 선언하지 않은 식별자다. 따라서 y = 20이 실행되면 참조 에러가 발생한 것처럼 보인다. 하지만 선언하지 않은 식별자 y는 마치 선언된 전역 변수처럼 동작한다. 이는 선언하지 않은 식별자에 값을 할당하면 전역 객체의 프로퍼티가 되기 때문이다.

foo 함수가 호출되면 자바스크립트 엔진은 y 변수에 값을 할당하기 위해 먼저 스코프 체인을 통해 선언된 변수인지 확인한다. 이때 foo 함수의 스코프와 전역 스코프 어디에서도 y 변수의 선언을 찾을 수 없으므로 참조 에러가 발생한다. 하지만 자바스크립트 엔진은 y = 20을 window.y = 20으로 해석하여 전역 객체에 프로퍼티를 동적 생성한다. 결국 y는 전역 객체의 프로퍼티가 되어 마치 전역 변수처럼 동작한다. 이러한 현상을 암묵적 전역이라 한다.

하지만 y는 변수 선언 없이 단지 전역 객체의 프로퍼티로 추가 되었을 뿐이다. 따라서 y는 변수가 아니다. y는 변수가 아니므로 변수 호이스팅이 발생하지 않는다.