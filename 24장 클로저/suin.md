# 24장 클로저

MDN에서의 정의

클로저는 함수와 그 함수가 선언된 렉시컬 환경과의 조합이다.

```jsx
const x = 1;
function outerFunc(){
	const x = 10;
	function innerFunc(){
		console.log(x); //10
	}
	innerFunc();
}
outerFunc();
//outer 함수 내부에 중첩함수 innerFunc가 정의되고 호출되어있음.
//이때 중첩함수 innerFunc의 상위 스코프는 외부 함수 outerFunc의 스코프
//따라서 중첩함수 innerFunc 내부에서 자신을 포함하고 있는 외부함수 outerFunc의 x변수에 접근할 수있다.

const x = 1;
function outerFunc(){
	const x = 10;
	innerFunc();
}

function innerFunc(){
	console.log(x); //1
}

outerFunc();

//만약 innerFunc 함수가 outerFunc 함수의 내부에서 정의된 중첩 함수가 아니라면
//innerFunc 함수를 outerFunc함수 내부에 호출한다 하더라도 outerFunc함수의 변수에 접근할수 없다.
```

## 24.1 렉시컬 스코프

- 자바스크립트 엔진은 함수를 어디서 호출했는지가 아니라 함수를 어디에 정의 했는지에따라 상위 스코프를 결정한다. - 렉시컬스코프라고 한다.
- 함수의 상위 스코프는 함수를 정의한 위치에 의해 정적으로 결정되고 변하지 않는다.
- 렉시컬 환경의 “외부 렉시컬 환경에 대한 참조”에 저장할 참조값, 즉 상위스코프에 대한 참조는 함수 정의가 평가되는 시점에 함수가 정의된 환경에 의해 경정된다. → 렉시컬 스코프

## 24.2 함수 객체의 내부 슬롯

- 함수가 정의된 환경(위치)과 호출되는 환경(위치)은 다를수 있다. → 렉시컬 스코프가 가능하려면 호출되는 환경과는 상관없이 자신이 정의된 환경 (즉 상위 스코프)를 기억해야한다
- 이를 위해 함수는 자신의 내부슬롯[[Environment]]에 자신이 정의된 환경, 즉 상위 스코프의 참조를 정의한다.
    - 함수 정의 평가되어 함수객체를 생성할때 자신이 정의된 환경에 의해 결정된 상위 스코프의 참조를 함수객체 자신의 내부슬롯 [[Environment]]에 저장한다.
    - 이때 자신의 내부슬롯 [[Environment]]에 저장된 상위 스코프의 참조는 현재 실생중인 실행 컨텍스트의 렉시컬 환경을 가르킨다.
- 함수 객체의 내부슬롯 [[Environment]]에 저장된 현재 실행중인 실행 컨텍스트의 렉시컬 환경의 참조가 바로 상위 스코프다.
    - 또한 자신이 호출되었을때 생성될 함수 렉시컬 환경의 “외부 렉시컬 환경에 대한 참조”에 저장될 참조 값이다.
    - 함수객체는 내부슬롯 [[Environment]]에 저장한 렉시컬 환경의 참조 즉 상위 스코프를 자신이 존재하는 한 기억한다.

```jsx
const x = 1;
function foo(){
	const x = 10;
	//상위 스코프는 함수 정의 환경(위치)에 따라 결정된다.
	//함수 호출 위치와 상위 스코프는 아무런 관계가 없다
	bar();
}

//함수 bar는 자신의 상위 스코프, 즉 전역 렉시컬 환경을 [[Environment]]에 저장하여 기억한다.
function bar(){
	console.log(x);
}
foo();
bar();
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4b90ddbd-9270-4e2b-b13a-20dd97431d50/Untitled.png)

## 24.3 클로저와 렉시컬 환경

```jsx
const x = 1;
//
function outer(){
	const x = 10;
	const inner function(){console.log(x);}
	return inner;
}

//outer 함수를 호출하면 중첩 함수 inner를 반환한다
//그리고 outer함수의 실행 컨택스트는 실행컨택스트 스택에서 팝되어 제거된다.
const innerFunc = outer();
innerFunc();//10

