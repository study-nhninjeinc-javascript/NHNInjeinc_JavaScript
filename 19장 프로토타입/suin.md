## 19.1 객체지향 프로그래밍

- 객체지향 프로그래밍 : 전통적인 명령형 프로그래밍의 절차지향적 관점에서 벗어나 여러개의 독립적인 단위 → 객체의 집합으로 프로그램을 표션하려는 프로그래밍 패러다임
- 객체지향 프로그래밍은 실체를 인식하는 철학적 사고를 프로그래밍에 접목하려는 시도에 시작
실체는 특징이나 성질을 나타내는 **속성**을 가지고 있다.
- 다양한 속성중에서 프로그램에서 필요한 속성만 간추려 내어 표현하는 것을 **추상화**라고 한다
- 객체 : **상태데이터와 동작을 하나의 논리적인 단위로 묶은 복합적인 자료구조**
- 상태데이터 : 프로퍼티  /  동작 : 메서드

```jsx
const circle = {
	radius : 5, //상태데이터 : 프로퍼티
	getDiameter(){ //동작 : 메서드
		return 2 * this.radius;
	}
}

console.log(circle);//{radius: 5, getDiameter: ƒ}
console.log(circle.getDiameter());//10
```

## 19.2 상속과 프로토타입

- 상속 : 어떤 객체의 프로퍼티 또는 메서드를 다른 객체가 상속받아 그대로 사용할 수 있는 것
- 자바스크립트는 프로포타입을 기반으로 상속을 구현하여 불필요한 중복을 제거
중복을 제거하는 방법은 기존의 코드를 적극적으로 재사용 하는 것
- **자바스크립트는 프로토타입을 기반으로 상속을 구현한다.**

```jsx
function Cricle(radius){
	this.radius = radius;
	this.getArea = function (){
		//Math.PI 원주율을 나타내는 상수
		//**거듭제곱
		return Math.PI * this.radius ** 2;
	}
}

const circle1 = new Cricle(1);
const circle2 = new Cricle(2);

console.log(circle1.getArea === circle2.getArea);//false
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/944f878f-bcf4-413a-923d-10929022c6a4/Untitled.png)

```jsx
function Cricle(radius){
	this.radius = radius;
}
Cricle.prototype.getArea = function (){
	return Math.PI * this.radius ** 2;
}

const circle1 = new Cricle(1);
const circle2 = new Cricle(2);

console.log(circle1.getArea === circle2.getArea);//true
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/19156d49-77da-420c-a2ce-03cf6a3a3715/Untitled.png)

## 19.3 프로토타입 객체

- 프로토타입 객체
어떤 객체의 상위 객체 역할을 하는 객체
하위 객체는 상위객체의 프로퍼티를 자신의 프로퍼티 처럼 자유롭게 사용할 수 있다.

### 19.3.1 __proto__ 접근자 프로퍼티

- __proto__ 는 접근자 프로퍼티다.
    - **모든객체는 __proto__ 접근자 프로퍼티를 통해 자신의 프로토타입, [[Prototype]] 내부슬롯에 간접적으로 접근할 수 있다.**
- __proto__  접근자 프로퍼티는 상속을 통해서 사용된다
    - __proto__  접근자 프로퍼티는 객체가 직접소유하는 프로퍼티가 아니라 Object.prototype의 프로퍼티다.
- __proto__  접근자 프로퍼티를 통해 프로토 타입에 접근하는이유
    - 상호 참조에 의해 프로토 타입 체인이 생성되는것을 방지하기위해?
    
    ```jsx
    const parent = {};
    const child = {};
    child.__proto__ = parent;
    parent.__proto__ = child;//Uncaught TypeError: Cyclic __proto__ value
    ```
    
    - 아무런 체크없이 무조건적으로 프로토 타입을 교체할수없도록 __proto__ 접근자프로퍼티를 통해 접근하고 교체
- __proto__  접근자 프로퍼티를 코드내에서 직접사용하는 것은 권장하지 않는다.
    - 모든객체가 __proto__ 프로퍼티를 사용할 수 있는 것은 아니다. — 나중에 살펴 본다고함..
    - 대신 프로토타입을 참조를 취득하고 싶다면 Object.getPrototypeOf메서드를 사용
    교체하고싶다면 Object.setPrototypeOf 사용

