# 26장 ES6 함수의 추가 기능

## 26.1 함수의 구분

- ES6이전의 모든 함수는 일반 함수로서 호출할수 있는 것은 물론 생성자 함수로서 호출할 수 있다.
- 따라서, ES6이전의 모든 삼수는 callable(호출할수있는 함수객체)이면서 constructor(인스턴스를 생성할수있는 함수객체)다.

```jsx
var foo = function(){
	return 1;
}

//일반적인 함수로서 호출
foo();

//생성자 함수로서 호출
new foo();

//매서드로서 호출
var obj = {foo:foo}
obj.foo();//1
```

- 주의할 것은 ES6이전에 일반적으로 메서드라고 부르던 객체에 바인딩된 함수도 callable이면서 constructor이다.
- 따라서 객체에 바인딩 된 함수도 일반 함수로서 호출할 수 있는것은 물론 생성자 함수로서 호출할수도 있다.

```jsx
//프로퍼티 f에[ 바인딩된 함수는 callable이며 constructor이다.
var obj = {
	x : 10,
	f : function(){ return this.x; }
}

//프로퍼티 f에 바인딩된 함수를 메서드로서 호출
console.log(obj.f());//10

//프로퍼티 f에 바인딩된 함수를 일반 함수로서 호출
var bar = obj.f;
console.log(bar());//10

//프로퍼티 f에 바인딩된 함수를 생성자 함수로서 호출
console.log(new obj.f());
//객체에 바인딩된 함수를 생성자 함수로 호출하는 경우가 문법 상 가능하다는 것은 문제가 있다
//성능 면에서도 문제가 있다
//객체에 바인딩된 함수가 constructor라는 것은 객체에 바인딩된 함수가 prototype 프로퍼티를 가지며
//프로토타입 객체도 생성한다는것을 의미한다.
//콜백함수도 constructor이기 때문에 불필요한 프로토타입 객체를 생성한다.
```

- ES6 이전의 모든 함수는 사용 목적에 따라 명확한 구분이 없으므로 호출 방식에 제약이 없고 생성자 함수로 호출되지 않아도 프로토 타입 객체를 생성한다. → 혼란스러우며 실수를 유발할 가능성이 있다
- ES6 함수를 사용 목적에 따라 세가지 종류로 명확히 구분했다

| ES6 함수의 구분 | constructor | prototype | super | arguement |
| --- | --- | --- | --- | --- |
| 일반함수 | O | O | X | O |
| 매서드 | X | X | O | O |
| 화살표 함수 | X | X | X | X |

## 26.2 메서드

- ES6 사양에서 메서드는 메서드는 축약표현으로 정의된 함수만을 의미한다.

```jsx
const obj = {
	x : 1,
	foo(){
		return this.x;
	},
	//bar바인딩된 함수는 메서드가 아닌 일반 함수이다.
	bar : function(){
		return this.x;
	}
}
console.log(obj.foo());//1
console.log(obj.bar());//1
```

- ES6 사양에서 정의한 메서드는 인스턴스를 생성할 수 없는 non-constructor다
→ 인스턴스를 생성할 수 없고 prototype 프로퍼티가 없고 프로토 타입도 생성하지 않는다.

```jsx
new obj.foo(); //Uncaught TypeError: obj.foo is not a constructor
new obj.bar();

obj.foo.hasOwnProperty('prototype');//false
obj.bar.hasOwnProperty('prototype');//true
```

- 빌트인 객체가 제공하는 프로토타입 메서드와 정적 메서드는 모두 non-constructor다.
- ES6 메서드는 자신을 바인딩한 객체를 가리키는 내부 슬롯 [[HomeObject]]를 갖는다.
- super참조는 내부슬롯 [[HomeObject]]를 사용하여 수퍼클래스의 메서드를 참조하므로 내부 슬롯 [[HomeObject]]를 갖는 ES6메서드는 super 키워드를 사용할수있다.
- ES6메서드가 아닌 함수는 super키워드를 사용할 수 없다. ES6메서드가 아닌 함수는 내부슬롯 [[HomeObject]]를 갖지 않기 때문이다.
- ES6메서드는 본연의 기능(super)을 추가하고 의미적으로 맞지않은 기능(constructor)는 제거 했다.
- 따라서 메서드를 정의할때 프로퍼티 값으로 익명 함수 표현식을 할당하는 ES6이전의 방식은 사용하지 않는것이 좋다.

