# 17_생성자 함수에 의한 객체 생성

## *Object 생성자 함수

new 연산자와 함께 Object 생성자 함수를 호출하면 **빈 객체 생성하여 반환**

why? 

`'new'` 연산자와 생성자 함수를 사용하면 유사한 객체 여러 개를 쉽게 만

1. 함수 이름의 첫 글자는 대문자로 시작
2. 반드시 `'new'` 연산자를 붙여 실행

```jsx
function User(name) {
  // this = {};  (빈 객체가 암시적으로 만들어짐)

  // 새로운 프로퍼티를 this에 추가함
  this.name = name;
  this.isAdmin = false;

  // return this;  (this가 암시적으로 반환됨)
}

let user = new User('kim')

user// {name:kim, isAdmin:false}
let user = new User('lee')
let user = new User('park')...
```

**재사용할 수 있는 객체 생성 코드**를 구현

### 생성자함수란

= new연산자와 함께 호출하여 **객체(인스턴스) 생성하는 함수** 

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b6cda83e-7905-4ba6-a0bb-38e9b12e795c/Untitled.png)

Object생성자 함수 이외에 여러 형태의 빌트인 생성자함수 제공

⚠️생성자 함수를 사용하여 객체 생성은 특별한 이유가 없다면 지양  (리털럴이 편하)

### 객체 리터럴에 의한 객체 생성 방식의 문제점

```jsx
const circle1 = {
  radius: 5,
  getDiameter() {
    return 2 * this.radius;
  }
};

console.log(circle1.getDiameter()); // 10

const circle2 = { 
  radius: 10,
  getDiameter() { 
    return 2 * this.radius; 
  }
}; 

console.log(circle2.getDiameter()); // 20
```

*객체 리터럴에 의한 객체 생성 방식은 **단 하나의 객체만 생성**한다.

*따라서 **동일한 프로퍼티를 갖는 객체를 여러개 생성**해야 하는 경우 

매번 같은 프로퍼티를 기술해야 하기 때문에 **비효율적**이다.

### **생성자 함수에 의한 객체 생성 방식의 장점**

```jsx
function Circle(radius) {
  // 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius; 
  };
}

// 인스턴스의 생성
const circle1 = new Circle(5); // 반지름이 5인 Circle 객체를 생성
const circle2 = new Circle(10); // 반지름이 10인 Circle 객체를 생성 

console.log(circle1.getDiameter()); // 10 
console.log(circle2.getDiameter()); // 20
```

⚠️this

**this**

this는 객체 자신의 프로퍼티나 메서드를 참조하기 위한 자기 참조 변수다. 

**this가 가리키는 값, 즉 this 바인딩은 함수 호출 방식에 따라 동적으로 결정된다.**

this 값은 런타임에 결정

| 함수 호출 방식 | this가 가리키는 값(this 바인딩) |
| --- | --- |
| 일반 함수로서 호출 | 전역 객체 |
| 메서드로서 호출 | 메서드를 호출한 객체(마침표 앞의 객체) |
| 생성자 함수로서 호출 | 생성자 함수가 생성할 인스턴스 |

```jsx
function foo() {
  console.log(this);
}

// 일반적인 함수로서 호출
// 전역 객체는 브라우저 환경에서는 window, Node.js 환경에서는 global을 가리킨다.
foo(); // window

const obj = { foo }; // ES6 프로퍼티 축약 표현

// 메서드로서 호출
obj.foo(); // obj //{foo:f}

// 생성자 함수로서 호출
const inst = new foo(); // inst(생성자 함수가 생성할 인스턴스) // foo{}
```

### 인스턴**생성과 this 바인딩**

빈 객체 생성 ⇒ 인스턴스는 this에 바인딩

⚠️바인딩 

식별자와 값을 연결하는 과정  **this와 this가 가리칠 객체를 바인딩**

### 인스턴스 초기화

this에 바인딩되어 있는 인스턴스에 프로퍼티나 메서드를 추가하고

생성자 함수가 **인수로 전달받은 초기값을 인스턴스 프로퍼티에 할당하여 초기화하거나 고정값을 할당합니다.**

```jsx
function Circle(radius){
//1.암묵 인스턴스 생성 및 this바인

//2.this에 바인딩 되어 있는 인스턴스 초기화
	this.radius=radius;
	this.f=function(){
		return 2 * this.radius;
	};
}
```

### 인스턴스 반환

생성자 함수 내부의 모든 처리가 끝나면 인스턴스가 바인딩된 this를 암묵적 반환

```jsx
function Circle(radius){
//1.암묵 인스턴스 생성 및 this바인

//2.this에 바인딩 되어 있는 인스턴스 초기화
	this.radius=radius;
	this.f=function(){
		return 2 * this.radius;
	};

//3.완성된 인스턴스가 바인딩 된 this암묵적 반
}

//인스턴스 생성
const test1 = new Circle(1);
console.log(test1) // Circle{radius:1, f:f}

//만약 this가 아닌 다른 객체를 명시적 반환하면 this가 반환되지 못하고 return 문에 명시 객체 반환
//원시값 return시에는 원시값은 무시하고 this반
```

### 내부메서드 [[Call]과 [[Construct]]

함수는 객체이므로 일반 객체와 동일한 동작 가능 .

⚠️함수와 일반객체 차이점 

`일반 객체는 호출할 수 없지만 함수는 호출할 수 있습니다`

함수는 일반객체의 내부 슬롯과 내부 메서드

[[Environment]],[[FormalParameters]]등

**추가적으로**  [[Call]], [[Construct]] 메서드를 추가로 가지고 있음

```
내부 메서드 [[Call]] 을 갖는 함수 객체 = callable
내부 메서드 [[Construct]] 를 갖는 함수 객체 = constructor
내부 메서드 [[Construct]] 를 갖지 않는 함수 객체 = non-constructor
```

⚠️`모든 함수 객체는 호출할 수 있지만 모든 함수 객체를 생성자 함수로서 호출할 수 있는 것은 아니다` 

⚠️constructor 와 non-constructor의 구분

constructor : 함수선언문, 함수표현식, 클래스
non-constructor : 메서드(ES6 메서드 축약표현) , 화살표함수

```jsx
const f1 ={
	x=function(){} //프로터티 일반함수정의(선언문 및 표현식) -> constructor
}

const f2 = {
x(){} // 축약표현(화살표포함) ->non-constructor
}
```

### new.target

javascript.ko에서는 중요하지 않은 내용이라 스킵해도 된다고 하였지만 간단하게 살펴보자면

=new연산자와 함께 호출여부를 검사

new.target은 함수 자신을 가르킨다

일반함수의 [new.target](http://new.target)은 undefined

es6에서는 안되니 instance of 로 확인 !