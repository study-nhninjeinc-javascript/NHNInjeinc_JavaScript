# subin

# 17. 생성자 함수에 의한 객체 생성

## 17.1 Object 생성자 함수

- new 연산자와 함께 Object생성자 함수를 호출하면 빈 객체를 생성하여 반환한다.
- 빈 객체를 생성한 이후 프로퍼티 또는 메서드를 추가하여 객체를 완성할 수 있다.

<aside>
💡 // 빈 객체의 생성
const person = new Object();

// 프로퍼티 추가
person.name = ‘Lee’;
person.sayHello = function() {
    console.log(’Hi! My name is ’ + this.name); 
};

console.log(person);    // {name: “Lee”, sayHello: f}
person.sayHello();    // Hi! My name is Lee

</aside>

- 생성자 함수란 new 연산자와 함께 호출하여 객체(인스턴스)를 생성하는 함수를 말한다.
    - 생성자 함수에 의해 생성된 객체를 인스턴스라 한다.
- 자바스크립트는 Object 생성자 함수 이외에도 String, Number, Boolean, Function, Array, Date, RegExp, Promist등의 빌트인 생성자 함수를 제공한다.

<aside>
💡 // String 생성자 함수에 의한 String 객체 생성 
const strObj = new String(’Lee’);
console.log(typeof strObj);    // object
console.log(strObj);    // String {”Lee”}

// Number 생성자 함수에 의한 Number 객체 생성
const numObj = new Number(123);
console.log(typeof numObj);    // Object
console.log(numObj);    // Number {123}

// Boolean 생성자 함수에 의한 Boolean 객체 생성
const boolObj = new Boolean(true);
console.log(typeof boolObj);    // object
console.log(boolObj);    // Boolean {true}

// Function 생성자 함수에 의한 Function 객체(함수) 생성
const func = new Function(’x’, ‘return x * x’);
console.log(typeof func);    // function
console.dir(func);    // f anoymous(x)

// Array 생성자 함수에 의한 Array 객체(배열) 생성
const arr = new Array(1, 2, 3);
console.log(typeof arr);    // object
console.log(arr);    // [1, 2, 3]

// RegExp 생성자 함수에 의한 RegExp 객체(정규 표현식) 생성
const regExp = new RegExp(/ab+c/i);
console.log(typeof regExp);    // object
console.log(regExp);    // /ab+c/i

// Date 생성자 함수에 의한 Date 객체 생성
const date = new Date();
console.log(typeof date);    // object
console.log(date);    // Mon May 04 2020 08:36:33 GMT+0900 (대한민국 표준시)

</aside>

- 반드시 Object 생성자 함수를 사용해 빈객체를 사용해야 하는 것은 아니다
- 객체를 생성하는 방법은 객체 리터럴을 사용하는 것이 더 간편하다.
- Object 생성자 함수를 사용해 객체를 생성하는 방식은 특별한 이유가 없다면 그다지 유용해 보이지 않는다.

## 17.2 생성자 함수

### 17.2.1 객체 리터럴에 의한 객체 생성 방식의 문제점

- 객체 리터럴에 의한 객체 생성 방식은 직관적이고 간편하다.
- 객체 리터럴에 의한 객체 생성 방식은 단 하나의 객체만 생성한다.
- 동일한 프로퍼티를 갖는 객체를 여러 개 생성해야 하는 경우 매번 같은 프로퍼티 기술해야 하기 때문에 비효율적이다.

<aside>
💡 const circle1 = {
    radius : 5,
    getDiameter() {
        return 2 * this.radius;
    }
};

console.log(circle1.getDiameter());    // 10

const circle2 = {
    radius : 10,
    getDiameter() {
        return 2 * this.radius;
    }
};

console.log(circle2.getDiameter());    // 20

</aside>

- 객체는 프로퍼티를 통해 객체 고유의 생태를 표현한다.
- 메서드를 통해 상태 데이터인 프로퍼티를 참조하고 조작하는 동작을 표현한다.
- 프로퍼티는 객체마다 프로퍼티 값이 다를 수 있지만 메서드는 내용이 동일한 경우가 일반적이다.
- 객체 리터럴에 의해 객체를 생성하는 경우 프로퍼티 구조가 동일함에도 불구하고 매번 같은 프로퍼티와 메서드를 기술해야 한다.
- 객체가 한두 개라면 넘어갈 수도 있겠지만 만약 수십 개의 객체를 생성해야 한다면 문제가 크다.

