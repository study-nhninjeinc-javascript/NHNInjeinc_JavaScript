# 19장 프로토타입

<aside>
💡 ES6에서 클래스가 도입되었다. 이는 새로운 객체 생성 메커니즘이라고 볼 수 있으며 이는 25장에서 자세히 알아보자..

</aside>

> 자바스크립트를 이루고 있는 거의 모든 것이 객체다! → 원시 타입의 값을 제외한 나머지 값들은 모두 객체다.
> 

## 객체지향 프로그래밍

> 객체의 집합으로 프로그램을 표현하려는 프로그래밍 패러다임을 말한다.
> 
- 실세계의 실체를 인식하는 사고를 프로그래밍에 접목 → 객체
    - 실체는 특징이나 성질을 나타내는 속성을 가진다.

<aside>
💡 다양한 속성 중 프로그램에 필요한 속성만 간추려 내어 표현하는 것을 추상화라고 한다.

</aside>

```jsx
const person = {
	name: 'Lee',
	address: 'Seoul',
	getInfo() {
		return 'name : ' + this.name + ', address:' + this.address;
	}
};
```

- 객체는 상태를 나타내거나 이를 조작할 수 있는 동작, 즉 프로퍼티와 메서드를 하나의 논리적인 단위로 묶은 복합적인 자료구조라 할 수 있다.

## 상속과 프로토타입

> 상속은 어떤 객체의 프로퍼티 또는 메서드를 다른 객체가 상속받아 그대로 사용할 수 있는 것을 말한다.
> 
- 자바스크립트는 프로토타입을 기반으로 상속을 구현하여 불필요한 중복을 제거할 수 있다.

```jsx
// 생성자 함수
function Circle(radius) {
	this.radius = radius;
	this.getArea = function() { // PI = 원주율
		return Math.PI * this.radius ** 2;
	};
}

// 반지름이 서로 다른 인스턴스를 생성자 함수를 통해 생성
const circle1 = new Circle(1);
const circle10 = new Circle(10);

// 동일한 동작을 하는 getArea 메서드를 두 인스턴스가 중복 소유한다.
console.log(circle1.getArea === circle10.getArea); // false
```

```jsx
function Circle(radius) {
	this.radius = radius;
}

/* 
Circle 생성자 함수가 생성한 모든 인스턴스가 getArea 메서드를 사용할 수 있도록 프로토타입에 추가한다.
*/
Circle.prototype.getArea = function() {
	return Math.PI * this.radius ** 2;
};

const circle1 = new Circle(1);
const circle10 = new Circle(10);

// 모든 인스턴스가 하나의 동일한 메서드를 공유한다.
console.log(circle1.getArea === circle2.getArea); // true

```

![19.1.PNG](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9875b910-253b-43db-a505-d60b6eaa01fb/19.1.png)

![19.2.PNG](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ef6041a4-1981-4d2d-9bad-52f710a7b9fd/19.2.png)

## 프로토타입 객체

> 모든 객체는 [[Prototype]] 내부 슬롯을 가지며 이는 프로토타입의 참조를 값으로 가진다.
> 

### __proto__

> 모든 객체는 __proto__ 접근자 프로퍼티를 통해 자신의 프로토타입에 간접적으로 접근할 수 있다.
> 
- __proto__ 는 접근자 프로퍼티다.
    - 접근자 프로퍼티를 통해 프로토타입에 접근하면 내부적으로 getter 함수가 호출된다.
    - 접근자 프로퍼티를 통해 새로운 프로토타입을 할당하면 setter 함수가 호출된다.
- 상속을 통해 사용된다.
    
    ```jsx
    const person = { name: 'Lee' };
    
    console.log(person.hasOwnProperty('__proto__')); // false
    ```
    
- 상호 참조에 의해 프로토타입 체인이 생성되는 것을 방지한다.
    
    ```jsx
    const parent = {};
    const child = {};
    
    child.__proto__ = parent;
    parent.__proto__ = child; // Error -> Cyclic __proto__ value
    ```
    
- __proto__ 접근자 프로퍼티를 코드 내에서 직접 사용하는것은 권장되지 않는다.
    - 모든 객체가 __proto__ 접근자 프로퍼티를 사용할 수 있는 것이 아니기 때문
    
    ```jsx
    const obj = Object.create(null); // Object._proto__ 를 상속받을 수 없다.
    
    console.log(obj.__proto__); // undefined
    
    console.log(Object.getPrototpyeOf(obj)); // null -> 해당 메서드를 통해 참조
    ```
    

