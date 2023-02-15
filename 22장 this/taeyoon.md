# 23_this

객체 리터럴 방식

```jsx
const circle = {
	radius : 5,
	getDiameter(){
		return 2*circle.radius'
	}
}

console.log(circle.getDiameter()); //10
```

생성자함수 방식

```jsx
function Circle(radius){
	//이 시점은 자신이 생성할 인스턴스 식별 x
	this.radius = radius;
	//여기서 o
}

Circle.prototype.getD =function(){
	return 2 *this.radius;
}

//생성자 함수로 인스턴스를 생성하려면 먼저 생성자 함수 정의
const circle = new Circle(5)
```

⚠️this는 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기 참조 변수다

this바인딩

바인딩이란 식별자와 값을 연결하는 과정

식별자 —— 메모리 공간의 주소 바인딩

---

자바스크립트의 this는 함수가 호출되는 방식에 따라 this에 바인딩될 값, 

즉 **this 바인딩이 동적으로 결정된다**

```jsx
// this는 어디서든지 참조 가능하다.
// 전역에서 this는 전역 객체 window를 가리킨다.
console.log(this); // window

function square(number) {
  // 일반 함수 내부에서 this는 전역 객체 window를 가리킨다.
  console.log(this); // window
  return number * number;
}
square(2);

const person = {
  name: 'Lee',
  getName() {
    // 메서드 내부에서 this는 메서드를 호출한 객체를 가리킨다.
    console.log(this); // {name: "Lee", getName: ƒ}
    return this.name;
  }
};
console.log(person.getName()); // Lee

function Person(name) {
  this.name = name;
  // 생성자 함수 내부에서 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  console.log(this); // Person {name: "Lee"}
}

const me = new Person('Lee');
```

---

### 함수 호출 방식과 this 바인딩

렉시컬 스코프와 this 바인딩은 결정 시기가 다르다

```jsx
// this 바인딩은 함수 호출 방식에 따라 동적으로 결정된다.
const foo = function () {
  console.dir(this);
};

// 동일한 함수도 다양한 방식으로 호출할 수 있다.

// 일반 함수 호출
// foo 함수를 일반적인 방식으로 호출
// foo 함수 내부의 this는 전역 객체 window를 가리킨다.
foo(); // window

// 메서드 호출
// foo 함수를 프로퍼티 값으로 할당하여 호출
// foo 함수 내부의 this는 메서드를 호출한 객체 obj를 가리킨다.
const obj = { foo };
obj.foo(); // obj

// 생성자 함수 호출
// foo 함수를 new 연산자와 함께 생성자 함수로 호출
// foo 함수 내부의 this는 생성자 함수가 생성한 인스턴스를 가리킨다.
new foo(); // foo {}

// Function.prototype.apply/call/bind 메서드에 의한 간접 호출
// foo 함수 내부의 this는 **인수에 의해 결정**된다. 
const bar = { name: 'bar' };

foo.call(bar);   // bar
foo.apply(bar);  // bar
foo.bind(bar)(); // bar
```

⚠️**일반 함수로 호출된 모든 함수**(중첩,콜백) 내부의 **this에는 전역 객체가 바인딩**

### 바인딩하는 방법

`**Function.prototype.apply/call/bind`메서드 활용**

```jsx
var value = 1;

const obj = {
  value: 100,
  foo() {
    // 콜백 함수에 명시적으로 this를 바인딩한다.
    setTimeout(function () {
      console.log(this.value); // 100
    }.bind(this), 100);
  }
};

obj.foo();
```

**화살표 함수 활용**

```jsx
var value = 1;

const obj = {
  value: 100,
  foo() {
    // 화살표 함수 내부의 this는 상위 스코프의 this를 가리킨다.
    setTimeout(() => console.log(this.value), 100); // 100
  }
};

obj.foo();
```