```jsx
const base = {
	name : 'Lee',
	sayHi(){
		return  `Hi ${this.name}`;	
	}
}

const derived = {
	__proto__ : base,
	//sayHi는 ES6메서드 [[HomeObject]]를 갖는다.
	//sayHi의 [[HomeObject]]는 sayHi가 바인딩된 객체인 derived를 가르키고
	//super는 sayHi의 [[HomeObject]]의 프로토타입 base를 가리킨다.
	sayHi(){
		return  `${super.sayHi()}. How are you doing?`;	
	}
}
console.log(derived.sayHi());
```

## 26.3 화살표함수

- 화살표 함수는 function 키워드 대신 화살표를 사용하여 기존의 함수정의 방식보다 더 간략하게 함수를 정의할수있다.

### 화살표 함수 정의

**함수정의**

- 화살표 함수는 함수 선언문으로 정의할수 없고 함수 표현식으로 정의해야한다.
- 호출 방식은 기존 함수와 동일하다

```jsx
const multiply = (x,y)=> x*y;
multiply(3,5);//15
```

**매개변수 선언**

- 매개변수가 여러개인 경우 소괄호 안에 매개변수를 선언한다.
한개인 경우 소괄호를 생략할수 있다.
없는 경우 소괄호를 생략할수 없다.

**함수 몸체정의**

- 함수 몸체가 하나의문으로 구성된다면 함수 몸체를 감싸는 중괄호를 생략할수 있다.
내부의 문이 값으로 평가될수 있는 표현식인 문이면 암묵적으로 반환한다.
- 함수 몸체를 감싸는 중괄호를 생략한 경우 함수 몸체 내부의 문이 표현식이 아닌 문이라면 에러가 발생한다. → 표현식이 아닌 문은 반환할수 없기 때문이다.
- 함수 몸체가 하나의 문으로 구성된다고 해도 함수 몸체의 문이 표현식이 아닌 문이라면 중괄호를 생략할수 없다.

```jsx
const arrow = ()=> const x = 1;//Uncaught SyntaxError: Unexpected token 'const'
```

- 객체리터럴로 반환하는 경우 리터럴을 소괄호로 감싸야된다
→ 감싸지 않으면 중괄호를 함수 몸체를 감싸는 중괄호로 잘못해석하기 때문

```jsx
const create = (id, content)=> ({id,content});
create(1,'Javascript');//{id: 1, content: 'Javascript'}
```

- 함수몸체가 여러개의 문으로 구성된다면 함수 몸체를 감싸는 중괄호를 생략할수 없다,
반환값이 있다면 명시적으로 반환해야한다.
- 화살표함수도 즉시 실행함수로 사용할수있다.

```jsx
const person = (name => ({
	sayHi(){return `Hi Myname is ${name}`}
}))('Suin');
console.log(person.sayHi());
```

- 화살표 함수도 일급 객체이므로 Array.prototype.map, Array.prototype.filter 등과 같은 고차 함수에 인수로 전달할수있다.

```jsx
[1,2,3].map(v => v * 2);//[2, 4, 6]
```

### 화살표 함수와 일반 함수의 차이

1. 화살표함수는 인스턴스를 생성할수 없는 non-constructor다
2. 중복된 매개변수 이름을 선언할수 없다.
3. 화살표 함수는 함수 자체의 this, arguments, super, new.target바인딩을 갖지않는다.

### this