### 함수 객체의 prototype 프로퍼티

> 함수 객체만이 소유하는 prototype 프로퍼티는 생성자 함수가 생성할 인스턴스의 프로토타입을 가리킨다.
> 

<aside>
💡 생성자 함수로 객체를 생성하면 __proto__ 접근자 프로퍼티와 prototype 프로퍼티는 결국 동일한 프로토타입을 가리킨다.

</aside>

```jsx
function Person(name) {
	this.name = name;
}

const me = new Person('Lee');
```

![19-1.PNG](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/671a40a1-a74b-4782-bf16-9f9172f591b6/19-1.png)

### 프로토타입의 constructor 프로퍼티와 생성자 함수

> 모든 프로토타입은 constructor 프로퍼티를 갖는다. → constructor 프로퍼티는 prototype 프로퍼티로 자신을 참조하고 있는 생성자 함수를 가리킨다.
> 

![19-1.PNG](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/671a40a1-a74b-4782-bf16-9f9172f591b6/19-1.png)

- 생성자 함수에 의해 생성된 인스턴스는 프로토타입의 constructor 프로퍼티에 의해 생성자 함수와 연결된다.

## 리터럴 표기법에 의해 생성된 객체의 생성자 함수와 프로토타입

```jsx
// 생성자 함수를 통해 생성한 객체
const c = new Object();

// 객체 리터럴로 생성한 객체
const o = {};

console.log(c.constructor === Object); // true

console.log(o.constructor === Object); // true
```

🔎 Object 생성자 함수에 인수를 전달하지 않거나 undefined 또는 null 을 인수로 전달하면서 호출하면 내부적으로 추상 연산 OrdinaryObjectCreate 를 호출하여 Object.prototype을 프로토타입으로 갖는 빈 객체를 생성한다.

| 리터럴 표기법 | 생성자 함수 | 프로토타입 |
| --- | --- | --- |
| 객체 리터럴 | Object | Object.prototype |
| 함수 리터럴 | Function | Function.prototype |
| 배열 리터럴 | Array | Array.prototype |
| 정규 표현식 리터럴 | RegExp | RegExp.prototype |

<aside>
💡 프로토타입과 생성자 함수는 단독으로 존재할 수 없고 언제나 쌍으로 존재한다.

</aside>

## 프로토타입의 생성 시점

> 프로토타입은 생성자 함수가 생성되는 시점에 더불어 생성된다.
> 

### 사용자 정의 생성자 함수와 프로토타입 생성 시점

```jsx
// 함수의 정의가 평가되어 함수 객체를 생성하는 시점에 프로토타입도 같이 생성된다.
console.log(Person.prototype); // {constructor: f}

function Person(name) {};
```

<aside>
💡 프로토타입도 객체이고 모든 객체는 프로토타입을 가진다. 생성된 프로토타입의 프로토타입은 언제나 Object.prototype 이다.

</aside>

### 빌트인 생성자 함수와 프로토타입 생성 시점

> 모든 빌트인 생성자 함수는 전역 객체가 생성되는 시점에 생성된다.
> 

<aside>
💡 전역 객체는 코드가 실행되지 이전 단계에 자바스크립트 엔진에 의해 생성되는 객체이다.

</aside>

## 객체 생성 방식과 프로토타입의 결정

### 객체 리터럴

```jsx
const obj = { x: 1 };

console.log(obj.constructor === Object); // true
console.log(obj.hasOwnProperty('constructor'); // false
```

### Object 생성자 함수

```jsx
const obj = new Object();
obj.x = 1;

console.log(obj.constructor === Object); // true
console.log(obj.hasOwnProperty('constructor')); false;
```

### 사용자 정의 생성자 함수

```jsx
function Person(name) { this.name = name; }

const obj = new Person('Lee');

console.log(obj.hasOwnProperty('constructor')); //true

console.log(obj.constructor === Object); // false

console.log(obj.constructor === Person); // true

// 프로토타입 메서드
Person.prototype.sayHello = function() {
	console.log(`Hello! My name is ${this.name}`);
};

const newObj = new Person('Kim');

// 프로토타입에 추가된 메서드를 상속받아 자신의 메소드처럼 사용할 수 있다.
obj.sayHello();
newObj.sayHello();
```

## 프로토타입 체인

