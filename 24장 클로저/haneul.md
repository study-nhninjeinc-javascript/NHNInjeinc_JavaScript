# 클로저

> 클로저는 자바스크립트의 개념 중 하나지만 자바스크립트 고유의 개념이 아닌 함수형 프로그래밍 언어에서 사용되는 개념이다.
> 

## 렉시컬 스코프

> 13장에서 배웠던 렉시컬 스코프에 대해 다시한번 복습해 보자
> 

```jsx
// 함수 정의 시점에 상위 스코프가 결정되는 렉시컬 스코프
const x = 1;

function outerFunc() {
	const x = 2;
	innerFunc();
}

function innerFunc() {
	console.log(x);
}

outerFunc(); // 1
innerFunc(); // 1
```

## [[Environment]]

> 함수 객체의 내부 슬롯 [[Environment]] 는 자신이 정의된 환경, 즉 자신의 상위 스코프의 참조를 저장한다.
> 

<aside>
💡 함수 객체는 내부 슬롯 [[Environment]] 에 저장한 렉시컬 환경의 참조를 자신이 존재하는 한 기억한다.

</aside>

### 예제

```jsx
// 함수 정의 시점에 상위 스코프가 결정되는 렉시컬 스코프
const x = 1;

function outerFunc() {
	const x = 2;
	innerFunc();
}

function innerFunc() {
	console.log(x);
}

outerFunc(); // 1
```

### innerFunc 호출된 시점..

![24-1.PNG](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7911364a-e423-41bf-8f18-d785dae64eee/24-1.png)

## 클로저와 렉시컬 환경

> 클로저는 함수와 그 함수가 선언된 렉시컬 환경과의 조합이다. → 외부 함수보다 중첩 함수가 더 오래 유지되는 경우 생명 주기가 종료된 외부 함수의 변수를 참조하는 중첩함수를 클로저라고 부른다.
> 

### 예제

```jsx
const x = 1;

function outerFunc() {
	const x = 2;
	const innerFunc = function() { console.log(x); };
	return innerFunc;
}

const closureFunc = outerFunc();
closureFunc(); // 2
```

### outerFunc 함수 평가 시점

![24-2.PNG](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/945d1f99-507d-47de-9997-f06227a6786f/24-2.png)

### outerFunc 함수 호출(실행) 시점 → innerFunc 함수 평가 시점

> innerFunc 함수를 함수 표현식으로 정의했기 때문에 런타임에 평가된다.
> 

![24-3.PNG](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8a8f5244-1244-49eb-9195-435199b2c40f/24-3.png)

### outerFunc 함수 종료 시점 (POP)

> 함수가 종료되어 실행 컨텍스트 스택에서 제거(POP) 되지만 해당 함수의 렉시컬 환경까지 소멸하는 것은 아니다.
> 

![24-4.PNG](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3f35f8fd-e601-4ae9-8abb-e462a3492b9a/24-4.png)

### innerFunc 함수 호출(실행) 시점

> 함수 객체의 외부 렉시컬 환경 참조 값은 [[Environment]] 내부 슬롯에 저장되어 있는 참조값이 할당된다.
> 

![24-5.PNG](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2449f878-1c50-404f-9a89-b8766cc8b486/24-5.png)

<aside>
💡 이론적으로 모든 함수는 클로저다. 그러나 일반적으로 모든 함수를 클로저라고 하지 않는다.

</aside>

- 중첩 함수가 외부 함수보다 일찍 소멸되는 경우 클로저라고 부르기 적합하지 않다..
    - 중첩 함수가 외부 함수보다 늦게 소멸되더라도 상위 스코프의 식별자를 참조하고 있지 않다면 이 또한 클로저라 부를 수 없다.

## 활용

<aside>
💡 클로저는 상태를 안전하게 변경하고 유지하기 위해 사용한다. → 상태를 안전하게 은닉하고 특정 함수에게만 상태 변경을 허용하기 위해 사용한다.

</aside>

```jsx
let count = 0; // 어디서나 참조가 가능한 전역 변수 -> 의도치 않게 상태가 변경될 수 있다.

count increase = function () {
	return ++count;
};

console.log(increase()); // 1
console.log(increase()); // 2
console.log(increase()); // 3
```

```jsx
const increase = (function() {
	let count = 0;

	return function() {
		return ++count;
	};
}());

console.log(increase()); // 1
console.log(increase()); // 2
console.log(increase()); // 3

count = 0; // error -> 의도치 않은 변경을 걱정할 필요가 없다.
```

