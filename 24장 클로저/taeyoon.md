# 24_클로저

키워드 = > 함수가 선언된 렉시컬 환경

```jsx
const x = 1;

function outer(){
	const x =10;
	function inner(){
		console.log(x); //10
	}
	inner();
}
outer();
//중첩함수이기에 outer에 접근가능
```

### 렉시컬 스코프

⇒ 함수를 어디서 호출이 아니라 **어디서 정의를 했는지에 따른 상위스코프 결정 !**

```jsx
const x = 1;
function f(){
	const x = 10;
	b();
}

function b(){
	console.log(x);
}
f(); // 1
b(); // 1
```

⇒상위 스코프에 대한 참조는 함수 정의가 평가되는 시점에 **함수 정의된 위치에 의해 결정**

### 함수 객체의 내부 슬롯

**함수 코드 평가 순서** 

1. 함수 실행컨텍스트 생성
2. 함수 렉시컬 환경생성
    1. 함수 환경 레코드 생
    2. this바인딩
    3. 외부 렉시컬 환경에 대한 참조 결정

⇒외부 렉시컬 환경에 대한 참조에는 함수 객체의 내부 슬롯 에 저장된 렉시컬 환경의 참조가 할당

### 클로저와 렉시컬 환경

```jsx
const x =1;

//1
function outer(){
	const x = 10;
	const inner = function(){console.log(x);}
	return inner;
}

//outer 함수를 호출하면 중첩 함수 inner 반환
//그리고 outer함수의 실행 컨텍스트는 실행 컨텍스트 스택에서 팝되어 제거 된다
const innerFunc = outer();
innerFunc(); //10
```

inner가 나오면서 내부슬롯데 outer의 참조를 가지고 나와서 x = 10을 참조 가능

외부 함수보다 중첩함가 더 오래 유지되는 경우 중첩 함수는 이미 생명 주기를 종료한 외부 함수의 변수를 다시 참조할 수 있다 이러한 중첩 함수를 **클로저**라고 부른다

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a7b2445a-8200-428e-8607-0f6b8e47a857/Untitled.png)

outer 함수의 실행컨텍스트는 실행 컨텍스트 스택에서 제거되지만 outer 함수의 렉시컬 환경까지

소멸되는 것은 아니다!

inner 함수의 내부 슬롯으로 인해 참조 그리고 inner는 return이 된 후 **innerFunc에 의해 참조 되고 있기에 가비지 컬렉션 대상 x**

### 클로저의 활용

```jsx
//특정 함수(increase)에게만 상태 변경 허용
let num = 0;

const increase = function(){
	return ++num;
}

console.log(increase()); //1
console.log(increase()); //2
console.log(increase()); //3
```

⇒ num은 전역변수이기에 지역으로 바꿔서 의도치 않은 변경 바꾸는 법

```jsx
//함수리턴
const increase = (function(){
	let num = 0;
	//클로저!
	return function(){
		return ++num;
	};
}());
console.log(increase()); //1
console.log(increase()); //2
console.log(increase()); //3
```

```jsx
//객체 리턴
const counter = (function(){
	let num = 0;
	
	//클로저인 메서드를 갖는 객체를 반환!
	//객체 리터럴은 스코프를 만들지 않는다
	//따라서 아래 메서들의 상위 스코프는 즉시 실행함수의 렉시컬 환경
	return {
		increase(){
			return ++num;
		},
		decrease(){
			return num > 0 ? --num : 0 ;
		}
	}
}();)

//외부에서 참조도 x
```

```jsx
////////////////////////////////////코드 질문

//생성자 함수 표현
const Counter = (function(){
	//카운트 상태 변수
	let num = 0;
	
	function Counter(){
		//this.num = 0 // 프로퍼티는 public하므로 은닉되지않는다 
	}

	Counter.prototype.increase = function(){
		return ++num;
	};
	
	Counter.prototype.decrease = function(){
		return num > 0 ? --num : 0;
	};

return Counter;
}());

const counter = new Counter();

```

### 클로저 예제

```jsx
//함수를 인수로 전달받고 함수를 반환하는 고차함수
function makeCounter(aux){
	//카운트 상태를 유지하기 위한 자유 변수
	let counter = 0;

	//클로저 반환
	return  function(){
		//인수로 전달받은 보조 함수에 상태 변경을 위임
		counter = aux(counter);
		return counter;
	}
}

//보조함수 

function increase(n){
	return ++n;
}

function decrease(n){
	return --n;
}

//함수로 함수를 생성한다

//독립된 렉시컬환경?

//makeCouner 함수는 보조 함수를 인수로 전달받아 함수를 반환한다.
const increaser = makeCounter(increase);
console.log(increaser()); //1 
console.log(increaser()); //2

//increaser 함수와는 별개의 독립된 렉시컬 환경을 갖기 때문에 카운터 상대가 연동 x
const decreaser = madkeCounter(decrease);
console.log(decreaser()); // -1
console.log(decreaser()); // -2
```

예제 14,15 질문…

### 캡슐화와 정보 은닉

```jsx
function Person(name, age){
	this.name =name; //public
	let _age = age; //private

	//인스스 메서드
	this.sayHi = function(){
		console.log(`name: ${this.name} age: ${_age} `)
	}
}

const me = new Person('kim',20);
me.sayHi();
console.log(me.name); // kim
console.log(me._age); //undefined

////////
//프로토타입 메서드 문제
Person.prototype.sayHi =function(){
	console.log(`name: ${this.name} age: ${_age} `)
	//생성자함수의 지역변수인 _age를 참조할 수 없는 문제가 발생 !!
}
```

문제 해결

```jsx
const Person=(function(){
	let _age =0; //private

	//즉시실행함수안에 생성자함수 
	function Person(name,age){
		this.name = name;
		_age = age;
	}

	//프로토타입 메서드
	Person.prototype.sayHi = function(){
			console.log(`name: ${this.name} age: ${_age} `)
	}
	//생성자함수 반환
	return Person;
}());

const me = new Person('kim',20)
me.sayHi();
```

```jsx
class Person{
	 #age;
	constructor(name,age){
		this.name =name;
		this.#age = age;
	}
 sayHi(){
		console.log(`name: ${this.name} age: ${this.#age} `)
	}
}
```