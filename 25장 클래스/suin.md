# 25장 클래스

## 25.1 클래스는 프로토타입의 문법적 설탕인가?

- 프로토타입 객체지향 언어는 클래스가 필요없는 프로그래밍언어이다
→ ES5에서는 클래스 없이도 생성자 함수와 프로토 타입을 통해 객체지향 언어의 상속을 구현할수있다.
- ES6에서 도입된 클래스는 기존 클래스기반 객체지향 프로그래밍에 익숙한 프로그래머가 더욱 빠르게 학습할 수 있도록 클래스 기반 객체지향 프로그래밍 언어와 매우흡사한 새로운 객체 생성 매커니즘을 제시한다.
    - 그렇다고 SE6의 클래스가 기존의 프로토타입 기반 객체지향 모델을 폐지하고 새롭게 클래스 기반 객체지향 모델을 제공하는것은 아니다.
    - 사실 클래스는 함수이며 기존 프로토타입 기반 패턴을 클래스 기반 패턴처럼 사용할 수 있도록하는 문법적 설탕이라고 볼수있다.
- 단, 클래스와 생성자 함수는 모두 프로토타입기반의 인스턴스를 생성하지만 정확히 동일하게 동작하지는 않는다.
- 클래스는 생성자함수보다 엄격하며 생성자 함수에서는 제공하지 않는 기능도 제공한다.
- 클래스와 생성자함수의 차이
    1. 클래스를 new 연산자 없이 호출하면 에러
    생성자 함수를 new 연산자 없이 호출하면 일반함수로서 호출 - 에러는 나지않는다.
    2. 클래스는 상속을 지원하는 extends와 super 키워드를 제공 O
    생성자 함수는  extends와 super 키워드를 제공 X
    3. 클래스는 호이스팅이 발생하지 않는 것처럼 동작
    함수 선언문으로 선언된 생성자함수는 함수 호이스팅 발생
    함수 표현식으로 선언된 생성자 함수는 변수 호이스팅 발생
    4. 클래스 내의 모든 코드에는 암묵적으로 strict mode가 지정되어 strict mode를 해제할수 X
    생성자 함수는 암묵적으로 strict mode가 지정되지 X
    5. 클래스의 constructor, 프로토타입 메서드, 정적 메서드는 모두 프로퍼티 어트리뷰트 [[Enumerable]]의 값이 false다. → 열거되지 않는다.
- 생성자함수와 클래스는 프로토타입 기반의 객체지향을 구현했다는 점이 유사 하지만
클래스는 생성자 함수 기반의 객체생성방식보다 견고하고 명료하다
    - 특히 클래스 extends와 super 키워드는 상속관계 구현을 더욱 간결하고 명료하게한다.
- 따라서 클래스를 프로토타입 기반 객체 생성 패턴의 단순한 문법적 설탕이라고 보기보다는 “새로운 객체생성 메커니즘”으로 보는것이 합당하다.

문법적 설탕 : 사람이 이해 하고 표현하기 쉽게 디자인된 프로그래밍 언어 문법

## 25.2 클래스 정의

- 클래스는 class 키워드를 사용하여 정의한다
- 클래스이름은 파스칼케이스를 사용하는것이 일반적이다.
- 일반적이지는 않지만 함수와 마찬가지로 표현식으로 클래스를 정의할 수도 있다.
→ 이때 클래스는 함수와 마찬가지로 이름을 가질 수도 있고, 갖지 않을수도 있다.
- 클래스를 표현식으로 정의할수 있다는것은 클래스가 값으로 사용할 수 있는 일급 객체라는 것을 의미한다.
- 클래스 몸체에는 0개 이상의 매서드만 정의할 수 있다.
정의할 수 있는 메서드는 constructor(생성자), 프로토타입 메서드, 정적 메서드의 세가지가있다.