### 19.3.2 함수객체의 prototype 프로퍼티

- 함수 객체만이 소유하는 prototype 프로퍼티는 생성자 함수가 생성할 인스턴스의 프로토타입을 가리킨다.
- prototype 프로퍼티는 생성자함수가 생성할 객체의 프로토 타입을 가리킨다.
→ non-constructor인 화살표함수와 ES6메서드 축약 표현으로 정의한 메서드는 prototype프로퍼티를 소유하지 않으며 프로토타입도 생성하지않는다.
- 모든객체가 가지고 있는 __proto__접근자 프로퍼티 함수만이 가지고 있는 prototype프로퍼티는 결국 동일한 프로퍼티를 갖는다.
하지만 이들 프로퍼티를 사용하는 주체가 다르다.
    
    
    | 구분 | 소유 | 사용주체 | 사용목적 |
    | --- | --- | --- | --- |
    | __proto__
    접근자프로퍼티 | 모든객체 | 모든객체 | 객체가 자신의 프로토타입에 접근 또는 교체하기 위해 사용 |
    | prototype
    프로퍼티 | constructor | 생성자 함수 | 생성자 함수가 자신이 생성할 객체의 프로토타입을 할당하기 위해 사용 |

```jsx
function Person(name){
	this.name = name;
}
const me = new Person('Lee');
console.log(Person.prototype === me.__proto__);//true
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5534d631-9995-4af5-af88-0d3488fa4afd/Untitled.png)

### 19.3.3 프로토타입의 constructor 프로퍼티와 생성자함수

- 모든 프로토타입은 constructor프로퍼티를 갖는다.
- constructor프로퍼티는 prototype 프로퍼티로 자신을 참조하고있는생성자함수를 가리킨다.
- 생성자함수가 생성될때 → 함수객체가 생성될때 이뤄진다.

```jsx

console.log(me.constructor === Person);//true
```

## 19.4 리터럴 표기법에 의해 생성된 객체의 생성자 함수와 프로토 타입

- 리터럴 표기법에 의한 객체 생성 방식과 같이 명시적으로 new 연산자와 함께 생성자 함수를 호출하여 인스턴스를 생성하지 않는 객체 생성 방식도 있다.
객체리터럴, 함수리터럴, 배열리터럴, 정규표현식리터럴 등..
- 리터럴 표기법에 의해 생성된 객체도 프로토타입이 존재, constructor프로퍼티가 가리키는 생성자 함수가 반드시 객체를 생성한 생성자함수라고 단정 지을 수 는 없다.

```jsx
const obj = {};
console.log(obj.constructor == Object) //true
//객체리터럴로 생성했지만 obj의 constructor는 Object다 왤까?
```

- 객체리터럴이 평가될때는 추상연산 OrdernaryObjectCreate를 호출하여 빈객체를 생성하고 프로퍼티를 추가하도록 정의되어있다.
- 함수도 함수선언문과 함수표현식을 평가하여 함수객체를 생성한것은 Function 생성자 함수가 아니지만 foo함수의 생성자 함수는 Function 생성자 함수다.
- 프로토타입과 생성자 함수는 단독으로 존재할 수 없고 언제나 쌍이다.

## 19.5 프로토타입의 생성시점

- 프로토타입은생성자 함수가 생성되는 시점에 더불어 생성된다. → 언제나 쌍이기 때문에

### 19.5.1 사용자정의 생성자 함수와 프로토타입 생성 시점

- constructor는 함수정의가 평가되어 함수객체를 생성하는 시점에 프로토 타입도 더불어 생성

```jsx
//함수정의가 평가되어 함수 객체를 생성하는 시점에 프로토 타입도 더불어 생성
console.log(Person.prototype); //{constructor: ƒ}

