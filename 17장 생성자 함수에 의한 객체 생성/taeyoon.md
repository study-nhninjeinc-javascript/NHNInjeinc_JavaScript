# 17_생성자 함수에 의한 객체 생성

## *Object 생성자 함수

new 연산자와 함께 Object 생성자 함수를 호출하면 **빈 객체 생성하여 반환**

```jsx
const obj = new Object(); //빈 객체 생성

//프로퍼티 추가

obj.name = 'kim';
obj.f = function(){
	console.log('First name is' + this.name);
}

console.log(obj); // {name:"kim",f:f} 객체반환
obj.f(); //메서드 호출 
```

### 생성자함수란

= new연산자와 함께 호출하여 **객체(인스턴스) 생성하는 함수** 

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b6cda83e-7905-4ba6-a0bb-38e9b12e795c/Untitled.png)

Object생성자 함수 이외에 여러 형태의 빌트인 생성자함수 제공

⚠️생성자 함수를 사용하여 객체 생성은 특별한 이유가 없다면 지양

### 객체 리터럴에 의한 객체 생성 방식의 문제점

```jsx
const circle1 = {
  radius: 5,
  getDiameter() {
    return 2 * this.radius;
  }
};

console.log(circle1.getDiameter()); // 10

const circle2 = { 
  radius: 10,
  getDiameter() { 
    return 2 * this.radius; 
  }
}; 

console.log(circle2.getDiameter()); // 20
```

*객체 리터럴에 의한 객체 생성 방식은 **단 하나의 객체만 생성**한다.

*따라서 **동일한 프로퍼티를 갖는 객체를 여러개 생성**해야 하는 경우 

매번 같은 프로퍼티를 기술해야 하기 때문에 **비효율적**이다.

### **생성자 함수에 의한 객체 생성 방식의 장점**

```jsx
function Circle(radius) {
  // 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius; 
  };
}

// 인스턴스의 생성
const circle1 = new Circle(5); // 반지름이 5인 Circle 객체를 생성
const circle2 = new Circle(10); // 반지름이 10인 Circle 객체를 생성 

console.log(circle1.getDiameter()); // 10 
console.log(circle2.getDiameter()); // 20
```

⚠️this

**this**

this는 객체 자신의 프로퍼티나 메서드를 참조하기 위한 자기 참조 변수다. 

**this가 가리키는 값, 즉 this 바인딩은 함수 호출 방식에 따라 동적으로 결정된다.**

| 함수 호출 방식 | this가 가리키는 값(this 바인딩) |
| --- | --- |
| 일반 함수로서 호출 | 전역 객체 |
| 메서드로서 호출 | 메서드를 호출한 객체(마침표 앞의 객체) |
| 생성자 함수로서 호출 | 생성자 함수가 생성할 인스턴스 |

```jsx
function foo() {
  console.log(this);
}

// 일반적인 함수로서 호출
// 전역 객체는 브라우저 환경에서는 window, Node.js 환경에서는 global을 가리킨다.
foo(); // window

const obj = { foo }; // ES6 프로퍼티 축약 표현

// 메서드로서 호출
obj.foo(); // obj (객체를 가르킴)

// 생성자 함수로서 호출
const inst = new foo(); // inst
```

### 인스턴**생성과 this 바인딩**