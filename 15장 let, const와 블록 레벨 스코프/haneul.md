# 15. let, const와 블록 레벨 스코프

## var 키워드로 선언한 변수의 문제점

 ES6 전까지 변수를 선언할 수 있는 방법은 var 키워드를 사용하는 것이었다.

```jsx
// var 키워드로 선언한 변수의 특징
console.log(x); // undefined -> 변수 호이스팅

var x = 1;

if (true) {
	var x = 100; // 변수의 중복 선언 허용 + 함수 레벨 스코프
}

```

## let 키워드

```jsx
// let 키워드로 선언한 변수의 특징
console.log(x); // Error : is net defined -> 선언 단계와 초기화 단계 사이의 일시적 사각지대에 있는 x 를 참조할 수 없다.

let x = 1;

let x = 100; // Error : already been declared

if (true) {
	let x = 100; // 블록 레벨 스코프
}

console.log(window.x); // undefined -> let, const 키워드로 선언한 변수는 전역 객체의 프로퍼티가 아니다!
```

## const 키워드

 const 키워드의 특징은 let 키워드와 대부분 동일하다.

```jsx
// const 키워드의 특징
const x; // Error : Missing initializer -> const 키워드로 선언하는 변수는 반드시 선언과 동시에 초기화해야 한다.

console.log(x); // Error : undefined -> 호이스팅이 발생하지 않는 것처럼 동작한다. (일시적 사각지대)

const x = 1;

const x = 2; // Error : already been declared -> 중복 선언 금지!
x = 2; // Error : constant variable. -> 재할당이 금지된다!

{
	const x = 10; // 블록 레벨 스코프를 가진다.
}

const TAX_RATE = 0.1; // 일반적으로 상수의 이름은 대문자로 선언하며 여러 단어로 이뤄진 경우 언더스코어로 구분한다.

const person = {
	name: 'Kim'
};

person.name = 'Lee'; // const 키워드는 재할당을 금지할 뿐 불변이 아니다! -> 객체를 할당한 경우 값을 변경할 수 있다.
```

## var, let, const

- ES6를 사용한다면 var 키워드를 사용하지 않기
- 재할당이 필요한 경우에만 let 키워드를 사용한다. (스코프를 최대한 좁게 만든다.)
- 변경이 발생하지 않는 원시 값과 객체에는 const 키워드를 사용한다.
    - const 키워드는 재할당이 금지되어 var, let 키워드보다 안전하다!