//생성자 함수
function Person(name){
	this.name = name;
}
```

- non-constructor는 프로토 타입이 생기지 X → 화살표함수, ES6 축약메서드

### 19.5.2 빌트인 생성자 함수 프로토타입 생성 시점

- Object, String, Number, Function 등과 같은 빌트인 생성자 함수도 일반 함수와 마찬가지로 빌트인 생성자 함수가 생성되는 시점에 프로토타입이 생성된다.
- 이후 생성자 함수 또는 리터럴 표기법으로 객체를 생성하면 프로토 타입은 생성된 객체의 [[Prototype]]내부슬롯에 할당된다.

## 19.6 객체 생성 방식과 프로토타입의 결정

### 19.6.1 객체리터럴에의해 생성된 객체의 프로토타입

- 객체를 생성할때 OrdinaryObjectCreate를 호출한다.
OrdinaryObjectCreate에 전달되는 프로토타입은 Object.prototype
생성된객체는 Object.prototype을 프로토타입으로 갖게되면서 Object.prototype를 상속
생성된 객체는 constructor나 hasOwnProperty메서드를 갖고 있진 않지만 상속받아서 사용 가능

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/aafaf73f-ca90-4c93-b150-d4fe846be83f/Untitled.png)

### 19.6.2 Object 생성자 함수에 의해 생성된 객체의 프로토타입

- 객체를 생성할때 OrdinaryObjectCreate를 호출한다.
OrdinaryObjectCreate에 전달되는 프로토타입은 Object.prototype
생성된객체는 Object.prototype을 프로토타입으로 갖게되면서 Object.prototype를 상속
객체리터럴에의해 생성된 객체와 동일한구조..

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5e0e01c4-7de9-4f40-9840-37c5a3243810/Untitled.png)

### 19.6.3 생성자 함수에 의해 생성된 객체의 프로토타입

- 생성자 함수를 호출하여 인스턴스를 생성할때 OrdinaryObjectCreate를 호출한다.
OrdinaryObjectCreate에 전달되는 프로토 타입은 생성자 함수의 prototype 프로퍼티에 바인딩되어있는 객체 → 생성자 함수에 의해 생성되는 객체의 프로토타입 = 생성자 함수의 prototype프로퍼티에 바인딩되어있는객체

```jsx
function Person(name){
	this.name = name;
}
const me = new Person('Suin');
console.log(me.__proto__ === Person.prototype);//true
console.log(me.constructor === Person);//true
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/438369a1-5d8a-4123-ba59-274741c65153/Untitled.png)

```jsx
function Person(name){
	this.name = name;
}
Person.prototype.sayHello = function(){
	console.log('Hi I`m ${this.name}');
}

const me = new Person('Suin');
me.sayHello();

const you = new Person('Sujin');
you.sayHello();
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/909de7ee-a799-43c8-a6e8-608ac5124d2e/Untitled.png)

## 19.7 프로토타입 체인

- 자바스크립트는 객체의 프로퍼티에 접근하려고 할 때 해당객체에 접근하려느 프로퍼티가 없으면 [[Prototype]] 내부슬롯 참조를 따라 자신의 푸모역할을 하는 프로토 타입의 프로퍼티를 순차적으로 검색 → 프로토타입체인
- 프로토타입 체인은 자바스크립트가 객체지향 프로그래밍의 상속을 구현하는 메커니즘이다.
- 프로토타입 체인의 최상위에 있는 객체는 Object.prototype이다. → 모든 객체는 object.prototype을 상속받는다 → 아 그래서 함수에서 그렇게 설명한거구나..!
- Object.prototype = 프로토타입 체인의 종점. Object.prototype 의 [[Prototype]]은 null이다.
- 스코프 체인과 프로토타입체인은 서로 연관없이 별도로 동작하는것이 아니라 서로 협력하여 식별자와 프로퍼티를 검색하는데 사용

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/15ea1b43-4324-419d-b751-062fe9b36b60/Untitled.png)

## 19.8 오버라이딩과 프로퍼티 섀도잉

