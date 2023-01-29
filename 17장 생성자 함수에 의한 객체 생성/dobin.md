## 17장 생성자 함수에 의한 객체 생성

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
    

1. 내부 메서드 [[Call]]과 [[Construct]]
    - 함수는 객체이므로 일반 객체와 동일하게 동작할 수 있다. 함수 객체는 일반객체가 가지고 있는 내부 슬롯과 내부 메서드를 모두 가지고 있기 때문이다.
    - 함수는 객체이지만 일반 객체와 다르게 일반 객체는 호출할 수 없지만 함수는 호출할 수 있다.
    - 함수는 일반객체가 가지고 있는 내부 슬롯과 내부 메서드는 물론, 함수로서 동작하기 위해 함수 객체만을 위한 [[Environment]], [[Formal Parameters]] 등의 내부 슬롯과 [[Call]], [[Construct]] 같은 내부 메서드를 추가로 가지고 있다.
    - 일반 함수로서 호출되면 [[Call]]이 호출되고 이 내부 메서드를 갖는 함수 객체를 callable이라 하며, new 연산자와 함께 생성자함수로서 호출되면 내부 메서드[[Construct]]가 호출되고 이 내부메서드를 갖는 함수 객체를 constructor, 이 내부 메서드를 갖지 않는 함수를 non-constructor라고 부른다.
    
    1. constructor와 non-constructor의 구분
        - constructor : 함수 선언문, 함수 표현식, 클래스(클래스도 함수다)
        - non-constructor : 메서드(ES6 메서드 축약 표현), 화살표 함수
        
        ```jsx
        // 프로퍼티 x의 값으로 할당된 것은 일반 함수로 정의된 함수다.
        // 이는 메서드로 인정하지 않는다.
        const baz = {
        	x: function() {}
        };
        
        // 메서드 정의 : ES6의 메서드 축약 표현만 메서드로 인정
        const obj = {
        	x() {}
        };
        
        new baz.x(); // -> x {}
        new obj.x(); // TypeError : obj.x is not a constructor
        ```
        
    
    1. new 연산자
        - 일반 함수와 생성자 함수에 특별한 형식적 차이는 없다. new 연산자와 함께 함수를 호출하면 해당 함수는 생성자 함수로 동작한다.
        
        ```jsx
        // 일반 함수
        function add(x, y) {
        	return x + y;
        }
        
        // 생성자 함수로서 정의하지 않은 일반 함수를 new 연산자와 함께 호출
        let inst = new add();
        
        // 함수가 객체를 반환하지 않았으므로 반환문이 무시됨.
        // 따라서 빈 객체가 생성되어 반환
        console.log(inst); // {}
        
        // 객체를 반환하는 일반 함수
        function createUser(name, role) {
        	return {name, role};
        }
        
        // 일반 함수를 new 연산자와 함께 호출
        inst = new createUser('Lee', 'admin');
        
        // 함수가 생성한 객체를 반환된다.
        console.log(inst); // {name: "Lee", role: "admin"}
        ```
        
        - 반대로 new 연산자 없이 생성자 함수를 호출하면 일반 함수로 호출된다.
        
        ```jsx
        // 생성자 함수
        function Circle(radius) {
        	this.radius = radius;
        	this.getDeameter = function () {
        		return 2 * this.radius;
        	}
        }
        
        // new 없이 생성자 함수 호출하면 일반 함수로 됨
        const circle = Circle(5);
        console.log(circle); // undefined
        
        // 일반 함수라서 내부의 this는 전역객체를 가리킴
        console.log(radius); // 5
        
        circle.getDiameter();
        // TypeError : Cannot read property 'getDiameter' of undefined
        ```
        
        - 생성자 함수는 일반적으로 첫 문자를 대문자로 기술하는 파스칼 케이스로 명명하여 일반함수와 구별할 수 있도록 노력한다.
        
    2. new.target
        - 생성자 함수가 new 연산자 없이 호출되는 것을 방지하기 위해 파스칼 케이스 컨벤션을 사용한다 하더라도 실수는 언제나 발생할 수 있다. 이를 위해 ES6에서는 new.target을 지원한다.
        - this와 유사하게 constructor인 모든 함수 내부에서 암묵적인 지역 변수와 같이 사용되며 메타 프로퍼티라고 부른다.
        - new 연산자와 함게 생성자 함수로서 호출되면 함수 내부의 new.target은 함수 자신을 가리킨다. new 연산자 없이 일반 함수로서 호출된 함수 내부의 new.target은 undefined다.
        - 따라서 함수 내부에서 new.target을 사용하여 new 연산자와 생성자 함수로서 호출했는지 확인하여 그렇지 않은 경우 new 연산자와 함께 재귀 호출을 통해 생성자 함수로서 호출할 수 있다.
        
        ```jsx
        // 생성자 함수
        function Circle(radius) {
        	// 이 함수가 new 연산자와 함께 호출되지 않았다면 new.tartget은 undefined다.
        	if(!new.target) {
        		// new 연산자와 함께 생성자 함수를 재귀 호출하여 생성된 인스턴스를 반환한다.
        		return new Circle(radius);
        	}
        	this.radius = radius;
        	this.getDiameter = funciton () {
        		return 2 * this.radius;
        	};
        }
        
        // new 연산자 없이 생성자 함수를 호출하여도 new.target을 통해 생성자 함수로서 호출된다.
        const circle = Circle(5);
        console.log(circle.getDiameter());
        ```
        
        - 스코프 세이프 생성자 패턴 : new.target은 ES6에서 도입된 최신 문법으로 new.target을 사용할 수 없는 상황이라면 스코프 세이프 패턴을 사용할 수 있다.
            
            ```jsx
            // 스코프 세이프 생성자 패턴
            function Circle(radius) {
            	// 생성자 함수가 new 연산자와 함께 호출되면 함수의 선두에서 빈 객체를 생성하고
            	// this에 바인딩 한다. 이때 this와 Circle은 프로토타입에 의해 연결
            
            	// 이 함수가 new 연산자와 함게 호출되지 않았다면 이 시점의 this는 전역 객체 window를 가리킴
            	// 즉, this와 Circle은 프로토타입에 의해 연결되지 않는다.
            	if(!(this instanceof Circle)) {
            		// new 연산자와 함께 호출하여 생성된 인스턴스를 반환
            		return new Circle(radius);
            	}
            	this.radius = radius;
            	this.getDiameter = function () {
            		return 2 * this.radius;
            	};
            }
            
            // new 연산자 없이 생성자 함수를 호출하여도 생성자 함수로서 호출
            const circle = Circle(5);
            console.log(circle.getDiameter()); // 10
            ```
            
        - new 연산자와 함께 생성자 함수에 의해 생성된 객체(인스턴스)는 프로토타입에 의해 생상자 함수와 연결됨
        - 이를 이용해 new 연산자와 함게 호출되었는지 확인할 수 있다.
        - 대부분의 빌트인 생성자 함수 (Object, String, Number, Boolean, Function 등)는 new 연산자와 함께 호출되었는지를 확인한 후 적절한 값을 반환한다.
        
        ```jsx
        // Object와 Function 생성자 함수는 new 연산자 없이 호출해도
        // new 연산자와 함께 호출했을 때와 동일하게 동작
        let obj = new Object();
        console.log(obj); // {}
        
        obj = Object();
        console.log(obj); // {}
        
        let f = new Function('x', 'return x ** x');
        console.log(f); // f anonymous(x) { return x ** x}
        
        f = Function('x', 'return x ** x');
        console.log(f); // f anonymous(x) { return x ** }
        ```
        
        - 하지만 String, Number, Boolean 생성자 함수는 new 연산자와 함께 호출했을 때 String, Number, Boolean 객체를 생성하여 반환하지만 new 연산자 없이 호출하면 문자열, 숫자, 불리언 값을 반환한다. 이를 통해 데이터 타입을 변환 하기도 한다.
        
        ```jsx
        const str = String(123);
        console.log(str, typeof str); // 123 string
        
        const num = Number('123');
        console.log(num, typeof num); // 123 number
        
        const bool = Boolean('true');
        console.log(bool, typeof bool); // true boolean
        ```