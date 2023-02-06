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