# 18_함수와 일급 객체

### 일급객체

```jsx
//함수는 무명의 리터럴로 생성 가능
//함수 변수에 지정 가능
//런타임 시 함수 리터럴 평가 후 객체 생성 되고 변수에 할당 

const increase = function(num){
	return ++num;
}

const decrease = function(num){
	return --num
}

//함수는 객체에 저장 가능
const auxs = {increase, decrease}

//함수는 매개 변수에 전달 가능
//함수의 반환 값으로 사용 가능

function makeCounter(arg){
	let num = 0;
	return function(){
		num = arg(num);
		return num 
	}
}

//함수는 매개변수에 함수를 전달 가능
const increaser = makeCounter(auxs.increase);
console.log(increaser();)//1 
console.log(increaser();)//2 
```

### 함수의 객체의 프로퍼티

```jsx
function add(num){
	return num+num;
}

console.dir(add)
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/591a31a7-b6b2-42ba-b7ab-514a56f1e7fe/Untitled.png)

```jsx
//함수의 모든 프로퍼티의 프로퍼티 어트리뷰트 확인
console.log(Object.getOwnPropertyDescriptors(add));

//함수 객체 고유의 데이터프로퍼티

//{length: {…}, name: {…}, arguments: {…}, caller: {…}, prototype: {…}}
//arguments:{value: null, writable: false, enumerable: false, configurable: false}
//caller:{value: null, writable: false, enumerable: false, configurable: false}
//length:{value: 1, writable: false, enumerable: false, configurable: true}
//name:{value: 'add', writable: false, enumerable: false, configurable: true}
//prototype:{value: {…}, writable: true, enumerable: false, configurable: false}
//[[Prototype]]:Object

//__proto__는 add 함수의 프로퍼티가 아니다
console.log(Object.getOwnPropertyDescriptors(add,'__proto__')) //undefined 반환

//__proto__는  Object.prototype 객체의 접근자 프로퍼티

//add 함수는 Object.prototype 객체로부터 __proto__ 접근자 프로퍼티를 상속받음

console.log(Object.getOwnPropertyDescriptors(Object.prototype,'__proto__'));
//get,set,enumerable"false,configurable:true
```

Object.prototype 객체의 프로퍼티는 모든 객체가 상속받아 사용

__ proto __ 접근자 프로퍼티는 모든 객체가 사용가능

⚠️상속에 대해서는 19장 prototype에서 자세히 

### arguments 프로퍼티

keyword : 순회가능한 유사 배열 , 함수외부참조x

ES3부터 표준에서 폐지 

```jsx
function add(x,y){
	console.log(arguments);
	return x + y;
}

//전달되지 않은 매개변수는 호출시에 undefined로 초기화
console.log(add()) //NaN
console.log(add(1)) //NaN
console.log(add(1,2)) //2
console.log(add(1,2,3)) //2 초과된 인수는 arguments에 보관
```

callee//////

### arguments 객체의 (Symbol.iterator) 프로퍼티

arguments 객체의  (Symbol.iterator) 프로퍼티는 arguments 객체를 순회 가능한 자료구조 이터러블로 만들기 위한 프로퍼티

```jsx
function add(x,y){
	const iterator = arguments[Symbol.iterator]();

//이터레이터의 next 메서드를 호출하여 이터러블 객체 arguments를 순회
	console.log(iterator.next()); // {value:1, done:false}
	console.log(iterator.next()); // {value:2, done:false}
	console.log(iterator.next()); // {value:undefined, done:true} // 순회가 끝나서 true반환
}

add(1,2);'
```

⚠️자바스크립트는 인수개수를 확인 안하기에 인수 개수를 확인하고 함수를 동작 정희할때 

유용하게 사용할 수 있는 arguments객체

```jsx
function add(){
	let res = 0;

	//arguments 객체는 length 프로퍼티가 있는 유사 배열 객체이므로 for문 순회 가능 
	for(let i =0; i < arguments.length; ++i){
		res += arguments[i];
	}
	return res;
}

console.log(sum()) // 0
console.log(sum(1,2)) // 3
console.log(sum(1,2,3)) // 6
```

유사배열객체

**length 프로퍼티를 가진 객체**로 for문으로 순회할 수 있는 객체

** ES6부터 arguments 객체는 유사 배열 객체이면서 동시에 이터러블

```jsx
function add(){
	// arguments 객체를 배열로 반환
	const array = Array.prototype.slice.call(arguments); //예전방식
	//call this객체지정
	return array.reduce(function(pre,cur){
			return pre + cur;
	},0//초기값)
	//배열순회하면서 마지막까지 순회
} 
console.log(add(1,2))//3
```

```jsx
function add(...args){
	reteurn arg.reduce((pre,cur) => pre + cur, 0);
}
console.log(add(1,2))//3
```

### caller 프로퍼티

⇒자신을 호출한 함수를 가르킨다

```jsx
function f(func){
	return func();
}

function b(){
	return 'caller:' + b.caller;
}

console.log(f(b)) // caller : function f(func)~
console.log(b()); // caller : null
```

### length 프로퍼티

⇒정의할 때 선언한 매개변수의 개수

```jsx
function f(){}
console.log(f.length) //0

function b(x){
	return x;
}
console.log(b.length)//1

function c(arg1,arg2){
}
c(1,2,3,4);
console.log(c.legnth)//2
```

### name 프로퍼티

⇒함수 객체를 가르키는 식별자 값

```jsx
//기명
var nameF= function foo(){};
console.log(nameF.name)//foo

//익명
var annoyF = function(){};
console.log(annoyF.name); //annoyF

//선언문
function f(){};
console.log(f.name); f

```

### __ proto __ 접근자 프로퍼티

모든 객체는 [[Prototype]]이라는 내부 슬롯 갖음

__ proto __ 프로퍼티는  [[Prototype]] 내부 슬롯이 가리키는 

프로토타입 객체에 접근하기 위해 사용하는 접근자 프로퍼티 (간접접근)

```jsx
const obj= {a:1};

//객체 리터럴 방식으로 생성한 객체의 프로토타입 객체는 Object.prototype

console.log(obj.__proto__ === Object.prototype) //true (상속)

console.log(obj.hasOwnProperty('a')); //true (고유)
console.log(obj.hasOwnProperty('__proto__')); //false

```

### prototype 프로퍼티

생성자 함수로 호출할 수 있는 함수 객체 constructor만이 소유하는 프로퍼티

```jsx
(function(){}).hasOwnProperty('prototype') // true

({}).hasOwnProperty('prototype')//false
```

생성자 함수가 생성할 인스턴스의 프로토타입 객체를 가리킴