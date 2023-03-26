# 클래스

<aside>
💡 자바스크립트는 프로토타입 기반 객체지향 언어다. 프로토타입 기반 객체지향 언어는 클래스가 필요 없는 객체지향 프로그래밍 언어다.

</aside>

```jsx
// ES5 생성자 함수
const Person = (function() {
	// 생성자 함수
	function Person(name) {
		this.name = name;
	}

	Person.prototype.sayHi = function() {
		console.log(`Hi! I am ${this.name}`);
	};
	// 프로토타입 객체에 정의하는 이유는 메모리 절약 + 수정 용이 + 상속 활용

	return Person;
}());

let me = new Person('Lee');
me.sayHi();
```

> 이러한 방법이 클래스 기반 언어에 익숙한 프로그래머들에게는 혼란을 줄 수 있고 자바스크립트를 어렵게 느끼게 하는 하나의 장벽처럼 인식되었다.
> 

→ 그래서 ES6에서 클래스가 도입되었고 이는 클래스 기반 객체지향 프로그래밍 언어와 매우 흡사한 새로운 객체 생성 메커니즘을 제시한다.

<aside>
💡 그러나 이를 통해 기존 프로토타입 기반 객체지향 모델을 폐지하는 것이 아니다. 클래스는 함수이며 기존 프로토타입 기반 패턴을 클래스 기반 패턴처럼 사용할수 있도록 하는 문법적 설탕이라고 볼 수 있다.

</aside>

- 문법적 설탕이란 문법적으로 기능은 동일하나 해당 코드를 작성하거나 읽는 사람이 작성하기 쉽게, 또 이해하기 쉽게 만들어 주는 것 이라고 한다.

### 클래스와 생성자 함수의 차이점

- 클래스를 new 연산자 없이 호출하면 에러 발생
- 클래스는 상속을 지원하는 extends 와 super 키워드를 제공한다.
- 클래스는 호이스팅이 발생하지 않는 것처럼 동작한다.
- 클래스 내의 모든 코드에는 암묵적으로 strict mode 가 지정된다. (해제할 수 없다.)
- 클래스의 constructor , prototype method , static method 는 모두 프로퍼티 어트리뷰트 [[Enumerable]] 의 값이 false 다. (열거되지 않는다.)

### ! 클래스는 프로토타입 기반 객체 생성 패턴의 단순한 문법적 설탕이라고 보기보다는 새로운 객체 생성 메커니즘으로 볼 수 있겠다.

## 클래스 정의

```jsx
// 클래스 선언문
class Person {}

// 클래스 표현식
const Person = class {};

// 클래스 표현식
const Person2 = class MyClass {};
```

- 무명의 리터럴로 생성할 수 있다. → 런타임에 생성이 가능하다.
- 변수나 자료구조에 저장할 수 있다.
- 함수의 매개변수에게 전달할 수 있다.
- 함수의 반환값으로 사용할 수 있다.

```jsx
class Person {
	// 생성자
	constructor(name) {
		this.name = name; // public 프로퍼티
	}

	// 프로토타입 메서드
	sayHi() {
		console.log(`Hi, I am ${this.name}`);
	}

	// 정적 메서드
	static sayHello() {
		console.log('Hello!');
	}
}

const inst = new Person('Lee');

console.log(inst.name); // Lee
inst.sayHi(); // Hi, I am Lee
Person.sayHello(); // Hello!
inst.sayHello(); // error -> is not a function
```

## 클래스 호이스팅

클래스 선언문으로 정의한 클래스는 함수 선언문과 같이 소스코드 평가 과정(런타임 이전)에 먼저 평가되어 함수 객체를 생성한다.

```jsx
console.log(Person); // error -> before initialization

class Person{};
```

```jsx
const Person = '';

{
	console.log(Person); // error -> before initialization
	class Person{};
}
```

## 인스턴스 생성

```jsx
class Person {}

// 인스턴스 생성
const inst = new Person();

// new 연산자 없이 호출하는 경우 에러 발생
const noNew = Person(); // error -> Class constructor Person cannot be invoked without 'new'
```