//outer 함수를 호출하면 outer 함수는 중첩함수 inner를 반환하고 생명주기를 마감한다.
//즉, outer함수의 실행이 종료되면 outer함수의 실행컨택스트 스택에서 제거된다.
//이때 outer함수의 지역변수 outer함수의 실행컨택스트가 제거 되었으므로
//outer 함수의 지역 변수 또한 생명주기를 마감한다.
//따라서 진역변수 x는 더 유효하지 않게되어 지역변수 x에 접근 할수 있는 방법은 달리없으나
//실행결과 outer 함수의 지역변수 x의 값인 10이다.
//생명주기가 종료되어 실행컨택스트 스택에서 제거되어도 outer함수의 지역변수가 다시 부활한것처럼 동작하고 있다.
```

- 외부 함수보다 중첩 함수가 더 오래 유지되는 경우 중첩 함수는 이미 생명 주기가 종료한 외부함수의 변수를 참조할수 있다 - 이러한 중첩함수를 클로저라고 부른다
- 클로저는 함수와 그 함수가 선언된 렉시컬 환경과의 조합이다.
- 위 예제에서 inner 함수는 자신이 평가될때 자신이 정의된 위치에 의해 결정된 상위 스코프를 [[Environment]] 내부 슬롯에 저장한다. - 이때 저장된 상위 스코프는 함수가 존재하는 한 유지된다.
- outer 함수의 실행이 종료되면서 inner함수를 반환하면서 outer 생명주기가 종료 → outer 함수 실행 컨택스트가 실행컨텍스트 스택에서 제거되지만 outer 함수의 렉시컬 환경까지 소멸되지 않는다.
- outer 함수의 렉시컬 환경은 inner 함수의 [[Environment]] 내부슬롯에 의해 참조되고 있고 inner 함수는 전역면수 inner Func에의해 참조되어있으므로 가비지 컬렉션 대상이 되지 않는다.
- 외부함수보다 중첩함수가 더 오래 유지되는 경우 중첩함수는 이미 생명주기가 종료된 외부 함수의 변수를 참조 할수 있다. → 이러한 중첩함수를 클로저라고 한다.
클로저는 중첩함수가 상위 스코프의 식별자를 참조하고 있고 중첩함수가 외부함수보다 더 오래 유지되는 경우에 한정하는것이 일반적이다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7d26f468-5319-4a11-bfd0-abb9ea661856/Untitled.png)

## 24.4 클로저의 활용

- 클로저는 상태를 안전하게 변경하고 유지하기 위해 사용한다.
- 상태가 의도치 않게 변경되지 않도록 상태를 안전하게 은닉하고 특정 함수에게만 상태변경을 허용하기 위해 사용한다.

```jsx
let num = 0;

const increase = function(){
	return ++num;
}
console.log(increase());
//1. 카운트 상태는 increase함수가 호출되기 전까지 변경되지 않고 유지되어야한다.
//2. 이를 위해 카운트 상태는 increase 함수만이 변경할 수 있어야한다.
//그러나 전역변수를 통해 관리 되고 있어 언제든 누구나 접근할수 있고 변경이 가능하기 때문에
//오류를 발생시킬 가능성을 내포하고 있는 좋지 않은 코드다.-?

const increase = (function(){
	let num = 0;
	//클로저
	return function(){
		return ++num;
	};
}());

console.log(increase());//1
console.log(increase());//2
console.log(increase());//3
//즉시실행 함수는 한번만 실행 되므로 INCRESE가 호출될때마다 num 변수가 초기화 될일은 없다.
//은닉된 외부에서 직접 접근할수 없는 Private 변수이므로
//의도되지 않은 변경을 걱정할 필요도 없어 안정적인 프로그래밍이 가능하다
```

- 클로저는 상태가 의도치 않게 변경되지 않도록 은닉하고 특정함수에게만 상태 변경을 허용하여 상태를 안전하게 변경하고 유지하기 위해 사용한다.

```jsx
const counter = (function(){
	let num = 0;
	//클로저인 메서드를 갖는 객체를 반환한다.
	//객체 리터럴은 스코프를 만들지 않는다.
	//따라서 아래 매서드들의 상위 스코프는 즉시 실행함수의 랙시컬 환경이다.
	return {
		increase(){
			return ++num;
		},
		decrease(){
			return --num;
		},
	}
}());

console.log(counter.increase());//1
console.log(counter.increase());//2

console.log(counter.decrease());//1
console.log(counter.decrease());//0

//생성자 함수로 표현
const Counter = (function(){
	let num = 0;
	function Counter(){
		//this.num = 0; //프로퍼티는 public하므로 은닉되지 않는다.
	}
	
	Counter.prototype.increase = function(){
		return ++num;
	};

	Counter.prototype.decrease = function(){
		return --num;
	}

	return Counter;
}());

const counter = new Counter();

console.log(counter.increase());//1
console.log(counter.increase());//2

console.log(counter.decrease());//1
console.log(counter.decrease());//0