### 17.2.2 생성자 함수에 의한 객체 생성 방식의 장점

- 생성자 함수를 사용하여 프로퍼티 구조가 동일한 객체 여러개를 간편하게 생성할 수있다.

<aside>
💡 // 생성자 함수
function Circle(radius) {
    // 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
    this.radius = radius;
    this.getDiameter = function () {
        reture 2 * this.radius;
    };
}

// 인스턴스 생성
const circle1 = new Circle(5);     // 반지름이 5인 Circle 객체를 생성 
const circle2 = new Circle(10);     // 반지름이 10인 Circle 객체를 생성 

console.log(circle1.getDiameter());    // 10
console.log(circle2.getDiameter());    // 20

</aside>

### this

- this는 객체 자신의 프로퍼티나 메서드를 참조하기 위한 자기 참조 변수다.
- this가 가리키는 값 즉 this 바인딩은 함수 호출 방식에 따라 동적으로 결정된다.

| 함수 호출 방식 | this가 가리키는 값(this 바인딩) |
| --- | --- |
| 일반 함수로서 호출 | 전역 객체 |
| 메서드로서 호출 | 메서드를 호출한 객체(마침표 앞의 객체) |
| 생성자 함수로서 호출 | 생성자 함수가(미래에) 생성할 인스턴스 |

<aside>
💡 // 함수는 다양한 방식으로 호출될 수 있다.
function foo() {
    console.log(this);
}

// 일반적인 함수로서 호출
// 전역 객체는 브라우저 환경에서는 window, Node.js 환경에서는 global을 가리킨다.
foo();    // window

const obj = { foo };    // ES6 프로퍼티 축약 표현

// 메서드로서 호출
obj.foo();    // obj

// 생성자 함수로서 호출
const inst = new foo();    // inst

</aside>

- 생성자 함수는 이름 그대로 객체를 생성하는 함수다.
- 형식이 정해져 있는 것이 아니라 일반 함수와 동일한 방법으로 생성자 함수를 정의하고 new 연산자와 함께 호출하면 해당 함수는 생성자 함수로 동작한다.
- 만약 new 연산자와 함께 생성자 함수를 호출하지 않으면 생성자 함수가 아니라 일반 함수로 동작한다.

<aside>
💡 // new 연산자와 함께 호출하지 않으면 생성자 함수로 동작하지 않는다.
// 즉, 일반 함수로서 호출된다.
const circle3 = Circle(15);

// 일반 함수로서 호출된 Circle은 반환문이 없으므로 암묵적으로 undefined를 반환한다.
console.log(circle3);   // undefined

// 일반 함수로서 호출된 Circle 내의 this는 전역 객체를 가리킨다.
console.log(radius);    // 15

</aside>

### 17.2.3 생성자 함수의 인스턴스 생성 과정

- 먼저 생성자 함수의 몸체에서 수행해야 하는 것이 무엇인지 생각할 것 !!!
- 생성자 함수의 역할은 인스턴스를 생성하는 것과 생성된 인스턴스를 초기화(인스턴스 프로퍼티 추가 및 초기값 할당)하는 것이다.
- 생성자 함수가 인스턴스를 생성하는 것은 필수이고 생성된 인스턴스를 초기화하는 것은 옵션이다.

<aside>
💡 // 생성자 함수
function Circle(radius) {
    // 인스턴스 초기화
    this.radius = radius;
    this.getDiameter = function () {
        return 2 * this.radius;
    };
}

// 인스턴스 생성
const circle1 = new Circle(5);    // 반지름이 5인 Circle 객체를 생성

</aside>

- 인스턴스를 생성하고 반환하는 코드는 보이지 않는다.
- new 연산자와 함께 생성자 함수를 호출하면 자바스크립트 엔진은 암묵적으로 인스턴스를 생성하고 인스턴스를 초기화 한 후 암묵적으로 인스턴스를 반환한다.
1. 인스턴스 생성과 this 바인딩

- 암묵적으로 빈 객체가 생성된다. - 생성자 함수가 생성한 인스턴스다 (아직 완성 X)
- 암묵적으로 생성된 빈 객체, 즉 인스턴스는 this에 바인딩된다.
- 이 처리는 런타임 이전에 실행된다.
- 바인딩
    
    식별자와 값을 연결하는 과정
    
    변수 선언은 변수 이름과 확보된 메모리 공간의 주소를 바인딩하는 것이다.
    
    this 바인딩은 this와 this가 가리킬 객체를 바인딩 하는 것
    