```jsx
const Person = class MyName{};

// 함수 표현식과 마찬가지로 클래스를 가리키는 식별자로 인스턴스를 생성해야 한다. (여기선 Person)
const inst = new Person();

// 클래스 이름 MyName 은 함수와 동일하게 클래스 내부에서만 유효한 식별자다.
console.log(MyName); // error -> not defined
const inst2 = new MyName(); // error -> not defined
```

## 메서드

<aside>
💡 클래스 몸체에서 정의할 수 있는 메서드는 costructor(생성자), 프로토타입 메서드, 정적 메서드이다.

</aside>

### constructor

> 인스턴스를 생성하고 초기화하기 위한 특수한 메서드 (이름을 변경할 수 없다.)
> 

```jsx
class Person {
	// 생성자
	constructor(name) {
		// 인스턴스 생성 및 초기화
		this.name = name; // 생성자 내부의 this는 클래스가 생성할 인스턴스를 가리킨다.
	}
}

console.log(typeof Person); // function
// 클래스는 함수다.
```

<aside>
💡 모든 함수가 가지고 있는 prototype 프로퍼티가 가리키는 프로토타입 객체의 constructor 프로퍼티는 클래스 자신을 가리킨다. → 클래스가 인스턴스를 생성하는 생성자 함수라는 것을 의미

</aside>

> 클래스의 constructor는 메서드로 해석되는 것이 아니라 클래스가 평가되어 생성한 함수 객체 코드의 일부가 된다.
> 
- constructor는 클래스 내에 최대 한 개만 존재할 수 있으며 prototype 객체의 constructor 와는 다른 프로퍼티다.
- constructor는 생략할 수 있다. ( 빈 constructor 가 암묵적으로 정의된다. )
- 프로퍼티를 추가하며 인스턴스를 생성하려면 생성자 내부에서 this에 인스턴스 프로퍼티를 추가한다.
    - 이때 매개변수를 선언하여 활용할 수도 있다. (인스턴스를 생성할 때 인수를 전달한다 → 초기값)
- 생성자는 별도의 반환문을 갖지 않아야 한다. → new 연산자와 함께 클래스가 호출되면 암묵적으로 this 즉, 인스턴스를 반환하기 때문
    - 이때 별도로 반환문을 통해 다른 객체를 반환하면 this가 반환되지 않고 해당 반환문에 명시한 객체가 반환된다.
    - 원시 값을 반환하면 무시된다.

### 프로토타입 메서드

```jsx
class Person {
	constructor(name) {
		this.name = name;
	}

	// 클래스 몸체에서 정의한 메서드는 기본적으로 프로토타입 메서드가 된다.
	sayHi() {
		console.log(`Hi! ${this.name}`);
	}
}

const inst = new Person('Lee');
inst.sayHi();

Object.getPrototypeOf(inst) === Person.prototype; // true
inst instanceof Person; // true

inst.constructor === Person; // true
```

![class_1.PNG](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c0d48a6f-ec6b-4ec3-9ae6-5360dd2b38a0/class_1.png)

<aside>
💡 결국 클래스는 생성자 함수와 마찬가지로 프로토타입 기반의 객체 생성 메커니즘이다.

</aside>

### 정적 메서드

> 인스턴스를 생성하지 않아도 호출할 수 있는 메서드를 정적 메서드라고 한다.
> 

```jsx
class Person {
	constructor() {
		this.name = 'Person';
	}
	
	// 정적 메서드
	static sayHi() {
		console.log(`Hi! I am static method`);
	}
}

const inst = new person();

Person.sayHi(); // Hi! I am static method
```

![class_2.PNG](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/90ccd0be-3f80-4197-8898-f8d427c8cdc7/class_2.png)

<aside>
💡 정적 메서드는 인스턴스로 호출할 수 없다. → 프로토타입 체인 상 클래스가 없다.

</aside>

### 정적 메서드와 프로토타입 메서드의 차이

- 정적 메서드와 프로토타입 메서드는 자신이 속해 있는 프로토타입 체인이 다르다.
- 정적 메서드는 클래스로 호출하고 프로토타입 메서드는 인스턴스로 호출한다.
- 정적 메서드는 인스턴스 프로퍼티를 참조할 수 없지만 프로토타입 메서드는 인스턴스 프로퍼티를 참조할 수 있다.

