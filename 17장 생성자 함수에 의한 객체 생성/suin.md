# 17장 생성자 함수에 의한 객체 생성

## 17.1 Object 생성자 함수

- new 연산자와 함께 Object 생성자 함수를 호출하면 빈객체를 생성하여 반환한다.
- 빈객체를 생성한 이후 프로퍼티 또는 메서드를 추가하여 객체를 완성할수있다.

```jsx
const person = new Object();

//프로퍼티 추가
person.name = 'Lee';
person.sayHello = function(){
	console.log('Hi I`m '+this.name);
}
console.log(person);
//{name: 'Lee', sayHello: ƒ}
person.sayHello();
//Hi I`m Lee
```

- 생성자함수 : new 연산자와 함께 호출하여 객체를 생성하는 함수
- 인스턴스 : 생성자함수에 의해 생성된 객체
- 자바스크립트는 Object함수 이외도 String, Number, Function, Array등의 빌트인 생성자 함수를 제공한다.

```jsx
const strObj = new String('Lee');
console.log(typeof strObj); //object
console.log(strObj);//String {'Lee'}
```

- 반드시 Object 생성자 함수를 사용해 빈객체를 생성해야하는것은  아니다

## 17.2 생성자 함수

### 17.2.1 객체 리터럴에 의한 객체 생성 방식의 문제점

- 객체리터럴에 의한 객체 생성방식은 직관적이고 간편하지만 단하나의 객체만 생성
→ 동일한 프로퍼티를 갖는 객체를 여러개 생성하는 경우 매번 같은 프로퍼티를 기술해야하기때문에 비효율적
- 객체 리터럴에 의해 객체를 생성하는 경우 프로퍼티 구조가 동일함에도 불구하고 매번 같은 프로퍼티와 메서드를 기술해야한다.

### 17.2.2 생성자 함수에 의한 객체 생성 방식의 장점

- 생성자 함수에 의한 객체 생성방식은 객체를 생성하기 위한 템플릿 처럼 생성자 함수를 사용하여 프로퍼티 구조가 동일한 객체 여러개를 간편하게 생성할수있다

```jsx
function Circle(r){
	this.r = r;
	this.getDiameter = function(){
		//생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
		return 2*this.r;
	}
}
const circle = new Circle(5);
console.log(circle.getDiameter());//10
const circle2 = new Circle(15);
console.log(circle.getDiameter());//30
const circle3 = Circle(20);
console.log(circle3);//undefined
console.log(r);//20
```

- 생성자 함수는 이름그대로 객체를 생성하는 함수이다.
- 일반 함수와 동일한 방법으로 생성자 함수를 정의하고 new연산자와 함깨 호출하면 해당함수는 생성자 함수로 동작한다.
- 만약 new연산자와 함깨 생성자 함수를 호출하지 않으면 일반함수로 동작한다.

### 17.2.3 생성자 함수의 인스턴스 생성 과정

- 생성자 함수의 역할 : 프로퍼티구조가 동일한 인스턴스를 생성하기 위해 템플릿으로서 동작하여 **인스턴스를 생성, 생성된 인스턴스를 초기화 하는것**
- 자바스크립트 엔진은 안ㅁ묵적인 처리를 통해 인스턴스를 생성하고 반환
- new 연산자와 함깨 생성자 함수를 호출하면 자바스크립트 엔진은 과정을 거쳐 암묵적으로 인스턴스를 생성하고 인스턴스를 초기화 한후 암묵적으로 인스턴스를 반환한다.
    1. 인스턴스 생성과 this 바인딩
    암묵적으로 빈객체 생성 (아직 완성되진 않았지만 생성자 함수가 생성한 인스턴스)
    인스턴스는 this에 바인딩(식별자와 값을 연결하는 과정)된다
    이처리는 런타임 이전에 실행
    2. 인스턴스 초기화
    생성자 함수에 기술되어있는 코드가 한줄씩 실행되어 this에 바인딩 되어있는 인스턴스를 초기화한다.
    this에 바인딩 되어있는 인스턴스 프로퍼티나 메서드를 추가, 생성자 함수가 인수로 전달받은 초기값을 인스턴스 프로퍼티에 할당하여 초기화하거나 고정값을 할당.
    3. 인스턴스 반환
    생성자 함수 내부에서 모든 처리가 끝나면 완성된 인스턴스가 바인딩된 this를 암묵적으로 반환
    만약 this가 아닌다른 객체를 명시적으로 반환하면 this가 반환되지 못하고 return 문에 명시한 객체가 반환
    하지만 명시적으로 원시 값을 반환하면 원시값 반환은 무시, 암묵적으로 this가 반환

### 17.2.4 내부메서드 [[Call]]과 [[Construct]]

- 함수는 객체이므로 일반 객체와 동일하게 동작할수있다.
- 함수 객체는 일반객체가 가지고 있는 내부 슬롯과 내부메서드를 모두 가지고 있음
- 일반객체 = 함수객체는 아님 - 일반객체는 호출 X 함수는 호출 O
- 함수로서 동작하기 위해 함수객체만을 위한 [[Environment]] [[FomalParameters]]등의 내부슬롯과 [[[Call]] [[Construct]]과 같은 내부메서드를 가지고있다.
- 함수가 일반 함수로서 호출되면 [[[Call]]이 호출
 new 연산자와 함께 생성자함수로서 호출되면 [[Construct]]가 호출
- 함수는 반드시 callable이여한다. → 모든 함수객체는 [[[Call]]을 갖고있으므로 호출할수있다.
- 하지만 모든 함수객체가 [[Construct]]를 갖는건 아니다.

### 17.2.5 constructor와 non-constructor의 구분

- 자바스크립튼 엔진은 함수정의를 평가하여 함수객체를 생성할때 함수정의 방식에 따라 Construct, non-Construct로 구분한다.

```jsx
function foo(){}
const bar = function(){};
const baz ={
	x :function(){}
}

new foo();
//foo {}
new bar();
//bar {}
new baz.x();
//x {}

const arrow = () => {};
new arrow();
//Uncaught TypeError: arrow is not a constructor

const obj = {
	x(){}
}
new obj.x();
//Uncaught TypeError: obj.x is not a constructor
```

- 함수선언문과 함수 표현식으로 정의된 함수만이 construct
ES6 화살표 함수와, 메서트 축약을고 정의된 함수는 non constructor

### 17.2.6 new 연산자

- new 연산자와 함께 하수를 호출하면 해당함수는 생성자 함수로 동작한다.
→ [[Construct]]가 호출된다.
- new 연산자와 호출하는 함수는 construct여야한다.

### 17.2.6 new.target

- 생성자 함수가 new 연산자 없이 호출되는 것을 방지하기위해 파스칼 컨벤션을 사용한다 하더라도 실수는 언제나 발생할수있다
→ 이러한 위험성을 피하기 위해 ES6에서는 new.target을 지원
- new.target은 this와 유사하게 construct인 모든 함수 내부에서 안묵적인 지역변수와 같이 사용되며 메타 프로퍼티라고 부른다.
- 함수내부에서 new.target을 사용하면 mew 연산자와 함께 생성자 함수로서 호출되었는지 확인할수있다.
- new 연산자와 함께 호출되면 함수내부의 new.target은 함수자신을 가리킨다.
- new 연산자 없이 호출되면 new.target은 undefined

파스칼컨벤션