```jsx
//클래스 선언문
class Person{
	//생성자	
	constructor(name){
		//인스턴스 생성 및 초기화
		this.name = name;
	}
	//프로토타입 메서드
	sayHi(){
		console.log(`Hi I am ${this.name}`);
	}
	//정적메서드
	static sayHello(){
		console.log('Hello');
	}
}

//인스턴스 생성
const me = new Person('Lee');

//인스턴스 프로퍼티 참조
console.log(me.name);//Lee

//프로토타입 메서드 호출
me.sayHi();

//정적 메서드 호출
Person.sayHello();

//생성자 함수의 정의방식으로..
//클래스와 생성자 함수의 정의 방식은 형태적인 면에서 매우 유사하다
var Person = (function(){
	//생성자 함수
	function Person(name){
		this.name = name;
	}
	//프로토타입 메서드
	Person.prototype.sayHi = function(){
		console.log(`Hi I am ${this.name}`);
	}
	//정적메서드
	Person.sayHello = function(){
		console.log('Hello');
	}
	return Person;
}());
```

## 25.3 클래스 호이스팅

- 클래스는 함수로 평가된다

```jsx
class Person{}
console.log(typeof Person);//function
```

- 클래스 선언문으로 정의한 클래스는 함수 선언문과 같이 소스코드 평가과정, 즉 런타임 이전에 먼저 평가되어 함수객체를 생성한다.
- 이때 클래스가 평가되어 생성된 함수 객체는 생성자 함수로서 호출할 수 있는 함수, 즉 constructor다
- 생성자 함수로서 호출할수 있는 함수는 함수 정의가 평가되어 함수객체를 생성하는 시점에 프로토타입도 더불어 생성된다
→ 프로토타입과 생성자함수는 단독으로 존재할수 없고 언제나 쌍으로 존재하기 때문이다
- 단, 클래스는 클래스 정의 이전에 참조할 수 없다.

```jsx
console.log(Person);
//Uncaught ReferenceError: Cannot access 'Person' before initialization

//클래스 선언문 
class Person{}
```

- 클래스 선언문은 마치 호이스팅이 발생하지 않는 것처럼 보이나 그렇지않다.
- 클래스 선언문도 변수 선언, 함수 정의와 마찬가지로 호이스팅이 발생한다.
→ let, const 키워드로 선언한 변수처럼 클래스 선언문 이전에 일시적 사각지대(TDZ)에 빠지기때문에 호이스팅이 발행하지 않는 것 처럼 동작한다.

```jsx
const Person = '';
{
	//호이스팅이 발생하지 않는다면 ''이 출력되어야한다.
	console.log(Person);
	//Uncaught ReferenceError: Cannot access 'Person' before initialization
	class Person{}
}
```

## 25.4 인스턴스 생성

- 클래스는 생성자 함수이며, new 연산자와 함께 호출되어 인스턴스를 생성한다.
- 함수는 new 연산자의 사용 여부에 따라 일반함수,생성자 호출되지만
클래스는 인스턴스를 생성하는 것이 유일한 존재 이유이므로 반드시 new 연산자와 함께 호출해야한다.

```jsx
class Person{}
//클래스를 new 연산자 없이 호출하면 타입에러발생
const me = Person();
//Uncaught TypeError: Class constructor Person cannot be invoked without 'new'
```

- 클래스 표현식으로 정의된 클래스의 경우
클래스를 가리키는 식별자를 사용해 인스턴스를 생성하지 않고
기명 표현식의 클래스이름을 사용해 인스턴스를 생성하면 에러가 발생한다.
- 기명함수 표현식과 마찬가지로 클래스 표현식에서 사용한 클래스이름은 외부코드에서 접근 불가는 하기 때문이다.

```jsx
const Person = class Myclass{};

//함수표현식과 마찬가지로 클래스를 가리키는 식별자로 인스턴스를 생성 해야한다.
const me = new Person();

//클래스 이름 Myclass는 함수와 동일하게 클래스 몸체 내부에서만 유효한 식별자다
console.log(Myclass);
//Uncaught ReferenceError: Myclass is not defined

const you = new Myclass();
//Uncaught ReferenceError: Myclass is not defined
```

## 25.5 매서드

### constructor