```jsx
class Square {
	constructor(width, height) {
		// 인스턴스 프로퍼티
		this.width = width;
		this.height = height;
	}

	// 프로토타입 메서드
	area() {
		// 인스턴스 프로퍼티를 참조할 수 있다.
		return this.width * this.height
	}

	// 정적 메서드
	static staticArea(width, height) {
		// 인스턴스 프로퍼티를 참조할 수 없고 매개변수를 사용해야한다.
		return width * height;
	}
}

console.log(Square.staticArea(10, 10)); // 100

const inst = new Square(10, 10);
console.log(inst.area(); // 100
```

<aside>
💡 표준 빌트인 객체 또한 다양한 정적 메서드를 가지고 있다.

</aside>

### 클래스에서 정의한 메서드의 특징

- function. 키워드를 생략한 메서드 축약 표현 사용
- 객체 리터럴과는 다르게 클래스에 메서드를 정의할 때는 콤마가 필요 없다.
- 암묵적으로 strict mode로 실행된다.
- for … in 문이나 Object.keys 메서드 등으로 열거할 수 없다.
- 내부 메서드 [[Construct]]를 갖지 않는 non-constructor다. 따라서 new 연산자와 함께 호출할 수 없다.

## 클래스의 인스턴스 생성 과정

> 클래스의 인스턴스 생성 과정은 생성자 함수의 인스턴스 생성 과정과 유사하다.
> 

### 1. 인스턴스 생성과 this 바인딩

- 생성자 내부 코드가 실행되기 앞서 암묵적으로 빈 객체 생성 → 인스턴스
- 인스턴스의 프로토타입으로 클래스의 prototype 프로퍼티가 가리키는 객체가 설정
- 인스턴스가 this에 바인딩된다.

### 2. 인스턴스 초기화

- 생성자 내부 코드가 실행되어 this에 바인딩되어 있는 인스턴스를 초기화한다.
- 생성자가 생략된 경우 해당 과정이 생략된다.

### 3. 인스턴스 반환

- 하나의 완성된 인스턴스를 가리키는 this가 반환된다.
- 생성자 내부 코드에서 별도의 객체를 반환하면 해당 객체가 반환된다.

## 프로퍼티

### 인스턴스 프로퍼티

```jsx
class Person {
	constructor(name) {
		this.name = name;
	}
}

const inst = new Person('Lee');

console.log(inst.name); // 인스턴스 프로퍼티는 public 하다.
```

### 접근자 프로퍼티

> 값을 읽거나 저장할 때 사용하는 setter, getter 접근자 프로퍼티를 클래스에서 사용해보자
> 

```jsx
class Person {
	constructor(firstName, lastName) {
		this.firstName = firstName;
		this.lastName = lastName;
	}

	// fullName은 접근자 함수로 구성된 접근자 프로퍼티다.
	get fullName() { // getter
		return `${this.firstName} ${this.lastName}`;
	}

	set fullName(name) { // setter
		[this.firstName, this.lastName] = name.split(' ');
	}
}

const inst = new Person('HaNeul', 'Lee');

console.log(`${inst.firstName} ${inst.lastName}`); // 데이터 프로퍼티를 통한 값 참조

// 접근자 프로퍼티를 통한 값의 저장 및 참조
inst.fullName = 'GilDong Hong';
console.log(inst.fullName);

// 접근자 프로퍼티는 호출하는 것이 아니라 참조하는 형식으로 사용한다.
```

### 클래스 필드 정의 제안

> 클래스 필드란 클래스 기반 객체지향 언어에서 클래스가 생성할 인스턴스의 프로퍼티를 가리키는 용어다.
> 

```jsx
class Person {
	// 클래스 필드 정의
	name = 'Lee';
}

const inst = new Person();
```

<aside>
💡 자바스크립트에서도 클래스 기반 객체지향 언어의 클래스 필드처럼 프로퍼티를 정의할 수 있는 새로운 표준 사양이 제안되었기 때문.. ( 표준 사양이 된건 아니지만 선제적으로 최신 브라우저에 구현되었다고 함 )

