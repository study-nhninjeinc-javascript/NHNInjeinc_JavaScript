# 21_빌트인 객체

- 표준 빌트인 객체
    
    전역 객체의 프로퍼로서 별도의 선언없이 전역 변수처럼 사용
    
- 호스트 객체
    - 자바스크립트 실행환경(브라우저 또는 Node.js)에서 추가로 제공하는 객체
- 사용자 정의 객체
    - 사용자가 직접 정의하는 개체

### 표준 빌트인 객체

자바스크립트는 **Object, String, Number, Symbol, Date, Math, Array, Map/Set** …등 40여개 표준 빌트인 객체 제공

Math, Reflect, Json을 제외한 표준 빌트인 객체는 **모두 인스턴스를 생성할 수 있는 생성자 함수 객체**

```jsx
const strObj = new String('Kim'); //String {'Kim'}
console.log(type of strObj) //object

const numObj = new Number(123) // Number {123}
console.log(type of numObj) //object
```

```jsx
//String 생성자 함수에 의한 String 객체 생성
const strObj = new String('Kim') // String{'kim'}

//String 생성자 함수를 통해 생성한 strObj 객체의 프로토 타입은 String.prototype!!
console.log(Object.getPrototypeOf(strObj) === String.prototype ) //true
//String빌트인으로 생성했으니!

//각 prototype 마다 제공하는 메서드가 다른 것이 포인트 ?!
```

예를 들어서 Number로 생성한다면

 

```jsx
const numObj = new Number(1.5);

//toFixed는 Number.prototype의 프로토타입 메서드!
console.log(numObj.toFixed()); //2 

//정수인지 판단
console.log(numObj.isInteger(0.5)); // false
```

### 원시값과 래퍼 객체

문자열이나 숫자, 불리언 등의 원시값이 있는데도 문자열, 숫자, 불리언 객체를 생성하는 String, Number 등 표준 **빌트인 생성자 함수가 존재하는 이유?**

⚠️ 원시값인 문자열이 마치 객체처럼 동작

```jsx
const str ='hello';

//원시 타입인 문자열이 프로퍼티와 메서드를 갖고 있는 객체처럼 동작한다?

console.log(str.length); //5
console.log(str.toUpperCase) // HELLO
```

⇒자바스크립트 엔진이 일시적으로 원시값을 연관된 객체로 변환해 주기 때문 

연관된 객체를 생성하여 생성된 객체로 접근하고 **호출 후에 다시 원시값으로** 

⇒ 이러한 객체를 **래퍼객체**라고 한다

```jsx
const str = 'test';
console.log(str.length); // 4

//래퍼 객체로 프로퍼티 접근하거나 메서드 호출 후 다시 원시값으로
console.log(typeof str); // string
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6e8fd021-58b5-45ab-87eb-725b6b54cefb/Untitled.png)

```jsx
// ① 식별자 str은 문자열을 값으로 가지고 있다.
const str = "hello";

// ② 식별자 str은 암묵적으로 생성된 래퍼 객체를 가리킨다.
// 식별자 str의 값 'hello'는 래퍼 객체의 [[StringData]] 내부 슬롯에 할당된다.
// 래퍼 객체에 name 프로퍼티가 동적 추가된다.
str.name = "Lee";

// ③ 식별자 str은 다시 원래의 문자열, 즉 래퍼 객체의 [[StringData]] 내부 슬롯에 할당된 원시값을 갖는다.
// 이때 ②에서 생성된 래퍼 객체는 아무도 참조하지 않는 상태(사용되지 않는 쓰레기 값이 되었음)이므로 가비지 컬렉션의 대상이 된다.

// ④ 식별자 str은 새롭게 암묵적으로 생성된(②에서 생성된 래퍼 객체와는 다른) 래퍼 객체를 가리킨다.
// 새롭게 생성된 래퍼 객체에는 name 프로퍼티가 존재하지 않는다.
console.log(str.name); // undefined

// ⑤ 식별자 str은 다시 원래의 문자열, 즉 래퍼 객체의 [[StringData]] 내부 슬롯에 할당된 원시값을 갖는다.
// 이때 ④에서 생성된 래퍼 객체는 아무도 참조하지 않는 상태이므로 가비지 컬렉션의 대상이 된다.
console.log(typeof str, str);
```

### 전역 객체

전역 객체는 자바스크립트 환경에 따라 자칭하는 이름이 다르다

window,self,this,frames … Node.js = global

전역 객체는 표준 빌트인 객체와 환경에 따른 호스트 객체(API, Node.js의 호스트 API) 그리고 var키워드로 선언한 변수 전역함수를 프로퍼티로 갖는다

```jsx
//문자열 'F'를 16진수로 해석하여 10진수로 변환 후 반환
window.parseInt('F',16) // 15

//window.parseInt는 ParseInt로 호출할 수 있다.
parseInt('F',16) // 15 window생략가능

window.parseInt === parseInt // true
```

```jsx
var foo = 1;
console.log(window.foo);//1

//선언하지 않은 변수의 값을 암묵적 전역. bar는 전역 변수가 아니라 전역 객체의 프로퍼티

bar = 2;
console.log(window.bar)//2

function baz(){return 3;}
console.log(window.baz());//3
```

⚠️let,  const 키워드로 선언한 전역변수는 **전역 객체의 프로퍼티가 X**

window. ~ 접근할 수 없으며 **전역 렉시컬 환경의 선언적 환경 레코드 내에 존재**

```jsx
let foo = 123;
console.log(window.foo)//undefined
```

### 빌트인 전역 프로퍼티

예를 들어

```jsx
//isFinite
//인수가 유한수이면 true 반환
isFinite(0) // true
isFinite(2e64) //true
isFinite('10') //true
isFinite(null) // true: null =>0
isFinite(infinity) // false
//인수가 NAN으로 평가되는 값이라면 false반환
isFinite(NaN) // false
isFinite('Hello') // false

//null 이 숫자로 반환시에 0
```

---

encodeURI / decodeURI

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ce2ca86a-1502-4394-958e-649e84055eed/Untitled.png)

---

### 암묵적 전역

```jsx
console.log(x) //undefined

console.log(y) // referenceError 전역 변수가 아니라 전역 객체 프로퍼티는 호이스팅X
var x = 10;

function foo(){
	y = 20;
}
```

⚠️y는 delete 연산자로 삭제 가능 , 전역 변수 x는 delete 연산자로 삭제 불가