- 화살표 함수가 일반함수과 구별되는 가장 큰 특징이 this다
- 화살표함수는 다른 함수의 인수로 전달되어 콜백 함수로 사용되는 경우가 많다.
- 화살표 함수의 this는 일반함수의 this와 다르게 동작한다. → 콜백함수 내부의 this문제 즉, 콜백함수 내부의 this가 외부함수의 thist와 다르기 때문에 발생하는 문제를 해결하기 위해 의도적으로 설계된것이다.
- ES6에서는 화살표 함수를 사용하여 콜백 함수 내부의 this 문제를 해결할수 있다
- 화살표함수는 함수자체의 this 바인딩을 갖지 않는다.
→ 화살표 함수 내부에서 this를 참조하면 상위스코프의 this를 그대로 참조한다. - 이를 lextical this라한다.= 마치 렉시컬 스코프와 같이 화살표 함수의 this가 함수가 정의된 위치에서 의해 결정된다는것을 의미한다.
- 화살표 함수를 Function.prototype.bind를 사용하여 표현하면 다음과 같다.

```jsx
//화살표함수는 상위스코프의 this를 참조한다.
() => this.x;
//익병함수에 상위 스코프의 this를 주입한다. 위화살표 함수와 동일하게 동작한다.
(function(){return this.x}).bind(this);
```

- 만약 화살표 함수와 화살표 함수가 중첩되어있다면 상위 화살표 함수에도 this 바인딩이 없으므로 스코프 체인상 가장가까운 상위 함수 중에서 화살표 함수가 아닌 함수의 this를 참조한다.

```jsx
//중첩함수 foo의 상위 스코프는 즉시 실행함수다.
//따라서 화살표함수 foo의 this는 상위 스코프인 즉시 실행 함수의 this를 가리킨다.
(function(){
	const foo = () => console.log(this);
	foo();
}).call({a:1})//{a: 1}

//bar 함수는 화살표함수를 반환한다
//bar 함수가 반환한 화살표 함수의 상위 스코프는 화살표 함수 bar다
//화살표함수는 this바인딩을 갖지 않기 때문에
//bar 함수가 반환한 화살표 함수 내부에서 참조하는 this화살표 함수나 아닌
//즉시 실행 함수 this를 가리킨다.
(function(){
	const bar  = () => () => console.log(this);
	bar()();
}).call({a:1})//{a: 1}
```

- 만약 화살표 함수가 전역 함수라면 화살표 함수의 this는 전역객체를 가리킨다.
- 프로퍼티에 할당한 화살표 함수도 스코프 체인상에서 가장 가까운 함수 중에서 화살표 함수가 아닌 함수의 this를 참조한다.

```jsx
//increase 프로퍼티에 할당한 화살표 함수의 상위 스코프는 전역이다
//따라서 increase 프로퍼티에 할당한 화살표 함수의 this는 전역객체를 가리킨다.
const counter = {
	num : 1,
	increase : () => ++this.num
}
console.log(counter.increase());//NaN
```

- 화살표 함수는 함수자체의 this 바인딩을 갖지 않게 때문에 Function.prototype.call, Function.prototype.apply, Function.prototype.bind 메서드를 사용해도 화살표 함수 내부의 this를 교체할수 없다.
- Function.prototype.call, Function.prototype.apply, Function.prototype.bind메서드를 호출할 수 없다는 의미는 아니다.
화살표 함수는 함수자체의 this바인딩을 갖지 않기 때문에 this를 교체할수 없고 언제나 상위 스코프의 this 바인딩을 참조한다.

```jsx
const add = (a,b)=>a+b;
console.log(add.call(null,1,2));//3
```

- 매서드를 화살표 함수로 정의 하는것은 피해야한다. - 화살표 함수 내부의 this는 상위스코프의 this를 가리키기 때문에
- 프로토타입객체의 프로퍼티에 화살표함수를 할당하는 경우도 동일한 문제가 발생한다.