```jsx
function Person(name) { this.name = name; }

Person.prototype.sayHello = function() {
	console.log(`Hello! ${this.name}`);
}

const obj = new Person('Lee');

// hasOwnProperty 는 Object.prototype의 메서드다 -> 상속을 받았다
console.log(obj.hasOwnProperty('name')); // true
```

![19-3.PNG](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a35b59c0-b8df-4c9b-9135-5ecb481b4d38/19-3.png)

### 프로토타입 검색 순서

1. 프로퍼티 참조 혹은 메서드를 호출한 객체에서 먼저 해당 프로퍼티나 메서드가 있는지 검색한다. (hasOwnProperty를 찾는다고 가정한다.)
2. 해당 메서드가 없기 때문에 프로토타입 체인을 따라 Person의 프로토타입 객체로 이동하여 해당 메서드를 검색한다.
3. Person의 프로토타입 객체에도 해당 메서드가 없기때문에 다시 한번 프로토타입 체인을 따라 상위 프로토타입인 Object.prototype 객체로 이동하여 검색한다.
4. 이때 Object.prototype 에는 해당 메서드가 존재하기 때문에 자바스크립트 엔진은 해당 메서드를 호출하며 이때 호출되는 메서드의 this에는 최초 호출한 객체가 바인딩된다. 
    - 위 예제에서는 obj
    - Object.prototype은 프로토타입의 종점이다.

<aside>
💡 프로토타입 체인은 상속과 프로퍼티 검색을 위한 메커니즘이고 스코프 체인은 식별자 검색을 위한 메커니즘이다.

</aside>

## 오버라이딩과 프로퍼티 섀도잉

```jsx
const Person = (function () {
	// 생성자 함수 Person
	function Person(name) {
		this.name = name;
	}

	Person.prototype.sayHello = function() {
		console.log(`Hi! ${this.name}`);
	};

	// 생성자 함수를 반환
	return Person;
}());

const p1 = new Person('Lee');

p1.sayHello(); // Hi! Lee

// 인스턴스의 메서드
p1.sayHello = function () {
	console.log(`Hello! ${this.name});
};