<aside>
💡 function Circle(radius) {
    // 1. 암묵적으로 인스턴스가 생성되고 this에 바인딩된다.
    console.log(this);    // Circle()

    this.radius = radius;
    this.getDiameter = function() {
        return 2 * this. radius;
    };
}

</aside>

1. 인스턴스 초기화

- 생성자 함수에 기술되어 있는 코드가 한 줄씩 실행되어 this에 바인딩되어 있는 인스턴스를 초기화한다.
- 즉 this에 바인딩되어 있는 인스턴스에 프로퍼티나 메서드를 추가하고 생성자 함수가 인수로 전달받은 초기값을 인스턴스 프로퍼티에 할당하여 초기화하거나 고정값을 할당한다.

<aside>
💡 funciton Circle(radius) {
    // 1. 암묵적으로 인스턴스가 생성되고 this에 바인딩된다.
   
    // 2. this에 바인딩되어 있는 인스턴스를 초기화한다.
    this.radius = radius;
    this.getDiameter = function () {
        return 2 * this.radius;
    };
}

</aside>

1. 인스턴스 반환

- 생성자 함수 내부에서 모든 처리가 끝나면 완성된 인스턴스가 바인딩된 this를 암묵적으로 반환한다.

<aside>
💡 function Circle(radius) {
    // 1. 암묵적으로 빈 객체가 생성되고 this에 바인딩된다.
  
    // 2. this에 바인딩되어 있는 인스턴스를 초기화한다.
    this.radius = radius;
    this.getDiameter = function() {
        return 2 * this.radius;
    };

    // 3. 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다.
}

// 인스턴스 생성. Circle 생성자 함수는 암묵적으로 this를 반환한다.
const circle = new Circle(1);
console.log(circle);    // Circle {radius: 1, getDiameter: f}

</aside>

- 만약 this가 아닌 다른 객체를 명시적으로 반환하면 this가 반환하지 못하고 return 문에 명시한 객체가 반환된다.

<aside>
💡 function Circle(radius) {
    // 1. 암묵적으로 빈 객체가 생성되고 this에 바인딩된다.

    // 2. this에 바인딩되어 있는 인스턴스를 초기화한다.
    this.radius = radius;
    this.getDiameter = funciton() {
        return 2 * this.radius;
    };
    
    // 3. 암묵적으로 this를 반환한다.
    // 명시적으로 객체를 반환하면 암묵적인 this 반환이 무시된다.
    return {};
}

// 인스턴스 생성. Circle 생성자 함수는 명시적으로 반환한 객체를 반환한다.
const circle = new Circle(1);
console.log(circle);    // {}

</aside>

- 명시적으로 원시 값을 반환하면 원시 값 반환은 무시되고 암묵적으로 this가 반환된다.

<aside>
💡 function Circle(radius) {
    // 1. 암묵적으로 빈 객체가 생성되고 this에 바인딩된다.

    // 2. this에 바인딩되어 있는 인스턴스를 초기화한다.
    this.radius = radius;
    this.getDiameter = funciton() {
        return 2 * this.radius;
    };
    
    // 3. 암묵적으로 this를 반환한다.
    // 명시적으로 원시 값을 반환하면 원시 값 반환은 무시되고 암묵적으로 this가 반환된다.
    return 100;
}

// 인스턴스 생성. Circle 생성자 함수는 명시적으로 반환한 객체를 반환한다.
const circle = new Circle(1);
console.log(circle);    // Circle { radius:1, getDiameter: f}

</aside>

- 생성자 함수 내부에서 명시적으로 this가 아닌 다른 값을 반환하는것은 생성자 함수의 기본 동작을 회손한다.
- 생성자 함수 내부에서 return 문을 반드시 생략해야 한다.

### 17.2.4 내부 메서드 [[Call]]과 [[Construct]]

<aside>
💡 // 함수는 객체다.
function foo() {}

// 함수는 객체이므로 프로퍼티를 소유할 수 있다.
foo.prop = 10;

// 함수는 객체이므로 메서드를 소유할 수 있다.
foo.method = function () {
    console.log(this.prop);
};

foo.method();    // 10

</aside>