- constructor는 인스턴스를 생성하고 초기화하기 위한 특수한 메서드다.
constructor는 이름을 변경할 수 없다.
- 클래스는 평가되어 함수객체가된다.
클래스도 함수 객체 고유의 프로퍼티를 모두 갖고있다.
함수와 동일하게 프로토 타입과 연결되어 있으며 자신의 스코프 체인을 구성한다.
- 모든 함수 객체가 가지고 있는 prototype 프로퍼티가 가리키는 프로토타입 객체의 constructor 프로퍼티는 클래스 자신을 가리키고있다
→클래스가 인스턴스를 생성하는 생성자 함수라는 것을 의미, 즉, new 연산자와 함께 클래스를 호출하면 클래스를 인스턴스를 생성한다.

```jsx
class Person{
	constructor(name){
		this.name = name;	
	}
}
console.log(typeof Person);
console.dir(Person);

const me = new Person('Suin');
console.log(me);
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f8ccb11f-3d90-46c4-8972-ae00f3994f3c/Untitled.png)

- 클래스 constructor내부에서 this에 추가한 name 프로퍼티가 클래스가 생성한 인스턴스 프로퍼티로 추가된것을 확인할수있다. → 생성자 함수와 마찬가지로 constructor 내부에서 this에 추가한 프로퍼티는 인스턴스 프로퍼티가 된다. constructor 내부 this는 생성자 함수와 마찬가지로 클래스가 생성한 인스턴스를 가리킨다.
- construct는 매서드로 해석되는 것이 아니라 클래스가 평가되어 생겅한 함수 객체코드의 일부가 된다. 클래스 정의가 평가되면 constructor의 기술된 동작을 하는 함수 객체가 생성된다.
- constructor는 생성자 함수와 유사하지만 몇가지 차이가 있다.
    - construct는 클래스내에 최대 한 개만 존재할 수 있다. 만약 클래스가 2개이상의 constructor를 포함하면 문법에러가 발생한다.
    - constructor는 생략할 수 있다. 생략하면 클래스에 빈 constructor가 암묵적으로 정의된다. constructor를 생량한 클래스는 빈 constructor에 의해 빈객체를 생성한다.
        - 프로퍼티가 추가되어 초기화된 인스턴스를 생성하려면 consttructor내부에서 this에 인스턴스 프로퍼티를 추가한다.
        - 인스턴스를 생성할때 클래스 외부에서 인스턴스 프로퍼티 초기값을 전달하려면 다음과 같이 constructor에 매개변수를 선언하고 인스턴스를 생성할때 초기 값을 전달한다. 이때 초기 값은 constructor의 매개변수에 전달된다.
            
            ```jsx
            class Person{
            	constructor(name, adress){
            		this.name = name;	
            		this.adress = adress;	
            	}
            }
            //인수로 초기값을 전달한다. 초기값은 constructor에 전달된다.
            const me = new Person('Lee','Seoul');
            console.log(me);//Person {name: 'Lee', adress: 'Seoul'}
            ```
            
        - 인스턴스 생성과 동시에 인스턴스 프로퍼티 추가를 통해 인스턴스 초기화를 실행한다. 따라서, 인스턴스를 초기화하려면 constructor를 생략해서는 안된다
    - constructor는 별도의 반환문을 갖지 않아야한다.
        - new 연산자와 함께 클래스가 호출되면 생성자 함수와 동일하게 암묵적으로 this, 즉 인스턴스를 반환한다.
        - 만약 this가 아닌 다른 객체를 명시적으로 반환하면 this, 즉 인스턴스가 반환되지 못하고 return 문에 명시한 객체가 반환된다.
        - construcor 내부에서 명시적으로 this가 아닌 다른값을 반환하는 것은 클래스의 기본동작을 훼손 한다. → constructor내부에서 return 문은 반드시 생략해야한다.

### 프로토타입 메서드

- 클래스 몸체에서 정의한 매서드는 생성자 함수에 의한 객체 생성 방식과는 다르게 클래스의 prototype 프로퍼티에 메서드를 추가하지 않아도 기본적으로 프로토타입 메서드가 된다.
- 생성자 함수와 마찬가지로 클래스가 생성한 인스턴스는 프로토타입 체인의 일원이 된다.

```jsx
class Person{
	constructor(name){
		this.name = name;	
	}
	sayHi(){
		console.log(`Hi I am ${this.name}`);
	}
}
const me = new Person('Lee');
me.sayHi();//Hi I am Lee