```jsx
const person = {
	name : 'Suin',
	sayHi : () => console.log(`Hi ${this.name}`);
}

//sayHi 프로퍼티에 할당된 화살표 함수 내부의 this는 상위 스코프인 전역 this가 가르키는
//전역잭체를 가리키므로 이 예제를 브라우저에서 실행하면
//this.name은 빈 문자열을 갖는 window.name과 같다
//전역객체 window에는 빌트인 프로퍼티 name이 존재한다.
person.sayHi()//Hi
```

- 일반함수가 아닌 ES6메서드를 동적 추가 하고 싶다면 다음과 같이 객체 리터럴을 바인딩하고 프로토 타입의 constructor 프로퍼티와 생성자 함수간의 연결을 재설정한다.

```jsx
function Person(name){
	this.name = name;
}
Person.prototype = {
	//constructor 프로퍼티와 생성자 함수 간의 연결을 재설정
	constructor : Person,
	sayHi(){console.log(`Hi ${this.name}`);}
};

const person = new Person('Lee');
person.sayHi();//Hi Lee
```

- 클래스 필드 정의 제안을 사용하여 클래스 필드에 화살표 함수를 할당할수도 있다..

```jsx
class Person{
	//클래스필드 정의 제안
	name = 'Lee';
	sayHi = () => console.log(`Hi ${this.name}`);
}
const person = new Person();
person.sayHi();//Hi Lee
```

- sayHi필드에 할당한 화살표 함수의 상위 스코프는 클래스 외부다
하지만 this 는 클래스 외부를 참조하지않고 클래스가 생성할 인스턴스를 참조한다.
→ sayHi클래스 필드에 할당한 화살표함수내부에서 참조한 this는 constructor 내부의 this 바인딩과 같다.
- constructor 내부의 this 바인딩은 클래스가 생성한 인스턴스를 가리키므로 sayHi클래스 필드에 할당한 화살표 함수 내부의 this 또한 클래스가 생성한 인스턴스를 가리킨다.
- 하지만 클래스 필드에 할당한 화살표함수는 프로토 타입 매서드가 아니라 인스턴스메서드가 된다. → 메서드를 정의할때는 ES6메서드 축약표현으로 정의한 ES6메서드를 사용하는것이 좋다.

```jsx
class Person{
	//클래스필드 정의 제안
	name = 'Lee';
	sayHi(){console.log(`Hi ${this.name}`);}
}
const person = new Person();
person.sayHi();//Hi Lee
```

this 길고 어렵네용…

저는 대충 this를 쓰면 상위 스코프의 this를 사용한다.

이로인해 프로토타입객체의 프로퍼티에 화살표함수를 할당, 프로토타입객체의 프로퍼티에 화살표함수를 할당, 클래스 필드에 화살표 함수를 할당 등에는 쓰지 말자 - 왜 문제인지 설명 이런 느낌으로 봤습니다.

### super

- 화살표 함수는 함수 자체의 super 바인딩을 갖지 않는다.
- 따라서 화살표 함수 내부에서 super를 참조하면 this와 마찬가지로 상위 스코프의 super를 참조한다.

```jsx
class Base{
	constructor(name){
		this.name = name;
	}
	sayHi(){
		return `Hi ${this.name}`;
	}
}

class Derived extends Base{
	//화살표 함수의 상위 스코프인 constructor의 super를 가리킨다.
	sayHi = () => `${super.sayHi()} how are you doing?`;
}

const derived = new Derived('Suin');
console.log(derived.sayHi());//Hi Suin how are you doing?
```

- super는 내부슬롯을 [[HomeObject]]는 갖는 ES6메서드 내에서 사용할 수 있는 키워드다.
- sayHi클래스 필드에 할당한 화살표 함수는 ES6메서드는 아니지만 함수 자체의 super 바인딩을 갖지 않으므로 super를 참조해도 에러가 발생하지 않고 constructor의 super 바인딩을 참조한다.
- this와 마찬가지로 클래스필드에 할당한 화살표 함수 내부에서 super를 참조하면 constructor내부의 super 바인딩을 참조한다.

### arguments