- 함수는 객체지만 일반 객체와 다르다. - 일반 객체느느 호출할 수 없지만 함수는 호출할 수 있다.
- 일반 객체가 가지고 있는 내부 슬롯, 내부 메서드는 물론 함수로서 동작하기 위해 함수 객체만을 위한 [[Environment]], [[FormalParameters]] 등의 내부 슬롯과 [[Call]], [[Construct]] 같은 내부 메서드를 추가로 가지고 있다.

<aside>
💡 function foo() {}

// 일반적인 함수로서 호출: [[Call]]이 호출된다.
foo();

// 생성자 함수로서 호출: [[Construct]]가 호출된다.
new foo();

</aside>

내부 메서드 [[Call]]을 갖는 함수 객체를 callable

내부 메서드 [[Construct]]를 갖는 함수 객체를 constructor

내부 메서드 [[Construct]]를 갖지 않는 함수 객체를 non-constructor

![17-2.jpeg](subin%20ed798ecfc76749a4beca6b8d8eba77fd/17-2.jpeg)

- 함수 객체는 반드시 callable이어야 한다. 하지만 모든 함수 객체가 constructor인 것은 아니다.
- 모든 함수 객체는 내부 메서드 [[Call]]을 갖고 있으므로 호출할 수 있다.
- 함수 객체느느 callable이면서 constructor 이거나 callable이면서 non-constructor
- 모든 함수 객체는 호출할 수 있지만 모든 함수 객체를 생성자 함수로서 호출할 수 있는 것은 아니다.

### 17.2.5 constructor와 non-constructor의 구분

- 자바스크립트 엔진은 함수 정의를 평가해 함수 정의 방식에 따라 constructor와 non-constructor를 구분한다.
    - constructor : 함수 선언문, 함수 표현식, 클래스
    - non-constructor: 메서드(ES6 메서드 축약 표현), 화살표 함수

<aside>
💡 // 일반 함수 정의 : 함수 선언문, 함수 표현식
function foo() {
    const bar = function () {};
    // 프로퍼티 x의 값으로 할당한 것은 일반 함수로 정의된 함수다. 이는 메서드로 인정하지않는다.
    const baz = {
        x : function () {}
    }
};

// 일반 함수로 정의된 함수만이 constructor다.
new foo();    // → foo {}
new bar();    // → bar {}
new baz.x();    // → x {}

// 화살표 함수 정의
const arrow = () ⇒ {};

new arrow();    // TypeError : arrow is not a constructor

// 메서드 정의 : ES6의 축약 표현만 메서드로 인정한다.
const obj = {
    x() {}
};

new obj.x();    // TypeError: obj.x is not a constructor

</aside>

 

- 함수를 프로퍼티 값으로 사용하면 일반적으로 메서드로 통칭한다. 하지만 ECMAScript 사양에서 메서드란 ES6의 메서드 축약 표현만을 의미한다.
- 함수가 어디에 할당되어 있는지에 따라 메서드인지를 판단하는 것이 아닌 함수 정의 방식에 따라 constructor와 non-constructor를 구분한다.
    - non=constructor인 함수 객체를 생성자 함수로서 호출하면 에러가 발생한다.
- 주의할 점은 생성자 함수로서 호출할 것을 기대하고 정의하지 않은 일반 함수에 new 연산자를 붙여 호출하면 생성자 함수처럼 동작할 수도 있다는 것이다.

### 17.2.6 new 연산자

- 일반 함수와 생성자 함수에 특별한 형식적 차이는 없다.
- new 연산자와 함께 함수를 호출하면 해당 함수는 생성자 함수로 동작한다.

<aside>
💡 // 생성자 함수로서 정의하지 않은 일반 함수
function add(x, y){
    return x + y;
}

// 생성자 함수로서 정의하지 않은 일반 함수를 new 연산자와 함께 호출
let inst = new add();

// 함수가 객체를 반환하지 않았으므로 반환문이 뭄시된다. 따라서 빈 객체가 생성되어 반환된다.
console.log(inst);    // {} 

// 객체를 반환하는 일반 함수
function createUser(name, role) {
    return { name, role };
}

// 일반 함수를 new 연산자와 함께 호출
inst = new createUser(’Lee’, ‘admin’);
// 함수가 생성한 객체를 반환한다.
console.log(inst);    // {name: “Lee”, role: “admin”}

</aside>

 

- new 연산자 없이 생성자 함수를 호출하면 일반 함수로 호출된다.