</aside>

```jsx
class Person {
	// 클래스 필드
	name = 'Lee';

	constructor() {
		console.log(name); // error -> is not defined
	}
}
```

```jsx
class Person {
	// 클래스 몸체에서 클래스 필드를 정의하는 경우 this에 클래스 필드를 바인딩하면 안된다.
	this.name = ''; // error -> unexcepted token
}
```

```jsx
class Person {
	name;
}

const inst = new Person();
console.log(inst); // name: undefined
```

```jsx
class Person {
	// 클래스 필드에 문자열 할당
	name = 'Lee';
	
	// 클래스 필드에 함수 할당
	getName = function() {
		return this.name;
	};
}
```

### private 필드 정의 제안

```jsx
class Person {
	firstName = 'Lee';

	constructor(lastName) {
		this.lastName = lastName;
	}
}

const inst = new Person('Haneul');
console.log(`${inst.firstName} ${inst.lastName}`); // Lee Haneul
```

> 다행히도 2021년 TC39 프로세스에는 private 필드를 정의할 수 있는 새로운 표준 사양이 제안되었다고 한다. (클래스 필드 정의 제안처럼 최신 브라우저에 이미 구현되어있다.)
> 

```jsx
class Person {
	// private 필드 정의는 '#' 구문을 붙인다.
	#name = '';

	constructor(name) {
		// private 필드 참조
		this.#name = name;
	}
}

const inst = new Person('Lee');

console.log(inst.#name); // error -> private field
```

```jsx
class Person {
	// private 필드 정의
	#name = '';

	constructor(name) {
		this.#name = name;
	}

	// 접근자 프로퍼티
	get name() {
		return this.#name.trim();
	}
}

const inst = new Person(' Lee ');
console.log(inst.name); // Lee
```

<aside>
💡 private 필드는 반드시 클래스 몸체에 정의해야 한다. private 필드를 직접 constructor에 정의하면 에러가 발생한다.

</aside>

### static 필드 정의 제안

> static 키워드를 사용하여 정적 메서드를 정의하는 것 처럼 정적 필드를 정의하는 제안도 최신 브라우저에 구현이 되어 있다고 한다.
> 

```jsx
class Person {
	// static public 필드 정의
	static HELLO = 'HELLO WORLDS';

	// static private 필드 정의
	static #dot= '.';

	// static 메서드
	static sayHi(name); {
		return `${this.HELLO} I am ${name}${this.#dot}`;
	}
}

console.log(Person.HELLO);
console.log(Person.sayHi('SKY'));
```

## 상속에 의한 클래스 확장

> 상속에 의한 클래스 확장은 기존 클래스를 상속받아 새로운 클래스를 확장하여 정의하는 것이다.
> 

```jsx
class Animal { // 슈퍼 클래스가 될 클래스 정의 
	constructor(age, weight) {
		this.age = age;
		this.weight = weight;
	}

	eat() { return 'eat'; }

	move() { return 'move'; }
}

// 상속을 통해 슈퍼 클래스(Animal)를 확장한 서브 클래스(Bird)
class Bird extends Animal {
	fly() { return 'fly'; }
}

const bird = new Bird(1, 5);

console.log(bird); // age: 1 , weight: 5
console.log(bird instanceof Bird); // true
console.log(bird instanceof Animal); // true

console.log(bird.eat()); // eat
console.log(bird.move()); // move
console.log(bird.fly()); // fly
```

![class_3.PNG](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6bf55a1b-e450-4f8c-865c-449ce5916853/class_3.png)

<aside>
💡 기존 자바스크립트에서는 생성자 함수를 사용해 클래스를 흉내 내려고 의사 클래스 상속 패턴을 사용하기도 했으나 클래스의 등장으로 더는 필요하지 않다.

</aside>

### extends 키워드

> 상속을 통해 클래스를 확장하려면 extends 키워드를 사용하여 상속받을 클래스를 정의한다.
> 

```jsx
class Base {} // 슈퍼(베이스/부모) 클래스

