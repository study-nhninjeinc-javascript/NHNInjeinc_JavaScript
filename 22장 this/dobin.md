# 22장 this

### this 키워드란

자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 특수한 식별자가 필요하다. 

이를 위해 자바스크립트는 this라는 특수한 식별자를 제공한다.

this는 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기 참조 변수다. this를 통해 자신이 속한 객체 또는 자신이 생성할 인스턴스의 프로퍼티나 메서드를 참조할 수 있다.

this가 가리키는 값, 즉 this 바인딩은 함수 호출 방식에 의해 동적으로 결정된다.

(바인딩이란 식별자와 값을 연결화는 과정을 의미)

💡 예제 

```jsx
// 객체 리터럴
const circle = {
	radius : 5,
	getDiameter() {
		// this는 메서드를 호출한 객체를 가리킨다.
		return 2 * this.radius;
	}
}

console.log(circle.getDiameter()); // 10

// 생성자 함수
function Circle(radius) {
	// this는 생성자 함수가 생성할 인스턴스를 가리킨다.
	this.radius = radius;
}

Circle.prototype.getDiameter = function() {
	// this는 생성자 함수가 생성한 인스턴스를 가리킨다.
	return 2 * this.radius;
}

// 인스턴스 생성
const circle = new Circle(5);
console.log(circle.getDiameter()); // 10
```

위 처럼 this는 상황에 따라 가리키는 대상이 다르다. ( 함수가 호출되는 방식에 따라 this에 바인딩 될 값을 동적으로 결정)

### 함수 호출 방식과 this 바인딩

렉시컬 상위 스코프는 함수 정의가 평가되어 함수 객체각 생성되는 시점에 상쉬 스코프를 결정한다. 하지만 this 바인딩은 함수 호출 시점에 결정된다.

함수 호출 방식은 다음과 같다.

- 일반 함수 호출
- 메서드 호출
- 생성자 함수 호출
- Function.prototype.apply/call/bind 메서드에 의한 간접 호출

💡 일반 함수 호출

```jsx
// 기본적으로 this에는 전역 객체가 바인딩된다. (콜백 함수도 마찬가지)
function foo() {
	console.log("foo's this : ", this); // window
	function bar() {
		console.log("bar's this : ", this); // window
	}
	bar();
}
foo();
```

💡 메서드 호출

메서드 내부의 this에는 메서드를 호출한 객체 , 즉 메서드 이름 앞의 마침표 연산자 앞에 기술한 객체가 바인딩 됨. (메서드 내부의 this는 메서드를 소유한 객체가 아닌 메서드를 호출한 객체에 바인딩 된다.) 

```jsx
const person = {
	name : "shin",
	getName() {
		// 메서드 내부의 this는 메서드를 호출한 객체에 바인딩된다.
		return this.name;
	}
};

// 메서드 getName을 호출한 객체는 person이다.
console.log(person.getName()); // shin

const anoterPerson = {
	name : "kim"
};

// getName 메서드를 anoterPerson 객체의 메서드로 할당
anotherPerson.getName = person.getName;

// getName 메서드를 호출한 객체는 anotherPerson이다.
console.log(anotherPerson.getName()); // kim

// getName 메서드를 변수에 할당
const getName = person.getName;

// getName 메서드를 일반 함수로 호출
console.log(getName()); // ''

// 일반 함수로 호출된 getName 함수 내부의 this.name은 브라우저 환경에서 window.name과 같음
```

위 예제의 내부 모습