// me 객체의 프로토타입은 Person.prototype이다.
Object.getPrototypeOf(me) === Person.prototype; //true
me instanceof Person; //true

// Person.prototype의 프로토타입은 Object.prototype이다.
Object.getPrototypeOf(Person.prototype) === Object.prototype; //true
me instanceof Object; //true

// me 객체의 constructor는 Person 클래스다.
me.constructor === Person; //true
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2288e9ea-0238-4997-a2d0-c5b0b8d74898/Untitled.png)

- 클래스 몸체에 정의한 매서드는 인스턴스의 프로토타입에 존재하는 프로토타입 매서드가 된다
인스턴스는 프로토타입 메서드를 상속받아 사용할수있다
- 프로토 타입 체인은 기존의 모든 객체 생성  방식 뿐만 아니라 클래스에 의해 생성된 인스턴스에도 동일하게 적용된다. 생성자 함수의 역할을 클래스가 할뿐이다
- 결국 클래스는 생성자 함수와 같이 인스턴스를 생성하는 생성자 함수라고 볼 수 있다.
→ 클래스는 생성자 함수와 마찬ㄴ가지로 프로토타입 기반의 객체생성 메커니즘이다.

### 정적 메서드

- 정적 메서드는 인스턴스를 생성하지 않아도 호출할 수 있는 메서드를 말한다.
- 생성자 함수의 경우 정적 메서드를 생성하기 위해서는 다음과 같이 명시적으로 생성자 함수에 메서드를 추가 해야한다.
- 클래스에서는 메서드에 static 키워드를 붙이면 정적 메서드가 된다.

```jsx
class Person{
	constructor(name){
		this.name = name;	
	}
	static sayHi(){
		console.log(`Hi!`);
	}
}
//정적매서드는 클래스로 호출한다.
//정적매서드는 인스턴스 없어도 호출할수 있다.
Person.sayHi();//Hi!

//인스턴스 생성
const me = new Person('Lee');
me.sayHi();//Uncaught TypeError: me.sayHi is not a function
```

- 정적메서드는 클래스에 바인딩된 메서드가 된다.
- 클래스는 함수객체로 평가되므로 자신의 프로퍼티를 소유할수 있다.
- 클래스는 클래스 정의가 평가되는 시점에 함수 객체가 되므로 인스턴스와 달리 별다른 생성과정이 필요 없다. → 정적메서드는 클래스 정의 이후 인스턴스를 생성하지 않아도 호출할 수 있다.
- 정적매서드는 프로토 타입 메서드 처럼 인스턴스로 호출하지 않고 클래스로 호출한다.
- 정적 매서드는 인스턴스로 호출할수 없다. 정적 메서드가 바인딩된 클래스는 인스턴스의 프로토타입 체인 상에 존재하지 않기 때문이다.
→ 인스턴스의 프로토타입 체인상에는 클래스가 존재하지 않기 때문에 인스턴스로 클래스 메서드를 상속 받을수있다

### 정적메서드와 프로토타입메서드의 차이

- 정적 메서드와 프로토 타입 메서드의 차이는 다음과 같다.
    1. 정적 매서드와 프로토타입 메서드는 자신이 속해 있는 프로토타입 체인이 다르다
    2. 정적메서드는 클래스로 호출하고 프로토타입 메서드는 인스턴스로 호출한다.
    3. 정적 메서드는 인스턴스 프로퍼티를 참조할 수 없지만 프로토아비 메서드는 인스턴스 프로퍼티를 참조할 수 있다.

```jsx
class Square{
	//정적메서드
	static area(width, height){
		return width * height;
	}
}
console.log(Square.area(10, 10));//100
```

```jsx
class Square{
	constructor(width, height) {
    this.width = width;
    this.height = height;
  }
  // 프로토타입 메서드
  area() {
    return this.width * this.height;
  }
}
const square = new Square(10, 10)
console.log(square.area()) // 100
```