```jsx
const Person = (function(){
	//생성자함수
	function Person(name){
		this.name = name;
	}

	//프로토타입 메서드
	Person.prototype.sayHello = function(){
		console.log('Hi My name is ${this.name}');
	};
}());

const me = new Person('Suin');

//인스턴스 메서드
me.sayHello = function = function(){
		console.log('Hey My name is ${this.name}');
}

me.sayHello(); //Hey My name is Suin

delete me.sayHello;
me.sayHello(); //Hi My name is Suin

//프로토타입 체인을 통해 프로토타입 메서드가 삭제되지 않는다.
delete me.sayHello;
me.sayHello(); //Hi My name is Suin
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d62453f2-893e-4772-9713-7f4a71bfbd5d/Untitled.png)

- 프로퍼티 셰도잉 : 상속관계에 의해 프로퍼티가 가려지는 현상
- 오버라이딩 : 상위클래스가 가지고 있는 매서드를 하위 클래스가 재정의하여 사용하는 방식
- 하위 객체를 통해 프로토타입의 프로퍼티를 변경또는 삭제하는 것은 불가능
→ 하위 객체를 통해 get은 가능 set은 허용X
- 삭제하려면 하위객체의 프로토타입체인으로 접근하는게 아니라 프로토타입에 접근해야한다.

## 19.9 프로토타입의 교체

프로토타입은 임의의 다른객채로 변경할수있다.
→ 부모객체인 프로토타입을 동적으로 변경할 수 있다는 것을 의미

이러한 특징을 활용하여 객체간의 상속관계를 동적으로 변경할수있다.

### 19.9.1 생성자 함수에 의한 프로토타입의 교체

```jsx
const Person = (function(){
	//생성자함수
	function Person(name){
		this.name = name;
	}

	//생성자 함수의 prototype 프로퍼티를 통해 프로토타입을 교체
	Person.prototype = {
		sayHello(){
			console.log('Hi My name is ${this.name}');
		}
	};

	return Person;
}());

const me = new Person('Suin');

console.log(me.constructor === Person); // false
console.log(me.constructor === Object); // true
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4b97c462-064f-4495-8d09-bbd32a2b7d07/Untitled.png)

프로토타입으로 교체한 객체리터럴에는 constructor 프로퍼티가 없다.

이렇게 프로토타입을 교체하면 constructor 프로퍼티와 생성자 함수 간의 경결이 파괴

연결을 되살릴려면 constructor 프로퍼티를 추가하여 되살릴수있다.

```jsx
const Person = (function(){
	//생성자함수
	function Person(name){
		this.name = name;
	}

	//생성자 함수의 prototype 프로퍼티를 통해 프로토타입을 교체
	Person.prototype = {
		constructor : Person,
		sayHello(){
			console.log('Hi My name is ${this.name}');
		}
	};

	return Person;
}());

const me = new Person('Suin');

console.log(me.constructor === Person); //true
console.log(me.constructor === Object); //false
```

### 19.9.2 인스턴스에 의한 프로토타입의 교체

- 생성자 함수의 prototype 프로퍼티 뿐만아니라 __proto__ 접근자 프로퍼티를 통해 접근가능
- __proto__  접근자 프로퍼티를 통해 프로토타입을 교체하는 것은 이미 생성된 객체의 프로토타입을 교체하는것이다.

```jsx
function Person(name){
	this.name = name;
}
const me = new Person('Suin');

const parent = {
	sayHello(){
		console.log('Hi My name is ${this.name}');
	}
}

//me객체의 프로토타입을 prent객체로 교체한다.
Object.setPrototypeOf(me, parent);
//me.__proto__ = parent;

me.sayHello();//'Hi My name is Suin

console.log(me.constructor === Person); // false
console.log(me.constructor === Object); // true
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f07d9b8d-6b41-4fe0-9f55-907bfd6db94c/Untitled.png)

```jsx
function Person(name){
	this.name = name;
}
const me = new Person('Suin');

const parent = {
	constructor : Person,
	sayHello(){
		console.log('Hi My name is ${this.name}');
	}
}

//me객체의 프로토타입을 prent객체로 교체한다.
Object.setPrototypeOf(me, parent);
//me.__proto__ = parent;

me.sayHello();//'Hi My name is Suin