![https://dev-dobin.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fb108f4eb-dd6c-4677-a96d-dee35b7abfe3%2FUntitled.png?id=f8117355-26c7-4587-b558-08df8f85b56e&table=block&spaceId=f800ef36-5d6f-4e98-a2f1-f4b2d48cc3fe&width=1030&userId=&cache=v2](https://dev-dobin.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fb108f4eb-dd6c-4677-a96d-dee35b7abfe3%2FUntitled.png?id=f8117355-26c7-4587-b558-08df8f85b56e&table=block&spaceId=f800ef36-5d6f-4e98-a2f1-f4b2d48cc3fe&width=1030&userId=&cache=v2)

프로토타입 메서드 내부에서 사용된 this도 일반 메서드와 마찬가지로 해당 메서드를 호출한 객체에 바인딩 된다.

```jsx
function Person(name) {
	this.name = name;
}

Person.prototype.getName = function () {
	return this.name;
};

const me = new Person('shin');

// getName 메서드를 호출한 객체는 me다.
console.log(me.getName()); // shin

Person.prototype.name = 'kim';

// getName 메서드를 호출한 객체는 Person.prototype이다.
console.log(Person.prototype.getName()); // kim
```

위 예제의 내부 모습

![Untitlhttps://dev-dobin.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F8bcd4b05-f947-49e4-868f-7d63cff435f3%2FUntitled.png?id=ca680c7d-9621-4a81-bb6c-4319453c8870&table=block&spaceId=f800ef36-5d6f-4e98-a2f1-f4b2d48cc3fe&width=1130&userId=&cache=v2ed](https://dev-dobin.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F8bcd4b05-f947-49e4-868f-7d63cff435f3%2FUntitled.png?id=ca680c7d-9621-4a81-bb6c-4319453c8870&table=block&spaceId=f800ef36-5d6f-4e98-a2f1-f4b2d48cc3fe&width=1130&userId=&cache=v2)

💡 생성자 함수 호출

```jsx
// 생성자 함수
function Circle(radius) {
	// 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킴
	this.radius = radius;
	this.getDiameter = function() {
		return 2 * this.radius;
	}
}

// 반지름이 5인 Circle 객체를 생성
const circle1 = new Circle(5);
// 반지름이 10인 Circle 객체를 생성
const circle2 = Circle(10);

console.log(circle1.getDiameter()); // 10
console.log(circle2); // undefined
console.log(radius); // 10
```

💡 Funtionc.prototype.apply/call/bind 메서드에 의한 간접 호출

apply, call, bind 메서드는 Function.prototype의 메서드다. 즉, 이들 메서드는 모든 함수가 상속받아 사용할 수 있다.

![https://dev-dobin.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F16588b2e-291e-4d4d-947a-614ef1d7aec4%2FUntitled.png?id=0e72cabd-4a9b-4813-bb22-8ffdf64d23c8&table=block&spaceId=f800ef36-5d6f-4e98-a2f1-f4b2d48cc3fe&width=740&userId=&cache=v2](https://dev-dobin.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F16588b2e-291e-4d4d-947a-614ef1d7aec4%2FUntitled.png?id=0e72cabd-4a9b-4813-bb22-8ffdf64d23c8&table=block&spaceId=f800ef36-5d6f-4e98-a2f1-f4b2d48cc3fe&width=740&userId=&cache=v2)

```jsx
function getThisBinding () {
	return this;
}

// this로 사용할 객체
const thisArg = { a : 1 };

console.log(getThisBinding()); // window

// getThisBinding 함수를 호출하면서 인수로 전달한 객체를 getThisBinding 함수의 this에 바인딩 한다.
console.log(getThisBinding.apply(thisArg)); // { a : 1}
console.log(getThisBinding.call(thisArg)); // { a : 1 }
```

apply와 call 메서드의 본질적인 기능은 함수를 호출하는 것이다. apply call 메서드는 함수를 호출하면서 첫 번째 인수로 전달한 특정 객체를 호출한 함수의 this에 바인딩한다.

apply와 call 메서드는 호출할 함수에 인수를 전달하는 방식만 다를 뿐 동일하게 동작한다.

위 예제는 호출할 함수, 즉 getThisBinding 함수에 인수를 전달하지 않는다. 

💡 getThisBinding에 인수를 전달할 경우

```jsx
function getThisBinding() {
	console.log(arguments);
	return this;
}

// this로 사용할 객체
const thisArg = { a : 1 };

// getThisBinding 함수를 호출하면서 인수로 전달한 객체를 getThisBinding 함수의 this에 바인딩 한다.
// apply 메서드는 호출할 함수의 인수를 배열로 묶어 전달한다.
console.log(getThisBinding.apply(thisArg, [1,2,3]));
// Arguments(3) [1,2,3, callee : f, Symbol(Symbol.iterator) : f ]
// {a:1}

console.log(getThisBinding.call(thisArg, 1,2,3));
// Arguments(3) [1,2,3, callee : f, Symbol(Symbol.iterator) : f ]
// {a:1}
```

apply 메서드는 호출할 함수의 인수를 배열로 묶어 전달한다.  call 메서드는 호출할 함수의 인수를 쉼표로 구분한 리스트 형식으로 전달한다. 이처럼 apply와 call 메서드는 호출할 함수에 인수를 전달하는 방식만 다를 뿐 this로 사용할 객체를 전달하면서 함수를 호출하는 것은 동일하다.

💡 Function.prototype.bind 메서드는 apply와 call 메서드와 달리 함수를 호출하지 않는다.

다만 첫번째 인수로 전달한 값으로 this방인딩이 교체된 함수를 새롭게 생성해 반환한다.

```jsx
function getThisBinding() {
	return this;
}

// this로 사용할 객체
const thisArg = { a : 1 };

// bind 메서드는 첫 번재 인수로 전달한 thisArg로 this 바인딩이 교체된 getThisBinding 함수를 새롭게 생성해 반환
console.log(getThisBinding.bind(thisArg)); // getThisBiding
// bind 메서드는 함수를 호출하지는 않으므로 명시적으로 호출해야 한다.
console.log(getThisBinding.bind(thisArg)()); // { a : 1 }
```

💡 bind 메서드는 메서드의 this와 메서드 내부의 중첩 함수 또는 콜백 함수의 this가 불일치하는 문제를 해결하기 위해 유용하게 사용된다.

```jsx
const person = {
	name : 'shin',
	foo(callback) {
		setTimeout(callback, 100);
	}
};

person.foo(function() {
	console.log('dobin ${this.name}');
});
```

- 일반 함수로 호출된 콜백 함수 내부의 this.name은 전역객체를 바라본다.
- 브라우저 환경일때 this.[name](http://window.name) == ‘ ’ , Node.js 환경은 [this.name](http://this.name) == undefined다.

person.foo의 콜백 함수가 호출되기 이전인 setTimeout 함수를 호출하기 직전까지는

this는 foo 메서드를 호출한 객체, 즉 person 객체를 가리킨다.

그러나 person.foo의 콜백 함수가 일반 함수로서 호출된 (익명함수 안의 consle.log 부분) 시점에서 this는 전역 객체를 가리킨다. 

이때 위 예제에서 [person.foo](http://person.foo)의 콜백함수는 외부 함수 person.foo를 돕는 헬퍼 함수(보조 함수) 역할을 하기 때문에 외부 함수 person.foo 내부의 this와 콜백 함수 내부의 this가 상이하면 문맥상 문제가 발생한다. 따라서 콜백 함수 내부의 this를 외부 함수 내부의 this와 일치시켜야 한다. 이때 bind 메서드를 사용하여 this를 일치시킬 수 있다.

```jsx
const person = {
	name : 'shin',
	foo(callback) {
		// bind 메서드로 callback 함수 내부의 this 바인딩을 전달
		setTimeout(callback.bind(this), 100);
	}
};

person.foo(function() {
	console.log('dobin ${this.name}'); // dobin shin
});
```

🎯 함수 호출방식에 따른 this 바인딩 총정리
![https://dev-dobin.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F55a932e7-c257-4a32-9b01-e79e63114fcb%2FUntitled.png?id=7a329715-7fd3-4efe-8f1d-8a937a24ccf1&table=block&spaceId=f800ef36-5d6f-4e98-a2f1-f4b2d48cc3fe&width=1120&userId=&cache=v2](https://dev-dobin.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F55a932e7-c257-4a32-9b01-e79e63114fcb%2FUntitled.png?id=7a329715-7fd3-4efe-8f1d-8a937a24ccf1&table=block&spaceId=f800ef36-5d6f-4e98-a2f1-f4b2d48cc3fe&width=1120&userId=&cache=v2)