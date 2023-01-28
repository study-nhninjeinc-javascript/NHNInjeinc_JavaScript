# 17. 생성자 함수에 의한 객체 생성

## Object 생성자 함수

```jsx
// 빈 객체 생성
const person = new Object();

// 프로퍼티 추가
person.name = 'Lee';
person.sayHello = function() {
	console.log(`Hi! My name is ${this.name}`);
};

person.sayHello(); // Hi! My name is Lee
```

- 자바스크립트는 Object 생성자 함수 이외에도 String, Number, Boolean, Function, Array, Data, RegExp.Promise 등의 빌트인 생성자 함수를 제공한다.

## 생성자 함수

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

console.log(circle2.getDiameter(); // 20
```

### 생성자 함수에 의한 객체 생성 방식의 장점

```jsx
// 생성자 함수
function Circle(radius) {
	// 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
	this.radius = radius;
	this.getDiameter = function() {
		return 2 * this.radius;
	};
}

// 인스턴스 생성
const circle1 = new Circle(5);
const circle2 = new Circle(10);
```

```jsx
// 함수 호출 방식에 따른 this가 가리키는 값을 알아보자
function isThis() {
	console.log(this);
}

// 일반 함수로서 호출한 경우 전역 객체를 가리킨다.
isThis(); // window

// 메서드로서 호출한 경우 메서드를 호출한 객체를 가리킨다.
const obj = { isThis }; // 프로퍼티 축약 표현
obj.isThis(); // obj

// 생성자 함수로서 호출한 경우 생성자 함수가 생성할 인스턴스를 가리킨다.
const inst = new isThis(); // inst

```

### 생성자 함수의 인스턴스 생성 과정

```jsx
function InstanceCreation(num) {
	// 암묵적으로 빈 객체가 생성되며 빈 this에 바인딩 된다.
	console.log(this); // InstanceCreation {}

	// this에 바인딩되어 있는 인스턴스를 초기화한다.
	this.num = num; // { num: num }
	this.getExNum = function() {
		return num * num;
	}

	// return {}; // -> 명시적으로 객체를 반환하면 this 반환이 무시됨!
	return 100; // -> 명시적으로 원시 값을 반환하면 원시 값 반환이 무시되고 this가 반환됨!
	// 생성자 함수에서 this 가 아닌 다른 값을 반환하는 것은 생성자 함수의 기본 동작을 훼손하니, 생성자 함수 내부에서는 return 문을 사용하지 않는 편이 좋을듯 하다.

	// 완성된 인스턴스가 바인딩된 this를 암묵적으로 반환한다.
}

// 인스턴스를 생성하는, InstanceCreation 생성자 함수는 암묵적으로 this를 반환한다.
const instanceCreate = new InstancCreation(5);
console.log(instanceReate); // InstanceCreation { num: 5, getExNum: function() {}}
```

```jsx

```

### 내부 메서드 [[Call]]과 [[Construct]]

```jsx
function imObj() {};
// 함수는 객체다 -> 프로퍼티를 소유할 수 있다!
imObj.num = 10;

// 메서드도 마찬가지
imObj.method = function() {
	console.log(this.num);
};

imObj.method(); // 10
```

> 일반 객체는 호출할 수 없지만 함수는 (객체면서) 호출할 수 있다.
따라서 함수는 일반 객체가 가지는 내부 슬롯, 내부 메서드와 함께 함수로서 동작하기 위한 [[Environment]], [[FormalParameters]] 등의 내부 슬롯과
[[Call]], [[Construct]]와 같은 내부 메서드를 추가로 가지고 있다.
> 

```jsx
function highObj() {};

// 일반적인 함수로서 호출시 [[Call]] 내부 메서드가 호출됨
highObj();

// 생성자 함수로서 호출시 [[Construct]] 내부 메서드가 호출됨
new highObj();
```

<aside>
💡 모든 함수는 Callable 객체 즉 [[Call]] 내부 메서드를 가지는 객체이다.
그러나 모든 함수가 [[Construct]] 내부 메서드를 갖는 것은 아니다. ( WHY….)
→ 즉 함수 객체는 constructor 일 수도 있고 non-constructor 일 수도 있다.

</aside>

### constructor와 non-constructor

```jsx
// CONSTRUCTOR
function defineContext() {}; // 함수 선언문

const expression = function() {}; // 함수 표현식

const obj = {
	x: function() {}; // 일반 함수를 값으로 가지는 프로퍼티.. ( 메서드로 인정하지 않는다. )
};

// NON-CONSTRUCTOR
const arrowFunc = () => {}; // 화살표 함수

const newObj = {
	x() {} // ES6 축약 표현만 메서드로 인정된다. (즉 NON-CONSTRUCTOR 다.)
};
```

### new 연산자

```jsx
function add(x, y) {
	return x + y;
}

// 일반 함수를 new 연산자와 함께 호출
let inst = new add();

console.log(inst); // {} -> 반환문이 무시되어 빈 객체 반환

// 객체를 반환하는 일반 함수
function createUser(name, role) {
	return { name, role };
}

// 일반 함수를 new 연산자와 함께 호출
inst = new createUser('Lee', 'Admin');

console.log(inst); // { name: 'Lee', role: 'Admin' }

```

```jsx
function Circle(radius) {
	this.radius = radius;
	this.getDiameter = function() {
		return 2 * this.radius;
	};
}

// new 연산자 없이 생성자 함수 호출
const circle = Circle(5);
console.log(circle); // undefined

// 일반 함수로 호출되었기때문에 전역 객체 프로퍼티에 값이 할당됨..
console.log(radius); // 5
console.log(getDiameter()); // 10

circle.getDiameter(); // Error -> getDiameter undefined
```

### new.target

> 위처럼 생성자 함수가 new 연산자 없이 호출되는 것을 방지하기 위해 파스칼 케이스 컨벤션을 사용한다.
( 생성자 함수 이름의 첫 문자를 대문자로 기술하는 방법 )
그러나 실수로 인한 호출이 발생할 수 있기때문에 ES6에서는 [new.target](http://new.target)을 지원한다.
> 

<aside>
💡 new.target은 this와 유사하게 constructor인 모든 함수 내부에서 암묵적인 지역 변수와 같이 사용된다.

</aside>

```jsx
function Circle(radius) {
	// 해당 함수가 new 연산자 없이 호출되었다면 new.target은 undefined를 반환한다.
	if (!new.target) {
		// new 연산자와 함께 생성자 함수를 재귀호출하여 생성된 인스턴스를 반환
		return new Circle(radius);
	}

	this.radius = radius;
	this.getDiameter = function() {
		return 2 * this.radius;
	};
}

const circle = Circle(5); // new 연산자 없이 호출해도 생성자 함수로서 호출된다.
console.log(circle.getDiameter()); // 10
```

<aside>
💡 IE 에서는 [new.target](http://new.target) 을 지원하지 않는다.
또한 대부분의 빌트인 생성자 함수는 new 연산자와 함께 호출되었는지를 확인한 후 적절한 값을 반환한다.
String, Number, Boolean 생성자 함수는 new 연산자 없이 호출 시 해당 타입의 값을 반환한다. 이를 통해 데이터 타입을 변환하기도 한다.

</aside>