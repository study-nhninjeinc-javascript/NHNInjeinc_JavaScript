# 17장 생성자 함수에 의한 객체 생성

- 다양한 객체 생성 방식 중에서 생성자 함수를 사용하여 객체를 생성하는 방식을 알아보자.
- 객체 리터럴을 사용하여 객체를 생성하는 방식과 생성자 함수를 사용하여 객체를 생성하는 방식과의 장단점을 알아보자.

### Object 생성자 함수

- new 연산자와 함께 Object 생성자 함수를 호출하면 빈 객체를 생성하여 반환한다. 빈 객체를 생성한 이후 프로퍼티 또는 메서드를 추가하여 객체를 완성할 수 있다.
- 생성자 함수란 new 연산자와 함께 호출하여 객체(인스턴스)를 생성하는 함수를 말한다. 생성자 함수에 의해 생성된 객체를 인스턴스라 한다.
- 자바스크립트는 Object 생성자 함수 이외에도 String, Number, Boolean, Function, Array, Date, RegExp, Promise등의 빌트인 생성자 함수를 제공한다.

```jsx
// 빈 객체의 생성
const person = new Object();

// 프로퍼티 추가
person.name = "Shin";
person.sayHello = function() {
	console.log("Hi! My name is " + this.name);
};

console.log(person); // {name: "Shin", sayHello: f}
person.sayHello(); // Hi! My name is Shin
```

### 객체 리터럴에 의한 객체 생성 방식의 문제점

⚠️ 객체 리터럴에 의한 객체 생성 방식은 직관적이고 간편한다. 하지만 이 생성 방식은 단 하나의 객체만 생성한다. 따라서 동일한 프로퍼티를 갖는 객체를 여러개 생성해야 하는 경우 매번 같은 프로퍼티를 기술해야 한다.

### 생성자 함수에 의한 객체 생성 방식의 장점

- 생성자 함수에 의한 객체 생성 방식은 마치 객체(인스턴스)를 생성하기 위한 템플릿(클래스)처럼 생성자 함수를 사용하여 프로퍼티 구조가 동일한 객체 여러 개를 간편하게 생성할 수 있다.
- 클래스 기반 객체지향 언어의 생성자와는 다르게 그 형식이 정해져 있는 것이 아니라 일반 함수와 동일한 방법으로 생성자 함수를 정의하고 new 연산자와 함께 호출하면 해당 함수는 생성자 함수로 동작한다.
- 만약 new 연산자와 함께 생성자 함수를 호출하지 않으면 생성자 함수가 아니라 일반 함수로 동작한다.

```jsx
// 생성자 함수
function Circle(argRadius) {
	// 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킴
  // 즉, 프로퍼티가 됨
	this.radius = argRadius;
	this.getDiameter = function () {
		return 2 * this.radius;
	};
}

// 인스턴스 생성
const circle1 = new Circle(5); // 반지름이 5인 Circle 객체를 생성
const circle2 = new Circle(10); // 반지름이 10인 circle 객체를 생성

console.log(circle1.getDiameter()); // 10
console.log(circle2.getDiameter()); // 20

// 객체 리터럴 생성 방식
const circle3 = {
	radius: 5,
	getDiameter() {
		return 2 * this.radius;
	}
};

const circle4 = {
	radius: 10,
	getDiameter() {
		return 2 * this.radius;
	}
};

console.log(circle3.getDiameter()); // 10
console.log(circle4.getDiameter()); // 20	

// new 연산자 없이 호출한 경우
const circle5 = Circle(15);

// 일반 함수로 호출된 Circle은 반환문이 없으므로 암묵적으로 undefined를 반환
console.log(circle3); // undefined

// 일반 함수로서 호출된 Circle 내의 this는 전역 객체를 가리킴
console.log(radius); // 15
```

### this

- this는 객체 자신의 프로퍼티나 메서드를 참조하기 위한 자기 참조 변수다.
- this가 가리키는 값, 즉 this 바인딩은 함수 호출 방식에 따라 동적으로 결정된다.