console.log(me.constructor === Person); // true
console.log(me.constructor === Object); // false
```

프로토타입 교체를 통해 객체간의 상속관계를 동적으로 변경하는것은 꽤나 번거롭다

→ 프로토타입은 직접 교체하지 않는것이좋다.

집접상속이 더 편리하고 안전하다.

## 19.10 instanceof 연산자

- instanceof - 이항 연산자 : 객체 instanceof 생성자함수
만약 우변의 피연산자가 함수가아닌경우 TypeError
- 우변의 생성자 함수의 prototype에 바인딩된 객체가 좌변의 객체의 프로토타입 체인 상에 존재하면 true로 평가 그렇지 않은경우 false로 평가 - 프로토타입 체인에 있냐 없냐?

```jsx
function Person(name){
	this.name = name;
}

const me = new Person('Suin');

console.log(me instanceof Person); // true
console.log(me instanceof Object); // true

//프로토타입 교체할 객체
const parent = {};
//프로토타입교체
Object.setPrototypeOf(me, parent);

console.log(Person.prototype === parent); // false
console.log(parent.constructor === Person); // false

console.log(me instanceof Person); // false
console.log(me instanceof Object); // true
```

## 19.11 직접상속

### 19.11.1 Object.create에 의한 직겁 상속

- Object.create메서드는 명시적으로 프로토타입을 지정하여 새로운 객체를 생성
- Object.create메서드도 추상연산 OrdinaryObjectCreate를 호출
- Object.create(프로토타입으로 지정할객체[, 프로퍼티를 갖는 객체])
- 객체를 생성하면서 직접적으로 상속을 구현
- 장점
    - new 연산자 없이도 객체를 생성할 수 있다.
    - 프로토타입을 지정하면서 객체를 생성할 수 있다.
    - 객체 리터럴에 의해 생성된 객체도 상속받을수있다.

```jsx
//프로토타입이 null인 객체 생성 프로토타입 체인은 종점에 위치한다.
let obj = Object.create(null);
console.log(Object.getPrototypeOf(obj) === null); //true
//Object.prototype을 상속받지못한다.
console.log(obj.toString()); //Uncaught TypeError: obj.toString is not a function

obj = Object.create(Object.prototype);
console.log(Object.getPrototypeOf(obj) === Object.prototype); //true

obj = Object.create(Object.prototype,{
	x:{value : 1, writeable:true, enumerable:true, configurable:true}
});
console.log(obj.x); //1
console.log(Object.getPrototypeOf(obj) === Object.prototype); //true

const myProto = {x:10}
obj = Object.create(myProto);
console.log(obj.x); //10
console.log(Object.getPrototypeOf(obj) === Object.prototype); //false
console.log(Object.getPrototypeOf(obj) === Object.prototype); //true

//생성자 함수
function Person(name){
	this.name = name;
}
obj = Object.create(Person.prototype);
obj.name = 'suin';
console.log(obj.name); //suin
console.log(Object.getPrototypeOf(obj) === Person.prototype); //true

obj = Object.create(null);
obj.a = 1;
console.log(obj.hasOwnProperty('a'));
//Uncaught TypeError: obj.hasOwnProperty is not a function

//Object.prototype 빌트인 매서드는 객체로 직접호출하지 않는다.
console.log(Object.prototype.hasOwnProperty.call(obj, 'a'));
```

### 19.11.1 객체 리터럴 내부에서 __**proto__**에의한 직접 상속

```jsx
const myProto = {x : 10};

//객체리터럴에의해 객체를 생성하면서 프로토타입을 지정하여 직접 상속 받을 수 있다
const obj = {
	y : 20,
	__proto__ : myProto
}

console.log(obj.x, obj.y); //10 20
console.log(Object.getPrototypeOf(obj) === myProto); //true
```

## 19.12 정적 프로퍼티/메서드

- 정적 프로퍼티/메서드 : 생성자 함수로 인스턴스를 생성하지 않아도 참조/호출할 수 있는 프로퍼티/메서드를 말한다.

```jsx
function Person(name){
	this.name = name;
}
Person.prototype.sayHello = function(){
	console.log('Hi I`m ${this.name}');
}

//정적 프로퍼티
Person.staticProp = 'static prop';