- 프로토타입 메서드는 인스턴스로 호출해야하므로 프로토타입 메서드 내부의 this 프로토타입 메서드를 호출한 인스턴스를 가리킨다.
- 정적메서드는 클래스로 호출해야하므로 정적 메서드 내부의 this는 인스턴스가 아닌 클래스를 가리킨다. → 프로토타입 메서드와 정적메서드 내부의 this바인딩이 다르다
- 따라서 메서드 내부에서 인스턴스 프로퍼티를 참조할 필요가 있다면 this를 사용해야하며, 이러한 경우 프로토타입 메서드로 정의해야한다. 하지만 메서드 매부에서 인스턴스 프로퍼티를 참조할 필요가 없다면 this를 사용하지 않게된다.
- 메서드 내부에서 this를 사용하지 않더라도 프로토타입 메서드로 정의할수 있다.
하지만 반드시 인스턴스를 생성한 다음에 인스턴스로 호출해야 함므로 this를 사용하지 않는 메서드는 정적 메서드로 정의하는 것이 좋다.

### 클래스에서 정의한 메서드의 특징

1. function 키워드를 생략한 메서드 축약 표현을 사용한다.
2. 객체 리터럴과는 다르게 클래스에 메서드를 정의할 때는 콤마가 필요 없다.
3. 암묵적으로 strict mode로 실행된다.
4. for ... in 문이나 Object.keys 메서드 등으로 열거할 수 없다. 즉, 프로퍼티의 열거 가능 여부를 나타내며, 불리언 값을 갖는 프로퍼티 어트리뷰트 [[Enumerable]]의 값이 false다.
5. 내부 메서드 [[Construct]]를 갖지 않는 non-constructor다. 따라서 new 연산자와 함께 호출할 수 없다.

## 25.6 클래스의 인스턴스 생성과정

- new 연산자와 참께 클래스를 호출하면 생성자 함수와 마찬가지로 클래스의 내부메서드 [[Construct]]가 호출된다.
- 클래스는 new 연산자없이 호출된다.

### 1. 인스턴스 생성과 this 바인딩

- new 연산자와 함께 클래스를 호출하면 constructor 내부 코드가 실행되기에 앞서 암묵적으로 빈 객체가 생성 → 이 빈객체가 바로 클래스가 생성한 인스턴스
- 이때 클래스가 생성한 인스턴스의 프로토타입으로 클래스의 prototype 프로퍼티가 가리키는 객체가 설정
- 그리고 암묵적으로 생성된 빈 객체, 즉 인스턴스는 this에 바인딩된다.
- 따라서 constructor가 인수로 전달받은 초기값으로 인스턴스 프로퍼티 값을 초기화한다.

### 2. 인스턴스 초기화

- constructor의 내부 코드가 실행되어 this에 바인딩되어 있는 인스턴스를 초기화한다.
- 즉, this에 바인딩되어 있는 인스턴스에 프로퍼티를 추가하고 constructor가 인수로 전달받은 초기값으로 인스턴스의 프로퍼티 값을 초기화한다
- 만약 constructor가 생략되었다면 이과정은 생략된다.

### 3. 인스턴스 반환

- 클래스의 모든 처리가 끝나면 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다.

```jsx
class Person{
	//생성자
	constructor(name){
		//암묵적으로 인스턴스가 생성되고 this에 바인딩된다.
		console.log(this);//Person {}
		console.log(Object.getPrototypeOf(this) === Person.prototype);//true
		//this에 바인딩되어있는 인스턴스를 초기화한다.
		this.name = name;	
		//완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다.
	}
}
```

## 25.7 프로퍼티

### 인스턴스 프로퍼티

### 접근자 프로퍼티

### 클래스필드 정의 제안

### private필드 정의 제안

### static필드 정의 제안

## 25.8 상속에 의한 클래스 확장

### 클래스 상속과 생성자 함수 상속

### extends 키워드

### 동적 상속

### 서브클래스의 constructor

### super 키워드

### 상속 클래스의 인스턴스 생성과정

### 표준 빌트인 생성자 함수 확장