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

- 객체는 상태를 나타내거나 이를 조작할 수 있는 동작 즉 프로퍼티와 메서드를 하나의 논리적인 단위로 묶은 복합적인 자료구조라 할 수 있다.

## 상속과 프로토타입

> 상속은 어떤 객체의 프로퍼티 또는 메서드를 다른 객체가 상속받아 그대로 사용할 수 있는 것을 말한다.
> 
- 자바스크립트는 프로포타입을 기반으로 상속을 구현하여 불필요한 중복을 제거할 수 있다.

```jsx
// 생성자 함수
function Circle(radius) {
	this.radius = radius;
	this.getArea = function() {
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

// 모든 인스턴스가 하나의 메서드를 공유한다.
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

## 프로토타입의 교체

## instanceof 연산자

## 직접 상속

## 정적 프로퍼티/메서드

## 프로퍼티 존재 확인

## 프로퍼티 열거