# 22. this

```jsx
const circle = {
	radius = 5,
	getDiameter() {
		// 메서드가 자신이 속한 객체의 프로퍼티나 다른 메서드를 참조하려면 자신이 속한 객체를 참조할 수 있어야 한다.
		return 2 * circle.radius;
	}
};

// 위 객체 리터럴 방식처럼 식별자를 통해 재귀적으로 자신이 속한 객체를 참조하는 방식은 일반적이지 않고 바람직하지 못하다.

function Circle(radius) {
	// 생성자 함수는 new 연산자를 통해 객체를 생성한다.
	// 즉 아직 new 연산자를 통해 객체가 생성되지 않아 어떤 식별자를 통해 참조해야 할지 알 수 없다..
	????.radius = radius;
	????.getDiameter = function() {
		return 2 * ????.radius;
	};
}

const instance1 = new Circle(5);
```

```jsx
// this는 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기 참조 변수다.
// this는 함수 호출 방식에 의해 동적으로 결정된다.
const liObj = {
	radius = 5,
	getDiameter() {
		console.log(this);
		return 2 * this.radius;
	}
};

// liObj 의 메서드인 getDiameter 함수에서 this 는 해당 메서드를 호출한 객체를 가리킨다.
console.log(liObj.getDiameter()); //  { radius: 5, getDiameter: f } 10

function Circle(radius) {
	// 생성자 함수에서 this 는 생성자 함수가 생성할 인스턴스를 가리킨다.
	this.radius = radius;
}

Circlr.prototype.getDiameter = function () {
	console.log(this);
	return 2 * this.radius;
};

const inst1 = new Circle(5);
const inst2 = new Circle(10);

console.log(inst1.getDiameter()); // Circle { radius: 5 } 10
console.log(inst2.getDiameter()); // Circle { radius: 10 } 20
```

## 함수 호출 방식과 this 바인딩

<aside>
💡 함수의 상위 스코프를 결정하는 방식인 렉시컬 스코프는 함수 정의가 평가되어 함수 객체가 생성되는 시점에 상위 스코프를 결정하고 this 바인딩은 함수 호출 시점에 결정된다.

</aside>

### 일반 함수 호출

> 기본적으로 this에는 전역 객체가 바인딩된다.
> 

```jsx
function outerFunc() {
	console.log("outerFunc's this : " + this);
	function innerFunc() {
		console.log("innerFunc's this : " + this);
	}	
	innerFunc();
}

outerFunc();
/*
outerFunc's this : [object Window]
innerFunc's this : [object Window]
*/

// 엄격 모드가 적용된 일반 함수 내부 this 에는 undefined 가 바인딩된다.
function strictFunc() {
	'use strict';
	console.log("strictFunc's this : " + this);
}

strictFunc(); // strictFunc's this : undefined

// 메서드 내부의 중첩 함수나 콜백 함수의 this 에도 전역 객체가 바인딩 되기 때문에 이를 메서드나 콜백 함수의 this 바인딩과 일치시키는 편이 좋다.
var value = 1;

const obj = {
	value : 100,
	objMethod() {
		// 객체를 가리키고 있는 this 바인딩을 변수에 할당한다.
		const that = this;

		setTimeout(function() {
			console.log(that.value); // 100
		}, 100);
	}
};
```

### 메서드 호출

> 메서드 내의 this 는 메서드를 호출한 객체를 가리킨다.
> 

```jsx
const person = {
	name: 'Lee',
	getName() {
		return this.name;
	}
};

console.log(person.getName()); // Lee

const anotherPerson = { name: 'Kim' };
anotherPerson.getName = person.getName;

console.log(anotherPerson.getName()); // Kim
```

### 생성자 함수 호출

> 생성자 함수 내부 this 에는 생성자 함수가 생성할 인스턴스가 바인딩된다.
> 

```jsx
// 생성자 함수
function Person(name) {
	this.name = name;
	this.getName = function() {
		return this.name;
	};
}

const person1 = new Person('Lee');
const person2 = new Person('Kim');

console.log(person1.getName()); // Lee
console.log(person2.getName()); // Kim
```

### apply/call/bind 메서드에 의한 간접 호출

> apply/call/bind 메서드는 Function.prototype 객체의 메서드다.
> 

```jsx
function getThisBinding() {
	return this;
}

const thisArg = { a: 1 };

console.log(getThisBinding()); // Window

// apply 메서드는 this로 사용할 객체와 함수에게 전달할 인수 리스트를 전달받아 지정한 함수를 호출한다.
console.log(getThisBinding.apply(thisArg)); // { a: 1 }

// call 메서드는 this로 사용할 객체와 ',' 로 구분된 인수 리스트를 전달받아 지정한 함수를 호출한다.
console.log(getThisBinding.call(thisArg)); // { a: 1 }

// apply 메서드의 인수 전달 방식
console.log(getThisBinding.apply(thisArg, [1, 2, 3]));

// call 메서드의 인수 전달 방식
console.log(getThisBinding.call(thisArg, 1, 2, 3));

// 대표적인 활용법
function convertArgsToArray() {
	console.log(arguments);

	// arguments 객체를 배열로 변환 ( arguments 는 배열이 아니기 때문에 Array.prototype 객체 내의 메서드를 사용할 수 없다. )
	const arr = Array.prototype.slice.call(arguments); // slice 메서드를 인수없이 호출하면 배열의 복사본을 생성한다.
	console.log(arr);

	return arr;
}

convertArgsToArray(1, 2, 3); // [1, 2, 3]
```

```jsx
// bind 메서드는 함수를 호출하지 않고 첫 번째 인수로 전달한 값으로 this 바인딩이 교체된 함수를 새롭게 생성해 반환해준다.
function getThisBinding() {
	return this;
}

const thisArgs = { a: 1 };

console.log(getThisBinding.bind(thisArgs)); // getThisBinding

// bind 메서드는 함수를 호출하지 않으므로 명시적으로 호출해주어야 한다.
console.log(getThisBinding.bind(thisArgs)()); // { a: 1 }

// bind 의 활용법
const person = {
	name: 'Lee',
	personFunc(callback) {
		// bind 메서드로 callback 함수 내부의 this 바인딩을 전달한다. 
		setTimeout(callback.bind(this), 100);
	}
};

person.personFunc(function () {
	console.log('Hi! my name is ' + this.name);
});
```