```jsx
function makeCounter(aux) {
	let counter = 0;

	return function() {
		counter = aux(counter);
		return counter;
	};
}

// 보조 함수
function increase(n) {
	return ++n;
}

function decrease(n) {
	return --n;
}

// 함수로 함수를 생성
const increaser = makeCounter(increase);
console.log(increaser()); // 1
console.log(increaser()); // 2

const decreaser = makeCounter(decrease);
console.log(decreaser()); // -1 -> increaser 함수의 렉시컬과 다른 렉시컬이 생성된다.
console.log(decreaser()); // -2
```

```jsx
const asyncCounter = (function () {
	let counter = 0;

	return function(aux) {
		counter = aux(counter);
		return counter;
	};
}());

// 보조 함수
function increase(n) {
	return ++n;
}

function decrease(n) {
	return --n;
}

console.log(asyncCounter(increase)); // 1
console.log(asyncCounter(increase)); // 2

// counter 변수의 상태가 공유된다.
console.log(asyncCounter(decrease)); // 1
console.log(asyncCounter(decrease)); // 0
```

## 캡슐화와 정보 은닉

> 캡슐화는 객체의 상태를 나타내는 프로퍼티와 프로퍼티를 참조하고 조작할 수 있는 동작인 메서드를 하나로 묶는 것을 말한다.
캡슐화는 객체의 특정 프로퍼티나 메서드를 감출 목적으로 사용하는데 이를 정보 은닉이라 한다. → 캡슐화는 정보 은닉을 위해 사용한다.
> 

<aside>
💡 일반적으로 객체지향 프로그래밍 언어에서는 클래스를 정의하고 그 클래스를 구성하는 멤버에 대하 접근 제한자를 선언하여 공개 범위를 한정할 수 있다. → 자바스크립트는 제공하지 않는다..

</aside>

```jsx
function Person(name, age) {
	// 생성자 함수 내부
	this.name = name; // public
	let _age = age; // private
}

// 프로토타입 메서드를 생성자 함수 외부에서 정의할 때 '_age' 지역 변수를 참조할 수 없다. -> 즉시 실행 함수로 감싸서 하나의 함수 내에 모아서 작성해야한다.
Person.prototype.sayHi = function() {
		console.log(`Hi! My name is ${this.name}. I am ${_age}.`);
};

const me = new Person('Lee', 20);
me.sayHi(); // Hi! My name is Lee, I am 20
console.log(me.name); // Lee
console.log(me._age); // undefined -> 생성자 함수의 지역변수이므로 외부에서 참조하거나 변경할 수 없다.
```

```jsx
const Person = (function() {
	let _age = 0; // private
	
	// 생성자 함수
	function Person(name, age) {
		this.name = name;
		_age = age;
	}

	Person.prototype.sayHi = function() {
		console.log(`Hi, My name is ${this.name}. I am ${_age}.`);
	};

	return Person;
}());

const me = new Person('Lee', 20);
me.sayHi();
console.log(me.name); // Lee
console.log(me._age); // undefined

const you = new Person('Kim', 30);
you.sayHi();
console.log(you.name); // Kim
console.log(you._age); // undefined

me.sayHi(); // _age 변수의 상태가 유지되지 않는다 (you 인스턴스를 생성할 때 변경되어버렸다..)
```

<aside>
💡 위 예제처럼 자바스크립트는 정보 은닉을 완전하게 지원하진 않는다. → 자세한 내용은 25장에서 확인이 가능하다고 한다.

</aside>

## 자주 발생하는 실수

```jsx
var funcs = [];

for (var i = 0; i < 3; i++) {
	funcs[i] = function () { return i; };
}

for (var j = 0; j < funcs.length; j++) {
	console.log(funcs[j]()); // 3..
}
```

> 위 예제에서 for 문의 var 키워드로 선언한 i 변수는 함수 레벨 스코프를 갖기 때문에 전역 변수로 취급한다. → funcs 배열에 있는 함수를 호출하면 전역 변수의 i 를 반환하게 된다.
> 

```jsx
// 클로저를 사용해 0, 1, 2 를 출력해보자
let funcs = [];

for (var i = 0; i < 3; i ++) {
	funcs[i] = (function (id) { 
		return function() {
			return  id;
		};
	}(i));
}

for (var j = 0; j < funcs.length; j++) {
	console.log(funcs[j]());
}

// 혹은 let 키워드를 사용하면 let 키워드는 블록 레벨 스코프기 때문에 반복문이 실행될 때마다 새로운 렉시컬 환경이 생성된다. -> funcs 의 요소가 각 렉시컬 환경을 참조하고 있으니 각 렉시컬 환경에 대한 클로저가 생성된다.
for (let i = 0; i < 3; i++) {
	funcs[i] = function() { return i; };
}

for (let i = 0; i < funcs.length; i++) {
	console.log(funcs[i]());
}
```

<aside>
💡 또 다른 방법으로 함수형 프로그래밍 기법인 고차 함수를 사용하는 방법도 있다. (27장에 자세하게 다룬다고 한다.)

</aside>