//함수형 프로그래밍에서 클로저를 활용하는 간단한 예제
function makeCouter(aux){
	let counter = 0;
	
	//클로저를 반환
	return function(){
		//인수로 전달 받은 보조 함수에 상태 변경을 위임한다.
		counter = aux(counter);
		return counter;
	};
}
//보조함수
function increase(num){
	return ++num;
}

//보조함수
function decrease(num){
	return --num;
}

//함수로 함수를 생성한다.
//makeCounter 함수는 보조함수를 인수로 전달받아 함수를 반환한다.
const increaser = makeCouter(increase);
console.log(increaser());//1
console.log(increaser());//2

//increaser함수와는 별개의 독립된 렉시컬 환경을 갖기 때문에 카운터 상태가 연동되지 않는다.
//주의..
const decreaser = makeCouter(decrease);
console.log(decreaser());//-1
console.log(decreaser());//-2

//독립된카운터가 아니라 연동하여 증감이 가능한 카운터를 만들려면
//렉시컬 환경을 공유하는 클로저를 만들어야된다.
//makeCounter 함수를 두번 호출하지 말아야한다.
const counter = (function(){
	let counter = 0;
	return function (aux){
		//인수로 전달 받은 보조 함수에 상태 변경을 위임한다.
		counter = aux(counter);
		return counter;
	};
}());

//보조함수
function increase(num){
	return ++num;
}

//보조함수
function decrease(num){
	return --num;
}

//보조함수를 전달하여 호출
console.log(counter(increase));//1
console.log(counter(increase));//2
console.log(counter(decrease));//1
console.log(counter(decrease));//0
```

## 24.5 캡슐화와 정보은닉

- 캡슐화는 객체의 상태를 나타내는 프로퍼티와 프로퍼티를 참조하고 조작할 수 있도록 동작인 메서드를 하나로 묶는 것을 말한다.
- 캡슐화는 객체의 특정프로퍼티나 메서드를 감출 목적으로 사용하기도 하는데 → 정보은닉이라한다.
- 정보은닉은 외부에 공개할 필요가 없는 구현의 일부를 외부에 공개되지 않도록 감추어 적절치 못한 접근으로부터 객체의 상태가 변경되는 것을 방지해 정보를 보호하고 객체간의 상호 의존성 즉 결합도를 낮추는 효과가 있다
- 자바스크립트는 public, private, protected와 같은 접근 제한자를 제공하지 않는다 → 자바스크립트 모든 프로퍼티와 메서드는 기본적으로 외부에 공개되어있다. → 객체의 모든 프로퍼티와 메서드는 기본적으로 public 하다
- 자바스크립트는 정보은닉을 완전하게 지원하지 않는다.
- 인스턴스 메서드를 사용한다면 자유변수를 통해 private를 융대 낼수는 있지만 프로토타입 메서드를 사용하면 불가능해진다.
- TC39프로세스의 stage3에는 클레스에 private 필드를 정의할수 있는 새로운 표준사양이 제안되어있다.
→ 크롬74이상 최신Node.js에 이미구현되어있다. - 이부분은 25.7.4절에 있다

## 24.1 자주 발생하는 실수

```jsx
var funcs = [];
for(var i = 0; i < 3; i++){
	funcs[i] = function(){return i};
}
for(var j = 0; j < funcs.length; j++){
	console.log(funcs[j]());
}
//3이 3번 출력된다
//왜?
//var 키워드로 i변수는 블록레벨 스코프가아닌 함수 레벨 스코프를 갖지 때문에 전역변수이다.
//funcs배열 요소에 추가한 함수를 호출하면 적역변수 i를 참조한다.

//클로저를 사용해서 동작되게
var funcs = [];
for(var i = 0; i < 3; i++){
	funcs[i] = (function(id){
		return function (){return id}
	}(i));
}
for(var j = 0; j < funcs.length; j++){
	console.log(funcs[j]());
}

//let을 사용하면 깔끔히 해결된다.
var funcs = [];
for(let i = 0; i < 3; i++){
	funcs[i] = function(){return i};
}
for(var j = 0; j < funcs.length; j++){
	console.log(funcs[j]());
}
```

- let이나 const 키워드를 사용하는 반복문은 코드 블록을 반복실행할때마다 새로운 렉시컬 환경을 생성하여 반복할 당시의 상태를 마치 스냅숏을 찍는것처럼 저장한다.
- 단, 이는 반복문의 코드 블록 내부에서 함수를 정의할때 의미가 있다.
반복문의 코드 블록 내부에 함수 정의가 없는 반복문이 생성하는 새로운 렉시컬 환경을 반복 직후 아무도 참조 하지 않기 때문에 가비지 컬렉션의 대상이된다.
- 다른방법으로 함수형 프로그래밍 기법인 고차함수를 사용하는 방법이 있다.