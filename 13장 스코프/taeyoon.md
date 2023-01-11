# 13_scope

## scope정의

변수에 **접근할 수 있는 범위**

⚠️var와 let,const 의 스코프는 자바스크립트 엔진에서 다르게 동작

//실행컨텍스트에서 자세하게 다룸 undefined, 사각지대..

```jsx
function f(a,b){
	console.log(a,b);
	return a + b;
}

f(1,2); // 1,2 / 3

console.log(a,b); // 참조에러 
//=> 매개변수는 함수 몸체에서만 참조 가능이기에
```

모든 식별자(변수 이름, 클래스 이름, 함수이름)는 **자신이 선언된 위치에 의해** 타 코드가 참조할 수 있는 유효 범위가 결정

```jsx
var x = 'g'

function f(){
	var x = 'lo';
	console.log(x) // f함수 스코프 참조 lo
}

f();

console.log(x) // 전역 스코프 참조 g
```

어떤 x를 참조할 것이냐 ?

⇒식별자 결정

## var 키워드로 선언한 변수의 중복선언

```jsx
function f(){
	var x= 1;

	var x =2;
	console.log(x); //2 
	//자바스크립트 엔진은 var x =1;은 키워드가 없는 것으로 동작
}
f();

//////////////////////////////////////////////

function f2(){
	let x=1;

	let x=2; //문법 에러  let은 중복선언 x 
}
f2();
```

## 스코프의 종류

```jsx
//전역
var x = 'gx';
var y = 'gy';

function outer(local1){
	var z = 'outer local z';
	
	console.log(x) // gx
	console.log(y) //gy
	console.log(z) // outer local z

	function inner(local2){
		var x = 'inner local x';
		console.log(x) // inner local x
		console.log(y) // gy
		console.log(z) // outer local z
	}
	inner();
}
outer();
console.log(x) //gx  함수 중첩 내부 참조 x 위로위로
console.log(z) //참조 에러

```

⚠️ **전역 변수**는 어디서든 지 참조

⚠️ **지역변수**는 자신의 지역 스코프 와 하위 지역 스코프

## 스코프체인

스코프가 함수의 **중첩에 의해 계층적 구조** 

⚠️자바스크립트 엔진은 스코프 체인을 통해 변수를 참조하는 코드의 스코프에서 시작하여 **상위로 검색 //하위 스코프에서 참조는 XXXX**

```jsx
function f(){
	console.log('전역 f함수')
}

function f2(){
	function f(){
		console.log('지역 f함수')
	}
	f();
}

f2(); // 지역 f함수   
//=> f2를 호출하면 f2의 record에 f함수가 담기고 그것을 호출
```

## 함수 레벨 스코프

블록 레벨 스코프 

if, for, while ,try/catch 등 코드 블록이 지역 스코프 생성 ⇒ **let,const키워드 변수**는 **모든 코드 블록을 지역 스코프로 인정**

함수 레벨 스코프

함수가 지역 스코프 생성 ⇒ var 키워드로 선언된 변수는 오직 함수만 지역스코프로 인정

```jsx
var x = 1;

if(true){
	//var = 함소 몸체만 지역스코프로 인정
	//코드 블록 내에서 선언 되어도 모두 전역 변수
	//값 변경 부작용 
	var x= 10; //중복 선언 o
}
console.log(x) // 10

////////////////

let x = 1;

if(true){
	//var = 함소 몸체만 지역스코프로 인정
	//코드 블록 내에서 선언 되어도 모두 전역 변수
	//값 변경 부작용 
	let x= 10; //중복 선언 o
}
console.log(x) // 1 전역스코프
```

## 렉시컬 스코프 or 정적 스코프

자바스크립트에서는 함수를 **어디서 호출했는지가 아니라 어디서 정의 했는지**에 따라 상위스코프 결정

```jsx
var x = 1;

function f(){
	var x = 10;
	f2();
}

function f2(){
	console.log(x)
}

f(); //1   f실행컨텍 record x, f2.function obj..
f2(); //1

////////////////////////
var x = 1;

function f(){
	var x = 10;
	function f2(){
		console.log(x)
	}
}
f(); // 10
f2(); //참조에러
```

////////////////////////////////////////////////////////////////////////////////////

//

call stack 에선 최근에 추가된 실행 컨텍스트만 활성화 

호출스택(callstack) 

⇒함수를 실행하면 해당 함수의 기록을 스택 맨 위에 push합니다 

 결과 값을 반환하면 스택에 쌓여있던 함수는 pop

// callstack을 이해하기 위해 비동기 콜백 등 다양한 내용을 이해해야하므로 더 자세한 내용을 알아야합니다/////////

**Hositing**

선언문이 마치 최상단에 끌어올려진 듯한 현상

//Environement Record

식별자와 식별자에 바인딩된 값을 기록하는 객체

 

```jsx
console.log(hoTe); // undefined

var hoTe = 'value';

console.log(hoTe) // value
```

Variavle Hositing vs Function Hositing

Variavle Hositing

### 생성단계

var = 실행컨텍스트의 생성단계에서 선언문만 실행해서 Record에 기록 (미리 기록)

          **hote : undefined; 상태**

### 실행단계

선언문 외 나머지 코드 순차적 실행 (생성단계를 참조 및 업데이트)

          hote : value라고 업데이트

var 특징

선언 = 메모리 공간을 확보하고 식별자와 연결

초기화 = 식별자에 암묵적으로 undefined 값 바인딩

**선언과 초기화가 동시에 이루어짐**

const

hote를 기록은 하지만 undefined로 초기화 X

⇒선언문 이전에 hote를 참조하려면 Reference Error 발생

let, const  특징

선언 = 메모리 공간을 확보하고 식별자와 연결

초기화는 실행 X

선언문 전까지는 참조시에 Reference Error 

Function Hositing

```jsx
/* Global */
f(); // Type Error  // f는 undefined이기 때문에 호출 X

var f = () =>{
	//something..
}

========================

/* Global */
ff(); // Reference Error  // 아무 값도 없기 때문에 참조에러

const ff = () =>{
	//something..
}

//함수 선언문은 이전에 다뤘던 내용이기에 간단히
//선언과 동시에 함수가 생성되어 실행 가능
```

**렉시컬 스코프는 함수를 어디서 호출하는지가 아니라 어디에 선언하였는지에 따라 결정된다.**

### **식별자 결정**

//

변수섀도잉 가장 가까운 변수를 참조하기에 상위 스코프의 변수값은 무시

스코프 체인 식별자를 결정할 때 활용하는 스코프들의 연결리스트 타고타고타고타고

실행컨텍스트

⇒코드를 실행하는데 필요한 환경을 제공하는 객체