class Derived extends Base {} // 서브(파생/자식) 클래스
```

<aside>
💡 클래스도 프로토타입을 통해 상속 관계를 구현한다.

</aside>

### 동적 상속

> extends 키워드를 통해 생성자 함수를 상속받아 클래스를 확장할 수도 있다. 단, extends 키워드 앞에는 반드시 클래스가 와야한다.
> 

```jsx
function Base(a) {
	this.a = a;
}

// 생성자 함수를 상속받는 서브클래스
class Derived extends Base {}

const derived = new Derived(1);
console.log(derived); // a: 1
```

> extends 키워드 다음에는 클래스뿐만이 아니라 [[Contruct]] 내부 메서드를 갖는 함수 객체로 평가될 수 있는 모든 표현식을 사용할 수 있다.
> 

```jsx
function Base1() {}

class Base2 {}

let state = true;

// 표현식을 통해 조건에 따라 동적으로 상속 대상을 결정
class Derived extends (state ? Base1 : Base2) {}

const derived = new Derived();

console.log(derived); // Derived {}
console.log(derived instaceof Base1); // true
```

### 서브클래스의 constructor

> 서브클래스에서 constructor를 생략하면 ‘constructor(…args) { super(…args); }’ 가 암묵적으로 정의된다. → 슈퍼, 서브 둘 다 생략하면 빈 객체 반환됨.
> 

<aside>
💡 super(…args) 는 슈퍼클래스의 constructor 이다. 즉, 서브클래스의 생성자를 생략하면 슈퍼클래스의 생성자로 대체? 된다 라고 보면 될듯하다. → 매개변수만 전달해줌

</aside>

### super 키워드

### 호출

> super를 호출하면 슈퍼클래스의 constructor를 호출한다.
> 

```jsx
class Base {
	constructor(a, b) {
		this.a = a;
		this.b = b;
	}
}

// 슈퍼클래스의 생성자 내부에서 추가한 프로퍼티를 그대로 갖는 인스턴스를 서브클래스를 통해 생성한다고 하면 서브클래스의 생성자는 생략할 수 있다.
class Derived extends Base {}

const derived = new Derived(1, 2);

console.log(derived); // a: 1, b: 2

// 그러나 슈퍼, 서브 각 클래스의 생성자에서 추가한 프로퍼티를 모두 갖는 인스턴스를 생성한다고 했을 때는 생성자를 생략할 수 없다.
class Derived2 extends Base {
	constructor(a, b, c) {
		// this.c = c; // error -> must call super
		super(a, b); // 서브 클래스에서 생성자를 정의하는 경우 super 키워드를 반드시 호출해야한다. 생략하면 에러 발생한다. 또한 호출 전까지 this를 참조할 수 없다.
		this.c = c;
	}
}

const derived2 = new Derived2(1, 2, 3);

console.log(derived2); // a: 1. b: 2, c: 3
```

- super는 반드시 서브클래스의 생성자에서만 호출한다. (상속 받지 않는 클래스의 생성자나 함수에서 호출하면 에러남)
- 메서드 내에서 super를 참조하면 슈퍼클래스의 메서드를 호출할 수 있다.

### 참조

> 메서드 내에서 super를 참조하면 슈퍼클래스의 메서드를 호출할 수 있다.
> 

```jsx
class Base {
	constructor(name) {
		this.name = name;
	}
	
	// 슈퍼클래스의 프로토타입 메서드
	sayHi() {
		return `Hi! ${this.name}`;
	}
}

class Derived extends Base {
	// 서브클래스의 프로토타입 메서드
	sayHi() {
		// 서브클래스의 프로토타입 메서드에서 슈퍼클래스의 프로토타입 메서드를 호출
		return `${super.sayHi()}. how are you doing?`;
	}
}

const derived = new Derived('SKY');
console.log(derived.sayHi());
```

<aside>
💡 프로토타입 메서드는 프로토타입 객체에 메서드로 추가된다. → super를 참조하는 메서드가 바인딩되어있는 객체의 프로토타입을 찾을 수 있어야 한다. → 프로토타입 체인 상에 있어야 한다.

</aside>

- 메서드가 바인딩되어있느 객체의 프로토타입을 찾기 위해서는 [[HomeObject]] 내부 슬롯이 존재해야 한다.
    - ES6의 메서드 축약 표현으로 정의된 함수만이 [[HomeObject]] 내부 슬롯을 갖는다.

> 서브클래스의 정적 메서드 내에서 super 참조는 슈퍼클래스의 정적 메서드를 가리킨다.
> 

```jsx
class Base {
	static sayHi() {
		return 'Hi!';
	}
}

