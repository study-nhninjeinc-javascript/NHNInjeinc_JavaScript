# ES6 함수의 추가 기능

<aside>
💡 ES6 이전의 함수는 일반 함수로서 호출 할 수 있는 것은 물론 생성자 함수로서 호출할 수 있다.  - 메서드도 포함이다.(ES6 이전의 모든 함수는 callable 이면서 constructor다.)

</aside>

## 메서드

> ES6 사양에서 메서드는 메서드 축약 표현으로 정의된 함수만을 의미한다.
> 
- ES6 메서드는 인스턴스를 생성할 수 없는 non-constructor 이다.
    - prototype 프로퍼티가 없고 프로토타입도 생성하지 않는다.
- ES6 메서드는 자신을 바인딩한 객체를 가리키는 내부 슬롯 [[HomeObject]] 를 갖는다. (super 참조)

## 화살표 함수

```jsx
const multiply = (x, y) => x * y;
multiply(2, 3);

// 매개변수 하나인 경우 괄호 생략 가능
const singleply = x => { ... };
 
// 매개변수가 없는 경우에는 괄호를 생략할 수 없다.
const noneply = () => { ... };

// 함수 몸체를 감싸는 블록을 생략한 경우 내부 문이 표현식이 아닌 문이라면 에러가 발생.
const moon = () => x ** 2;

// 객체 리터럴을 반환하는 경우 객체 리터럴을 소괄호로 감싸주어야 한다.
const objArrow = (id, content) => ({ id, content });

// 화살표 함수도 즉시 실행 함수로 사용할 수 있다.
const person = (name => ({
	sayHi() { return `Hi? My name is ${name}.`; }
}))('Lee');

// 화살표 함수도 일급 객체이므로 고차 함수에 인수로 전달할 수 있다.
[1, 2, 3].map(v => v * 2);
```

### 화살표 함수와 일반 함수의 차이

- 화살표 함수는 인스턴스를 생성할 수 없는 non-constructor다.
- 화살표 함수는 중복된 매개변수 이름을 선언할 수 없다.
    - 일반 함수는 중복된 매개변수 이름을 선언해도 에러 발생하지 않음 → strict mode 에서는 에러 발생
- 화살표 함수는 함수 자체의 this, arguments, super, [new.target](http://new.target) 바인딩을 갖지 않는다.

### this

> 화살표 함수가 일반 함수와 구별되는 가장 큰 특징은 this다.
> 

<aside>
💡 22장에서 배운 this 에 따르면 this 바인딩은 함수의 호출 방식에 따라 동적으로 결정된다.

</aside>

```jsx
// 배열의 각 요소에 접두어를 추가하는 콜백함수
class Prefixer {
	constructor(prefix) {
		this.prefix = prefix;	
	}

	add(arr) {
		// 인수로 전달된 배열을 순회하며 배열의 모든 요소에 prefix 추가
		return arr.map(function (item) {
			return this.prefix + item; // type error
		};
	}
}

const prefixer = new Prefixer('-webkit-');
console.log(prefixer.add(['transition', 'user-selet']));
```

- 일반 함수로서 호출되는 모든 함수 내부의 this는  전역 객체를 가리킨다.
    - 그러나 strict mode가 적용된 코드는 일반 함수로서 호출된 함수 내부의 this 에는 undefined 가 바인딩된다.
    - class 내부의 모든 코드에는 strict mode가 적용된다.
- 이런 콜백 함수 내부 this 문제를 해결하기 위해 ES6 이전에는
    - 미리 this 객체를 회피시키거나
    - map 메서드의 두번째 매개변수를 이용
        - map 메서드의 두번째 매개변수는 콜백 함수 내부에서 사용할 this에 바인딩 될 값을 받는다.
    - 혹은 bind 메서드를 통해 바인딩 시키는 방법을 이용했다.

```jsx
// ES6 이후 화살표 함수를 사용하여 콜백 함수 내부의 this 문제 해결
class Prefixer {
	constructor(prefix) {
		this.prefix = prefix;	
	}

	add(arr) {
		// 화살표 함수를 콜백함수로 사용.
		return arr.map(item => this.prefix + item);
	}
}

const prefixer = new Prefixer('-webkit-');
console.log(prefixer.add(['transition', 'user-selet']));
```

- 화살표 함수는 함수 자체의 this 바인딩을 갖지 않기 때문에 call, apply, bind 메서드를 사용해도 함수 내부 this를 교체할 수 없다.
- 메서드를 화살표 함수로 정의하는 것은 피해야 한다.

```jsx
const person = {
	name : 'Lee',
	sayHi : () => console.log(`Hi ${this.name`);
};

person.sayHi(); // Hi
```

### super

> 화살표 함수는 함수 자체의 super 바인딩을 갖지 않는다. (this와 마찬가지로 상위 스코프의 super를 참조하게 된다.)
> 

```jsx
class Base {
	constructor(name) {
		this.name = name;
	}

	sayHi() {
		return `Hi! ${this.name}`;
	}
}

class Derived extends Base {
	// 화살표 함수의 super는 상위 스코프인 constructor의 super를 가리킨다.
	sayHi = () => `${super.sayHi()} how are you doing?`;
}

const derived = new Derived('Lee');
console.log(derived.sayHi()); // Hi! Lee how are you doing?
```

### arguments

> 화살표 함수는 함수 자체의 arguments 바인딩을 갖지 않는다. (상위 스코프의 arguments를 참조한다.)
> 

```jsx
(functiopn() {
	consst foo = () => console.log(arguments);
	foo(3, 4);
}(1, 2));

const foo = () => console.log(arguments);
foo(1, 2); // error -> arguments is not defined
```

## Rest 파라미터

> Rest 파라미터는 함수에 전달된 인수들의 목록을 배열로 전달받는다.
> 

```jsx
function foo (... rest) {
	// 매개변수 rest는 인수들의 목록을 배열로 전달받는 Rest 파라미터다.
	console.log(rest); // [1, 2, 3, 4, 5]
}

foo(1, 2, 3, 4, 5);
```

- 일반 매개변수와 Rest 파라미터는 함께 사용할 수 있다.
- Rest 파라미터는 항상 마지막 파라미터이어야 한다.
- Rest 파라미터는 하나만 선언할 수 있다 (하나의 파라미터로만 사용 가능하다?)
- Rest 파라미터는 length 프로퍼티에 영향을 주지 않는다.

### Rest 파라미터와 arguments

> arguments 객체는 배열이 아닌 유사 배열 객체이므로 배열 메서드를 사용하려면 배열로 변환해야한다.
> 
- ES6 이후 Rest 파라미터는 배열로 직접 전달받기 때문에 배열로 변환하지 않고 바로 배열 메서드를 사용할 수 있다.

## 매개변수 기본값

> 함수 호출 시 매개변수의 개수만큼 인수가 전달되지 않아도 에러가 발생하지 않는다. → 의도치 않은 결과가 나올 수 있다.
> 

```jsx
// 의도치 않은 결과를 방지하기 위해 방어 코드가 필요하다.
function sum(x, y) {
	// 인수가 전달되지 않아 매개변수의 값이 undefined인 경우 기본값을 할당.
	x = x || 0;
	y = y || 0;

	return x + y;
}

console.log(sum(1, 2)); // 3
console.log(sum(1)); // 1
```

```jsx
// ES6 에서 도입된 매개변수 기본값 이용
function sum(x = 0, y = 0) {
	return x + y;
}

console.log(sum(1, 2)); // 3
console.log(sum(1)); // 1
```