| 함수 호출 방식 | this가 가리키는 값(this 바인딩) |
| --- | --- |
| 일반 함수로서 호출 | 전역 객체 |
| 메서드로 호출 | 메서드를 호출한 객체(마침표 앞의 객체) |
| 생성자 함수로서 호출 | 생성자 함수가 (미래에) 생성할 인스턴스 |

```jsx
function foo() {
	console.log(this);
}

// 일반적인 함수로서 호출
foo(); // window

const obj = { foo }; // ES6 프로퍼티 축약 표현

// 메서드로서 호출
obj.foo(); // obj

// 생성자 함수로서 호출
const inst = new foo(); // inst
	
```

### 생성자 함수의 인스턴스 생성 과정

- 생성자 함수 내부의 코드를 살펴보면 this에 프로퍼티를 추가하고 필요에 따라 전달된 인수를 프로퍼티의 초기값으로서 할당하여 인스턴스를 초기화 한다. 하지만 인스턴스를 생성하고 반환하는 코드는 보이지 않는다.
- JS엔진은 암묵적인 처리를 통해 인스턴스를 생성하고 반환한다. new 연산자와 함께 생성자 함수를 호출하면 JS엔진은 다음과 같은 과정을 거쳐 인스턴스를 생성하고 인스턴스를 초기화한 후 암묵적으로 인스턴스를 반환한다.

1. **인스턴스 생성과 this 바인딩**
    
    암묵적으로 빈 객체가 생성된다. 이 빈 객체가 바로 생성자 함수가 생성한 인스턴스다. 그리고 암묵적으로 생성된 빈 객체, 즉 인스턴스는 this에 바인딩된다. 생성자 함수 내부의 this가 생성자 함수가 생성할 인스턴스를 가리키는 이유가 바로 이것이다. 이 처리는 함수 몸체의 코드가 한 줄씩 실행되는 런타임 이전에 실행된다.
    
    ```jsx
    function Circle(argRadius) {
    	// 1. 암묵적으로 인스턴스가 생성되고 this에 바인딩된다.
    	console.log(this); // Circle {}
    }
    ```
    

1. **인스턴스 초기화**
    
    생성자 함수에 기술되어 있는 코드가 한 줄씩 실행되어 this에 바인딩되어  있는 인스턴스를 초기화한다. 즉, this에 바인딩되어 있는 인스턴스에 프로퍼티나 메서드를 추가하고 생성자 함수가 인수로 전달받은 초기값을 인스턴스 프로퍼티에 할당하여 초기화하거나 고정값을 할당한다. 이 처리는 개발자가 기술한다.
    
    ```jsx
    function Circle(argRadius) {
    	// 1. 암묵적으로 인스턴스가 생성되고 this에 바인딩된다.
    	
    	// 2. this에 바인딩 되어 있는 인스턴스를 초기화한다.
    	this.radius = argRadius;
    	this.getDiameter = function () {
    		return 2 * this.radius;
    	};
    }
    ```
    

1. **인스턴스 반환**
    
    생성자 함수 내부에서 모든 처리가 끝나면 완성된 인스턴스가 바인딩된 this를 암묵적으로 반환한다.
    
    ```jsx
    function Circle(argRadius) {
    	// 1. 암묵적으로 인스턴스가 생성되고 this에 바인딩된다.
    	
    	// 2. this에 바인딩 되어 있는 인스턴스를 초기화한다.
    	this.radius = argRadius;
    	this.getDiameter = function () {
    		return 2 * this.radius;
    	};
    
    	// 3. 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환됨
    }
    
    // 인스턴스 생성. Circle 생성자 함수는 암묵적으로 this를 반환
    const circle = new Circle(1);
    console.log(circle); // Circle {radius: 1, getDiameter: f}
    
    // 만약 this가 아닌 다른 객체를 명시적으로 반환한 경우
    function Circle2(argRadius) {
    	this.radius = argRadius;
    	this.getDiameter = function () {
    		return 2 * this.radius;
    	};
    
    	// 명시적으로 객체를 반환하면 암묵적인 this 반환이 무시됨
    	return {};
    	// 만약 원시 값을 반환하면 원시 값 반환은 무시되고 암묵적으로 this가 반환
    	// return 100; // 무시됨
    }
    
    const circle2 = new Circle2(1);
    console.log(circle2); // {}
    ```