class Derived extends Base {
	static sayHi() {
		return `${super.sayHi()} how are you doing?`;
	}

	static sayHello() {
		return `${super.sayHi()}`;
	}
}

console.log(Derived.sayHi());
console.log(Derived.sayHello());
```

### 상속 클래스의 인스턴스 생성 과정

```jsx
class Rectangle {
	constructor(width, height) {
		this.width = width;
		this.height = height;
	}

	get Area() {
		return this.width * this.height;
	}

	toString() {
		return `width = ${this.width}, height = ${this.height}`;
	}
}

class ColorRectangle extends Rectangle{
	constructor(width, height, color) {
		super(width, height);
		this.color = color;
	}

	// 메서드 오버라이딩
	toString() {
		return super.toString() + `, color = ${this.color}`;
	}
}

const colorRectangle = new ColorRectangle(2, 4, 'red');

console.log(colorRectangle.getArea()); // 8
console.log(colorRectangle.toString()); // width = 2, height = 4, color = red
```

### 1. 서브클래스의 super 호출

> 클래스 평가 시 슈퍼클래스인지 서브클래스인지 구분하기 위해 내부 슬롯 [[ConstructorKind]] 를 갖는다.
> 
> - 다른 클래스를 상속받지 않는 클래스는 해당 슬롯의 값이 ‘base’ 로, 다른 클래스를 상속받는 서브클래스는 해당 슬롯의 값이 derived로 설정된다.
- base 클래스는 암묵적으로 빈 객체를 생성하고 이를 this에 바인딩한다.
- derived 클래스는 자신이 직접 인스턴스를 생성하지 않고 슈퍼클래스에게 인스턴스생성을 위임한다.
    - 서브클래스에의 생성자에서 반드시 super를 호출해야하는 이유이다. (호출 안하면 인스턴스 생성이 안된다는 것)

### 2. 슈퍼클래스의 인스턴스 생성과 this 바인딩

> 슈퍼클래스의 생성자 내부 코드 실행전에 암묵적으로 빈 객체 생성되고 this에 바인딩 된다. (이때 슈퍼클래스 생성자 내부의 this는 생성된 인스턴스를 가리킨다.
> 
- 인스턴스는 슈퍼클래스가 생성했으나 new 연산자와 함께 호출된 클래스는 서브클래스다. → [new.target](http://new.target) 은 서브클래스를 가리킨다.
    - 인스턴스는 new.target이 가리키는 서브클래스가 생성한 것으로 처리된다.

### 3. 슈퍼클래스의 인스턴스 초기화

> 슈퍼클래스의 생성자 내부 코드가 실행되며 this에 바인딩되어 있는 인스턴스를 초기화한다. (프로퍼티 추가 및 초기화)
> 

### 4. 서브클래스 constructor로의 복귀와 this바인딩

> super의 호출 종료 후 서브클래스의 생성자로 제어 흐름이 돌아온다. 이때 super가 반환한 인스턴스가 this에 바인딩된다. → 서브클래스가 별도의 인스턴스 생성 X 그대로 사용
> 

### 5. 서브클래스의 인스턴스 초기화

> 서브클래스의 생성자에 코드를 통해 인스턴스에 프로퍼티를 추가하고 초기화한다.
> 

### 6. 인스턴스 반환

> 클래스의 모든 처리가 끝나면 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다.
> 

### 표준 필트인 생성자 함수 확장

> 상속은 클래스뿐 아니라 [[Construct]] 내부 메서드를 갖는 함수 객체로 평가될 수 있는 모든 표현식을 사용할 수 있기 때문에 표준 빌트인 객체 또한 확장할 수 있다.
> 

<aside>
💡 여러 활용 방식이 있다. 일단 메서드 체이닝이 뭔지 알아보자

</aside>