//정적 메서드
Person.staticMethod = function(){
	console.log('staticMethod');
}

const me = new Person('Suin');

Person.staticMethod();//staticMethod
me.staticMethod();//Uncaught TypeError: me.staticMethod is not a function
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/59e968cf-39ab-44d5-9904-fa4fd9b98c69/Untitled.png)

- 생성자 함수가 인스턴스는 자신의 프로토타입 체인에 속한 객체의 프로퍼티/메서드에 접근할 수있다.
- 하지만 정적 프로퍼티/메서드는 인스턴스의 프로토타입 체인에 속한 객체의 프로퍼티/메서드가 아니므로 인스턴스로 접근할 수 없다.
- Object.create 메서드는 Object 생성자 함수의 정적 메서드
Object.prototype.hasOwnProperty 메서드는 Object.prototype의 메서드
→ Object.create 메서드는 인스턴스 → Object 생성자 함수가 생성한 객체로 호출할수없다.
그러나 Object.prototype.hasOwnProperty메서드는 모든객체의 프로토타입 체인의 종점 Object.prototype의 메서드 이므로 모든객체가 호출할수있다.
- 만약 인스턴스/프로토타입 메서드 내에서 this를 사용하지 않으면 그 메서드는 정적 메서드로 변경할 수 있다.
    - 인스턴스가 호출한 인스턴스/프로토타입 메서드 내에서 this는 인스턴스를 가리킨다.
    - 메서드 내에서 인스턴스를 참조할 필요가 없다면 정적 메서드로 변경하여도 동작한다.
    - 프로토타입 메서드를 호출하려면 인스턴스를 생성해야 하지만 정적 메서드는 인스턴스를 생성하지 않아도 호출할 수 있다

## 19.13 프로퍼티 존재 확인

### 19.13.1 in연산자

key in object

```jsx
const person = {
	name : 'Suin',
	address : 'Seoul'
}
console.log('name' in person); // true
console.log('age' in person); // false
console.log('toString' in person); // true
```

### 19.13.2 Object.prototype.hasOwnProperty메서드

```jsx
const person = {
	name : 'Suin',
	address : 'Seoul'
}
console.log(person.hasOwnProperty('name')); // true
console.log(person.hasOwnProperty('age')); // false
console.log(person.hasOwnProperty('toString')); // false
```

## 19.14 프로퍼티 열거

### 19.14.1 for…in문

for(변수선언문 in 객체){…}

```jsx
const person = {
	name : 'Suin',
	address : 'Seoul'
}
for(const key in person){
	console.log(key + ' : ' + person[key]);
}
console.log(Object.getOwnPropertyDescriptor(Object.prototype, 'toString'));
//{writable: true, enumerable: false, configurable: true, value: ƒ}
```

- for…in문은 객체의 프로토타입 체인상에 존재하는 모든프로토타입의 프로퍼티 중에서 프로퍼티 어트리뷰트 [[Enumerable]] 값이 true인 프로퍼티를 순회하며 열거한다.
- for…in문은프로퍼티 키가 심벌인 프로퍼티는 열거하지 않는다.
- 상속받은 프로퍼티는 제외하고 객체자신의 프로퍼티만 열거하려면 Object.prototype.hasOwnProperty 메서드를 사용하여 객체 자신의 프로퍼티인지 확인해야한다.
- 프로퍼티를 열거할 때 순서를 보장하지 않는다. - 대부분의 모던브라우저는 순서를 보장한다.

### 19.14.1 Object.keys/values/entries 메서드

- Object.keys 객체자신의 열거가능한 프로퍼티 키를 배열로 반환
- Object.values 객체자신의 열거가능한 프로퍼티 값을 배열로 반환
- Object.entries객체자신의 열거 가능한 프로퍼티 카와 값의 쌍을 배열을 배열에 담아 반환

```jsx
const person = {
	name : 'Suin',
	address : 'Seoul',
	__proto__ : {age : 20}
};

console.log(Object.keys(person)); //['name', 'address']
console.log(Object.values(person));//['Suin', 'Seoul']
console.log(Object.entries(person));
```