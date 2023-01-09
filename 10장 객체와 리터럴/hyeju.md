## 10.1 객체란?

- 객체는 0개 이상의 프로퍼티로 구성된 집합이며, 프로퍼티는 키와 값으로 구성된다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/dae1a0a4-f7af-4088-aa2a-7ee6b9346b40/Untitled.png)

프로퍼티 값이 함수일 경우, 일반 함수와 구분하기 위해 메서드라 부른다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8549d28e-1859-4476-b0c0-b979864d78a5/Untitled.png)

- 이처럼 객체는 프로퍼티와 메서드로 구성된 집합체이다.

## 10.2 객체 리터럴에 의한 객체 생성

- 자바스크립트는 프로토타입 기반 객체지향 언어로서 다양한 객체 생성 방법을 지원한다.
    - 객체 리터럴
    - object 생성자 함수
    - 생성자 함수
    - object.create 메서드
    - 클래스(ES6)

이 중 객체 리터럴을 사용하는 방법이 가장 일반적이고 간단한 방법.

```jsx
var person = {
	name : 'LEE',
	sayHello : function(){
		console.log(`Hello! my name is ${this.name}.`);
	}
}

console.log(typeof person); //object
console.log(person); //{name: 'LEE', sayHello: ƒ}
```

## 10.3 프로퍼티

식별자 네이밍 규칙을 따르지 않는 프로퍼티 키 이름에는 반드시 따옴표를 사용해야 함.

문자열 또는 문자열로 평가할 수 있는 표현식을 사용해 프로퍼티 키를 동적으로 생성할 수도 있다.

```jsx
var obj = {};
var key = 'hello';

//ES5 : 프로퍼티 키 동적 생성
obj[key] = 'world';
//ES6 : 계산된 프로퍼티 이름
//var obj = { [key] : 'world'};
console.log(obj); // {hello: 'world'}
```

## 10.4 메서드

- 메서드는 객체에 묶여 있는 함수를 의미한다.

```jsx
var circle = {
	radius : 5, //프로퍼티

	getDiameter : function(){
		return 2 * this.radius; //this는 circle을 가리킴.
	}
};

console.log(circle.getDiameter()); //10
```

## 10.5 프로퍼티 접근

- 프로퍼티 접근 방법은 두가지다
    - 마침표 표기법
    - 대괄호 표기법

```jsx
var person = {
	name : 'Lee'
};

//마침표 표기법에 의한 프로퍼티 접근
console.log(person.name); //Lee
//대괄호 표기법에 의한 프로퍼티 접근
console.log(person['name']); //Lee
```

- 대괄호 프로퍼티 접근 연산자 내부에 지정하는 프로퍼티 키는 반드시 따옴표로 감싼 문자열이어야 함.
- 객체에 존재하지 않는 프로퍼티에 접근하면 undefined를 반환한다.

## 10.6 프로퍼티 값 갱신

- 존재하는 프로퍼티에 값을 할당하면 프로퍼티 값이 갱신됨

## 10.7 프로퍼티 동적 생성

- 존재하지 않는 프로퍼티에 값을 할당하면 프로퍼티가 동적으로 생성되어 추가되고 프로퍼티 값이 할당됨.

```jsx
var person = {
	name : 'Lee'
};

person.age = 20;

console.log(person); //{name : 'Lee', age : 20}
```

## 10.8 프로퍼티 삭제

- delete 연산자로 객체의 프로퍼티를 삭제한다. 존재하지 않는 프로퍼티를 삭제하면 에러없이 무시됨.

```jsx
var person = {
	name : 'Lee'
};

person.age = 20;

delete person.age;
delete person.address;

console.log(person); //{name: 'Lee'}
```

## 10.9 ES6에서 추가된 객체 리터럴의 확장 기능

### 10.9.1 프로퍼티 축약 표현

- ES6에서는 변수 이름과 프로퍼티 키가 동일한 이름일 대 프로퍼티 키를 생략할 수 있다. 이때 프로퍼티 키는 변수 이름으로 자동 생성

```jsx
//ES6
let x = 1, y = 2;

//프로퍼티 축약 표현
const obj = { x, y };

console.log(obj); //{x: 1, y: 2}
```

### 10.9.2 계산된 프로퍼티 이름

- 표현식을 사용해 프로퍼티 키를 동적으로 생성할 수도 있다. 단, 프로퍼티 키로 사용할 표현식을 대괄호([…])로 묶어야 한다. 이를 계산된 프로퍼티 이름이라 한다.

```jsx
//ES5
var prefix = 'prop';
var i = 0;

var obj = {};

obj[prefix + '-' + ++i] = i;
obj[prefix + '-' + ++i] = i;
obj[prefix + '-' + ++i] = i;

console.log(obj); //{prop-1: 1, prop-2: 2, prop-3: 3}
```

```jsx
//ES6
const prefix = 'prop';
let i = 0;

//객체 리터럴 내부에서 계산된 프로퍼티 이름으로 프로퍼티 키를 동적 생성
const obj = {
	[`${prefix}-${++i}`] : i,
	[`${prefix}-${++i}`] : i,
	[`${prefix}-${++i}`] : i
};

console.log(obj); //{prop-1: 1, prop-2: 2, prop-3: 3}
```

### 10.9.3 메서드 축약 표현

```jsx
//ES5
var obj = {
	name : 'Lee',
	sayHi : function() {
		console.log('Hi! ' + this.name);
	}
};

obj.sayHi(); //Hi! Lee
```

```jsx
//ES6
const obj = {
	name : 'Lee',
	//메서드 축약 표현
	sayHi(){
		console.log('Hi! ' + this.name);
	}
};

obj.sayHi(); //Hi! Lee
```

---

## 알아두면 좋은!🎯

- Node.js 환경에서는 name이라는 식별자(변수, 함수 등의 이름) 선언이 없다. 하지만 브라우저 환경에서는 name이라는 전역 변수가 암묵적으로 존재한다. 전역 변수 name은 창의 이름을 가리키며, 기본값은 빈 문자열이다.