- 화살표함수는 함수 자체의 arguments바인딩을 갖지않는다
- 따라서 화살표함수 내부에서 arguments를 참조하면 this와 마찬가지로 상위 스코프의 arguments를 참조한다.
- 화살표 함수로 가변인자 함수를 구현해야할때는 Rest 파라미터를 사용해야한다.

## 26.4 Rest 파라미터

### 기본문법

- Rest파라미터(나머지 매개변수)는 매개변수 이름앞에 세개의 점을 붙여서 정의한 매개변수를 의미한다.
- Rest파라미터는 함수에 전달된 인수들의 목록을 배열로 전달받는다.
- 일반 매개변수와 Rest파라미터는 함께 사용할 수 있다.
인수들은 매개변수와 Rest파라미터에 순차적으로 할당된다.

```jsx
function foo(pram, ...rest){
	console.log(param);
	console.log(rest);
}
foo(1,2,3,4,5);
```

- Rest파라미터는 이름 그대로 먼저 선언된 매개변수에 할당된 인수를 제외한 나머지 인수들로 구성된 배열이 할당된다. → Rest파라미터는 마지막 파라미터여야된다.
- Rest파라미터는 단 하나만 선언할수있다.

```jsx
function foo(...rest, pram1, param2){}
//Uncaught SyntaxError: Rest parameter must be last formal parameter

function foo(...rest1, ...rest2){}
//Uncaught SyntaxError: Rest parameter must be last formal parameter
```

- Rest파라미터는 함수객체의 length프로퍼티에 영향을 주지 않는다.

```jsx
function foo(...rest){}
console.log(foo.length);

function foo1(param1, ...rest){}
console.log(foo1.length);
```

### Rest파라미터와 arguments객체

- artuments객체는 함수 호출시 전달된 인수들의 정보를 담고 있는 순회 가능한 유사배열 객체
- artuments객체는 유사배열 객체이므로 배열객체를 사용하려면 Function.prototype.call이나 Function.prototype.apply메서드를 사용해 artuments객체를 배열로 변환해야하는 번거로움이있다.
- ES6에서는 Rest파라미터를 사용하여 가변 인자 함수의 인수 목록을 배열로 직접 전달 받을수 있다.
- 함수와 ES6 메서드는 Rest파라미터와 artuments객체를 모두 사용할 수 있다.
- 하지만 화살표함수는 artuments를 갖지 않는다. 화살표함수로 가변인자 함수를 구현해야할때는 반드시 Rest파라미터를 사용해야한다.

## 26.5 매개변수 기본값

- 함수를 호출할 때 매개변수의 개수만큼 인수를 전달하는것이 바람직하지만 그렇지 않은 경우에도 에러가 발생하지 않는다.
→ 자바스크립트 엔진이 매개변수의 개수와 인수 개수를 체크하지 않기 때문
- 인수가 전달되지 않은 매개변수의 값은 undefined다.
- 매개변수에 인수가 전달되었는지 확인하여 인수가 전달되지 않은경우에 매개변수 기본값을 할당할 필요가 있다.
- ES6에서 도입된 매개변수 기본값을 사용하면 함수내에서 수행하던 인수체크및 초기화를 간소화 할 수 있다.

```jsx
function sum(x,y){
	//인수가 전달되지 않아서 매개변수의 값이 undefined인 경우 기본값을 할당한다.
	x = x || 0;
	y = y || 0;
}
console.log(sum(1,2));
console.log(sum(1));

function sum(x = 0, y = 0){
	//인수가 전달되지 않아서 매개변수의 값이 undefined인 경우 기본값을 할당한다.
	x = x || 0;
	y = y || 0;
}
console.log(sum(1,2));
console.log(sum(1));
```

- Rest파라미터에는 기본값을 지정할수 없다.

```jsx
function foo(...rest = []){
	console.log(rest);
}
foo();
//Uncaught SyntaxError: Rest parameter may not have a default initializer
```

- 매개변수 기본값은 함수 정의 시 선언한 매개변수 개수를 나타내는 함수 객체의 length 프로퍼티와 arguements객체에 아무런 영향을 주지 않는다.