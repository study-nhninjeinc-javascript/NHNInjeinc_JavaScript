# 18장 함수와 일급 객체

## 일급 객체

### 일급 객체의 조건

```jsx
// 무명 리터럴 함수 (익명 함수)
function () {};

// 무명 리터럴로 생성이 가능하다. + 변수에 저장할 수 있다.
let nonamedFunc = function() {};

// 객체나 배열 등에도 저장할 수 있다.
function forObj() {};
let obj = { forObj };
```

```jsx
function getFunction(func) {
	let num = 0;

	return function() { // 반환값으로 함수를 반환할 수 있다.
		num = func(num);
		return num;
	};
}

function increase(num) {
	return ++num;
}

const finalNum = getFunction(increase); // 함수를 매개변수에 전달할 수 있다.
console.log(finalNum()); // 1
console.log(finalNum()); // 2
```

<aside>
💡 일반 객체는 호출할 수 없지만 함수 객체는 호출할 수 있다. 또한 함수 객체는 일반 객체에는 없는 함수 고유의 프로퍼티를 소유한다.

</aside>

## 함수 객체의 프로퍼티

### 객체와 함수의 프로퍼티 어트리뷰트

```jsx
const obj = { number: 10 };

console.dir(obj);

console.log(Object.getOwnPropertyDescriptors(obj));
```

![OBJECT_DIR.PNG](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d9e49758-c39e-44c2-bb08-6873bf50ce2e/OBJECT_DIR.png)

```jsx
function square(number) {
	return number * number;
}

console.dir(square);
console.log(Object.getOwnPropertyDescriptors(square));
```

![FUNC_DIR.PNG](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/fe08076e-46ee-47a9-8f06-3a9a6e73749a/FUNC_DIR.png)

### arguments

- 함수 호출 시 전달된 인수들의 정보를 담고 있는 순회 가능한 유사 배열 객체
- 함수 외부에서는 참조할 수 없으며 함수 내부에서 지역 변수처럼 사용된다.

```jsx
function useArguments(x, y) {
	console.log(arguments);
	return x + y;
}

console.log(useArguments()); // Arguments(0) - NaN
console.log(useArguments(1, 2)); // Arguments(2) 0: 1, 1: 2 - 3
console.log(useArguments('Hello', 'World', 's')); // Arguments(3) 0: 'Hello', 1: 'World', 2: 's' - HelloWorld
```

### 결과

![noneArgs.PNG](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/eee08312-77b8-41ac-8d57-e1d93cdfef1a/noneArgs.png)

![correcArgs.PNG](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/734ad3e7-fdad-42b9-838e-d98735ce90ce/correcArgs.png)

![moreArgs.PNG](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/affeb0a0-aecc-4c99-8522-3eb5a931e5f2/moreArgs.png)

<aside>
💡 arguments는 배열이 아니므로 배열 메서드를 사용할 수 없다! → 배열 메서드 사용하려면 간접 호출을 해야한다. (22장에서 알아보도록 하자)
+ 이런 번거로움을 해결하기 위해 ES6 에서는 Rest 파라미터를 도입했다. → 이건 26장에서 알아보자..

</aside>

### caller

<aside>
💡 caller는 비표준 프로퍼티로 참고로만 알아두자 (브라우저와 Node.js 환경에서의 결과가 다를 수 있다.)

</aside>

```jsx
function myFunc(func) {
	return func();
}

function yourFunc() {
	return 'caller : ' + bar.caller;
}

console.log(myFunc(yourFunc)); // caller : function myFunc(func) { ... }

console.log(yourFunc()); // caller : null
```

### length

```jsx
function lengthFunction(a) {
	console.log(this.length === arguments.length);
}

lengthFunction(1); // true
lengthFunction(1, 2); // false
```

### name

<aside>
💡 ES5 와 ES6 에서 동작을 달리한다. 익명 함수 표현식의 경우 ES5에서는 빈 문자열을, ES6에서는 함수 객체를 가리키는 식별자를 값으로 갖는다.

</aside>

```jsx
let namedFunction = function functionName() {};

console.log(namedFunction.name); // functionName
// -> 기명 함수 표현식으로 정의된 함수의 name 프로퍼티는 함수의 이름을 값으로 갖는다.

let nonamedFunction = function() {}

console.log(nonamedFunction.name); // nonamedFunction
// -> 익명 함수 표현식으로 정의된 함수의 name 프로퍼티는 ES5 에서 빈 문자열을 갖는다. (ES6 이후에는 식별자를 값으로 갖는다.)

function declareFunction() {};
console.log(declareFunction.name); // declareFunction
// -> 함수 선언문은 암묵적으로 식별자가 생성된다. (함수 이름이 식별자가 된다.)
```

### __**proto__**

```jsx
const obj = { a: 1 };

// 객체 리터럴로 생성한 객체의 프로토타입 객체는 Object.prototype이다.
console.log(obj.__poroto__ === Object.prototype); // true

// 객체 리터럴로 생성한 객체는 프로토타입 객체인 prototype의 프로퍼티를 상속 받는다.
console.log(obj.hasOwnProperty('a')); // true -> hasOwnProperty는 prototype 객체의 메서드다.
console.log(obj.hasOwnProperty('__proto__')); // false -> hasOwnProperty 메서드는 객체의 고유 프로퍼티 존재 여부를 알려준다. (상속받은 프로퍼티는 제외)
```

### prototype

- prototype 프로퍼티는 생성자 함수로 호출할 수 있는 함수 객체만 소유하는 프로퍼티다.
    - 즉 CONSTRUCTOR 객체만 소유하는 프로퍼티라고 할 수 있다.

<aside>
💡 prototype 프로퍼티는 생성자 함수가 생성할 인스턴의 프로토타입 객체를 가리킨다.

</aside>