p1.sayHello(); // Hello! Lee
```

> 상위(프로토타입) 프로퍼티는 프로퍼티 섀도잉, 하위(인스턴스) 프로퍼티는 오버라이딩 된다.
> 

<aside>
💡 상속 관계에 의해 프로퍼티가 가려지는 현상을 프로퍼티 섀도잉이라 한다.

</aside>

- 동일한 이름의 프로퍼티를 하위 객체에서 추가할 순 있지만 하위 객체가 상위 객체의 프로퍼티를 삭제할 수 없다.
    - get 액세스는 허용되나 set 엑세스는 허용되지 않는다.
    
    ** 프로토타입 체인을 통해서 안되는 것이고 프로토타입에 직접 접근해서 삭제할 순 있다.
    

## 프로토타입의 교체

> 프로토타입은 임의의 다른 객체로 변경할 수 있다. → 부모 객체를 동적으로 변경할 수 있다.
> 

### 생성자 함수에 의한 프로토타입 교체

```jsx
const Person = (function () {
	function Person(name) { this.name = name; }

	Person.prototype = {
		// constructor: Person,
		sayHello() {
			console.log(`Hi!, ${this.name});
		}
	};

	return Person;
}());

const obj = new Person('Lee');

console.log(obj.constructor === Person); // false
```

### 인스턴스에 의한 프로토타입의 교체

```jsx
function Person(name) { this.name = name; }

const obj = new Person('Lee');

const parent = {
	sayHello() {
		console.log('Hi! ${this.name});
	}
};

Object.setPrototypeOf(obj, parent);
// obj.__proto__ = parent;

console.log(obj.constructor === Person); // false
```

## instanceof 연산자

> 객체 instanceof 생성자 함수
> 

 생성자 함수의 prototype에 바인딩된 객체가 좌측 객체의 프로토타입 체인 상에 존재하면 true 아니면 false로 평가된다.

```jsx
function Person(name) {
	this.name = name;
}

const obj = new Person('Lee');

console.log(obj instanceof Person); // true

console.log(obj instanceof Object); // true
```

## 직접 상속

### Object.create

```jsx
let obj = Object.create(null); // 프로토타입이 null 인 객체를 생성한다.
console.log(Object.getPrototypeOf(obj) === null); // true

obj = Object.create(Object.prototype, { x: { value: 1 } }); // 프로토타입이 Object.prototype 이고 x 프로퍼티를 가지는 객체가 생성된다.
```

### 장점

- new 연산자 없이 객체 생성 가능
- 프로토타입 지정하면서 생성 가능
- 객체 리터럴에 의해 생성된 객체를 상속받을 수 있음

<aside>
💡 Object.prototype의 빌트인 메서드를 객체가 직접 호출하는 것을 권장하지 않는다. → 간접적으로 호출하는 것이 좋다. ( Object.prototype.hasOwnProperty.call() )

</aside>

### 객체 리터럴 내부에서 __proto__ 에 의한 직접 상속

<aside>
💡 ES6 이후에는 객체 리터럴 내부에서 __proto__ 접근자 프로퍼티를 사용하여 직접 상속을 구현할 수 있다.

</aside>

```jsx
const myProto = { x : 10 };

const obj = {
	y: 20,

	__proto__: myProto
};
```

## 정적 프로퍼티/메서드

```jsx
function Person(name) {
	this.name = name;
}

Person.prototype.sayHello = function() {
	console.log(`Hi! ${this.name});
};

// 정적 프로퍼티
Person.staticProp = 'static prop';

Person.staticMethod = function() {
	console.log('staticMethod');
};

const obj = new Person('Lee');

// 생성자 함수에 추가한 정적 프로퍼티와 메서드는 생성자 함수로 참조, 호출한다.
Person.staticMethod(); // staticMethod

// 정적 프로퍼티와 메서드는 생성자 함수가 생성한 인스턴스로는 참조, 호출할 수 없다.
obj.staticMethod(); // Error : is not a function
```

<aside>
💡 인스턴스/프로토타입 메서드 내에서 this를 사용하지 않는다면 해당 메서드는 정적 메서드로 변경할 수 있다.

</aside>

```jsx
function CreateFunc() {}

// this를 참조하지 않는 프로토타입 메서드 -> 정적 메서드로 변경해도 동일하게 동작한다.
CreateFunc.prototype.x = function() {
	console.log('x'); // this.x 가 아닌 문자열 x를 출력
};

// CreateFunc.x(); Error -> 프로토타입 메서드를 호출하기 위해선 인스턴스를 생성해야한다..

const obj = new CreateFunc();

obj.x(); // x

CreateFunc.x = obj.x;

CreateFunc.x(); // 정적 메서드는 인스턴스를 생성하지 않고 호출할 수 있다.
```

<aside>
💡 프로토타입 프로퍼티나 메서드를 표기할 때 prototype을 #으로 표기하는 경우도 있다. (Object#isPrototypeOf)

</aside>

## 프로퍼티 존재 확인

### in 연산자

```jsx
const person = {
	name: 'Lee',
	address: 'Seoul'
};

console.log('name' in person); // true

console.log('address' in person); //true

console.log('age' in person); // false

// in 연산자는 상속받은 모든 프로토타입의 프로퍼티를 확인한다.
console.log('toString' in person); // true
```

```jsx
// ES6 에서 도입된 Reflect.has 메서드
const person = { name: 'Lee' };

console.log(Reflect.has(person, 'name')); // true
console.log(Reflect.has(person, 'toString')); // true
// in 연산자와 동일하게 동작한다.
```

### Object.prototype.hasOwnProperty 메서드

```jsx
console.log(person.hasOwnProperty('name')); // true
console.log(person.hasOwnProperty('toString')); // false
```

## 프로퍼티 열거

### for … in 문

```jsx
const person = {
	name: 'Lee',
	address: 'Seoul'
};

for (const key in person) { // key 변수에 person 객체의 프로퍼티 키가 반복하며 할당된다.
	console.log(key + ': ' + person[key]
}
```

<aside>
💡 for … in 문은 객체가 상속 받은 프로퍼티까지 열거한다.

</aside>

### Object.keys/values/entries 메서드

```jsx
// for ... in 문은 상속받은 프로퍼티도 열거하기 때문에 객체 자신의 고유 프로퍼티만 열거하는 경우 사용하기 좋다.
const person = {
	name: 'Lee',
	address: 'Seoul',
	__proto__: { age: 20 }
};

console.log(Object.keys(person)); // ["name", "address"]

// ES8 에서 도입된 Object.values
console.log(Object.values(person)); // ["Lee", "Seoul"]

// ES8 에서 도입된 Object.entries
console.log(Object.entries(person)); // [["name", "Lee"], ["address", "Seoul"]]
```