<aside>
💡 // 생성자 함수 
function Circle(radius) {
    this.radius = radius;
    this.getDiameter = function () {
        return 2 * this.radius;
    };
}

// new 연산자 없이 생성자 함수 호출하면 일반 함수로서 호출된다.
const circle = Circle(5);
console.log(circle);    // undefined

// 일반 함수 내부의 this는 전역 객체 window를 가리킨다.
console.log(radius);    // 5
console.log(getDiameter());    // 10

circle.getDiameter();
// TypeError : Cannot read property ‘getDiameter’ of undefined

</aside>

- 위 예제의 Circle 함수는 일반 함수로서 호출되었기 때문에 Circle 함수 내부의 this는 전역 객체 window를 가리킨다
- 일반 함수와 생성자 함수를 구별하기 위해 생성자 함수는 일반적으로 첫 문자를 대문자로 기술하는 파스칼 케이스로 명명하여 사용하는 것을 권장한다.

### 17.2.7 new.target

- 생성자 함수가 new 연산자 없이 호출되는 것을 방지하기 위해 ES6에서는 new.target을 지원한다. 단 IE에서는 지원하지 않는다.
- new 연산자와 함께 생성자 함수로서 호출되면 함수 내부의 [new.target](http://new.target)은 함수 자신을 가리킨다. new 연산자 없이 일반 함수로서 호출된 함수 내부의 new.target은 undefined다.

<aside>
💡 // 생성자 함수
function Circle(radius) {
    // 이 함수가 new 연산자와 함께 호출되지 않았다면 [new.target](http://new.target)은 undefined다.
    if(!new.target) {
        // new 연산자와 함께 생성자 함수를 재귀 호출하여 생성된 인스턴스를 반환한다.
        return new Circle(radius);
    }
    this.radius = radius;
    this.getDiameter = function () {
        return 2 * this.radius;
    }; 
}

// new 연산자 없이 생성자 함수를 호출하여도 new.target을 통해 생성자 함수로서 호출된다.
const circle = Circle(5);
console.log(circle.getDiameter());

</aside>

### 스코프 세이프 생성자 패턴

<aside>
💡 // Scope-Safe Constructor Pattern
function Circle(radius) {
    // 생성자 함수가 new 연산자와 함께 호출되면 함수의 선두에서 빈 객체를 생성하고
    // this에 바인딩한다. 이때 Circle은 프로토타입에 의해 연결되지 않는다.
    
    // 이 함수가 new 연산자와 함께 호출되지 않았다면 이 시점의 this는 전역 객체 window를          가리킨다. 
    // 즉 this와 circle은 프로토타입에 의해 연결되지 않는다.

    if(!(this instanceof Circle)) {
        // new 연산자와 함께 호출하여 생성된 인스턴스를 반환한다.
        return new Circle(radius);
    }

    this. radius = radius;
    this.getDiameter = function () {
        return 2 * this.radius;
    };
}

// new 연산자 없이 생성자 함수를 호출하여도 생성자 함수로서 호출된다.
const circle = Circle(5);
console.log(circle.getDiameter());    // 10

</aside>

- new 연산자와 함께 생성자 함수에 의해 생성된 객체(인스턴스)는 프로토타입에 의해 생성자 함수와 연결된다. 이를 이용해 new 연산자와 함께 호출되었는지 확인할 수 있다.
- 대부분 빌트인 생성자 함수(Object, String, Number, Boolean, Function, Array, Date, RegExp, Promise 등)은 new 연산자와 함께 호출되었는지를 확인한 후 적절한 값을 반환한다.

<aside>
💡 // Object와 Function 생성자 함수는 new 연산자 없이 호출해도 new 연산자와 함께 호출했을 때와 동일하게 동작한다.
let obj = new Object();
console.log(obj);	// {}

obj = Object();
console.log(obj);	// {}

let f = new Function('x', 'return x ** x');
console.log(f);	// f anonymous(x) { return x ** x }

f = Function('x', 'return x ** x');
console.log(f);	// f anonymous(x) { return x ** x }

// String, Number, Boolean 생성자 함수는 new 연산자 없이 호출하면 문자열, 숫자, 불리언 값을 반환한다. 이를 통해 데이터 타입 변환을 하기도 한다.

const str = String(123);
console.log(str, typeof str);	// 123 string

const num = Number('123');
console.log(num, typeof num);	// 123 number

const bool = Boolean('true');
console.log(bool, typeof bool);	// true boolean

</aside>