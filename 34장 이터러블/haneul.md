# 이터러블

## 이터레이션 프로토콜

> 순회 가능한 데이터 컬렉션을 만들기 위해 미리 약속한 규칙
> 

### 이터러블

> 이터러블 프로토콜을 준수한 객체를 이터러블이라 한다. 이터러블은 Symbol.iterator를 프로퍼티 키로 사용한 메서드를 직접 구현하거나 상속받은 객체를 말한다.
> 

```jsx
// 이터러블 객체인지 확인하는 함수
const isIterable = v => v !== null && typeof v[Symbol.iterator] === 'function';

// 배열, 문자열, Map, Set 등은 이터러블이다.
isIterable([]); // true
isIterable(''); // true
isIterable(new Map()); // true
isIterable(new Set()); // true

isIterable({}); // false
```

<aside>
💡 이터러블 객체는 for…of 문으로 순회할 수 있으며, 스프레드 문법과 배열 디스트럭처링 할당의 대상으로 사용할 수 있다.

</aside>

- TC39 프로세스의 스프레드 프로퍼티 제안에서 일반 객체에 스프레드 문법의 사용을 허용하고 있다고 한다.

### 이터레이터

> 이터러블의 Symbol.iterator 메서드를 호출하면 이터레이터 프로토콜을 준수한 이터레이터를 반환한다.
> 
- 이터러블의 Symbol.iterator 메서드가 반환한 이터레이터는 next 메서드를 갖는다.

```jsx
const arr = [1, 2, 3];

const iterator = arr[Symbol.iterator](); // 이터레이터 메서드 호출

console.log('next' in iterator); // true -> next 메서드를 갖는다.

console.log(iterator.next()); // { value: 1, done: false }
console.log(iterator.next()); // { value: 2, done: false }
console.log(iterator.next()); // { value: 3, done: false }
console.log(iterator.next()); // { value: undefined, done: true }
```

## 빌트인 이터러블

> Array, String, Map, Set, TypedArray, arguments, DOM 컬렉션 (NodeList, HTMLCollection)
> 

## for…of 문

> 이터러블을 순회하면서 이터러블의 요소를 변수에 할당한다.
> 

```jsx
const arr = [1, 2, 3];

for (const item of arr) {
	console.log(item); // 1 2 3
}
```

## 이터러블과 유사 배열 객체

> 유사 배열 객체는 마치 배열처럼 인덱스로 프로퍼티 값에 접근할 수 있고 length 프로퍼티를 갖는 객체를 말한다.
> 

```jsx
const likeArr = {
	0: 1,
	1: 2,
	2: 3,
	length: 3
};

// 유사 배열 객체는 이터러블이 아닌 일반 객체라서 for..of 문으로 순회할 수 없다.
for (const item of likeArr) {
	console.log(item);
}
// error -> likeArr is not iterable
```

<aside>
💡 arguments, NodeList, HTMLCollection 은 유사 배열 객체이면서 이터러블이다. ES6에서 해당 객체에 Symbol.iterator 메서드를 구현하여 이터러블이 되었다.

</aside>

- Array.from 메서드를 통해 유사 배열 객체 또는 이터러블을 인수로 전달받아 배열로 변환하면 이터러블이 아닌 객체를 배열, 즉 이터러블 객체로 만들 수 있다.

## 이터레이션 프로토콜의 필요성

<aside>
💡 데이터 공급자가 각자의 순회 방식을 가진다면 데이터 소비자는 다양한 데이터 공급자의 순회 방식을 모두 지원해야한다.

</aside>

- 데이터 공급자가 특정 객체의 타입, 데이터 소비자가 객체를 활용하여 동작을 하는 함수라고 볼때
    - 특정 함수는 모든 객체의 타입 즉 모든 데이터 공급자를 지원해야 한다. 이는 비효율적이라는 것.
    - 그러니 데이터 공급자와 데이터 소비자 사이에 인터페이스를 만들어 데이터 소비자가 해당 인터페이스에만 지원한다면 모든 공급자에 적합한 것이니 효율적으로 관리가 가능하다는 것
    - 여기서 그 인터페이스를 이터레이션 프로토콜이 담당하고 있다는 것이다.

## 사용자 정의 이터러블

> 이터레이션 프로토콜을 준수하지 않는 일반 객체도 이터레이션 프로토콜을 준수하도록 구현하면 사용자 정의 이터러블이 된다.
> 

### 사용자 정의 이터러블 구현

```jsx
// 피보나치 수열을 구현한 간단한 사용자 정의 이터러블을 구현해보자
const fibonacci = {
	[Symbol.iterator]() {
		let [pre, cur] = [0, 1]; // 디스트럭처링 할당 (pre 에 0, cur 에 1)
		const max = 10; // 수열의 최대값

		//Symbol.iterator 메서드는 next 메서드를 소유한 이터레이터를 반환해야함 (next 메서드는 iterator result object를 반환)
		return {
			next() {
				[pre, cur] = [cur, pre + cur];
				return { value: cur, done: cur >= max };
			}
		};
	}
};

// 이터러블인 피보나치 객체를 순회할 때마다 next 메서드가 호출됨
for (const num of fibonacci) {
	console.log(num); // 1 2 3 5 8
}

const arr = [...fibonacci]; // 스프레드 문법의 대상이 될 수 있다.
const [first, second, ...rest] = fibonacci; // 디스트럭처링 할당의 대상이 될 수 있다.
```

### 이터러블을 생성하는 함수

> 앞서 구현한 피보나치 이터러블 내부에는 수열의 최대값인 max를 가지고 있었다. → 내부에서 고정된 값으로 외부에서 변경할 방법이 없다 → 수정해보자
> 

```jsx
const fibonacciFunc = function (max) {
	let [pre, cur] = [0, 1];
	
	return {
		[Symbol.iterator]() {
			return {
				next() {
					[pre, cur] = [cur, pre + cur];
					return { value: cur, done: cur >= max };
				}
			};
		}
	};
};

for (const num of fibonacciFunc(10)) {
	console.log(num); // 1 2 3 5 8
}
```

### 이터러블이면서 이터레이터인 객체를 생성하는 함수

```jsx
const fibonacciFunc = function (max) {
	let [pre, cur] = [0, 1];
	
	return {
		[Symbol.iterator]() {
			return {
				next() {
					[pre, cur] = [cur, pre + cur];
					return { value: cur, done: cur >= max };
				}
			};
		}
	};
};

// fibonacciFunc 함수는 이터러블을 반환한다.
const iterable = fibonacciFunc(10);
// 이터러블의 Symbol.iterator 메서드는 이터레이터를 반환한다.
const iterator = iterable[Symbol.iterator]();

console.log(iterator.next()); // { value: 1, done: false }
console.log(iterator.next()); // { value: 2, done: false }
console.log(iterator.next()); // { value: 3, done: false }
console.log(iterator.next()); // { value: 5, done: false }
console.log(iterator.next()); // { value: 9, done: false }
```

> 이때 이터러블이면서 이터레이터인 객체를 생성하면 iterator 메서드를 호출하지 않아도 된다.
> 

```jsx
// 이터러블이면서 이터레이터인 객체
/*
{
	[Symbol.iterator]() { return this; },
	next() {
		return { value: any, done: boolean };
	}
}
*/

const fibonacciFunc = function (max) {
	let [pre, cur] = [0, 1];
	
	return {
		[Symbol.iterator]() { return this; },
		next() {
			[pre, cur] = [cur, pre + cur];
			return { value: cur, done: cur >= max };
		}
	};
};

// iter는 이터러블이면서 이터레이터다.
let iter = fibonacciFunc(10);

for (const num of iter) {
	console.log(num); // 1 2 3 5 8
}

iter = fibinacciFunc(10); // 해당 코드 주석 시 { value: 21, done: true }
console.log(iter.next()); // { value: 1, done: false }
```

### 무한 이터러블과 지연 평가

```jsx
const fibonacciFunc = function() {
	let [pre, cur] = [0, 1];

	return {
		[Symbol.iterator]() { return this; },
		next() {
			[pre, cur] = [cur, pre + cur];
			return { value: cur }; // done 프로퍼티가 없어 무한 이터러블이 된다.
		}
	};
};

for (const num of fibonacciFunc()) {
	if (num > 10000) break;
	console.log(num);
}
```

<aside>
💡 이터러블은 지연 평가를 통해 데이터를 생성한다.

</aside>

- 지연 평가는 데이터가 필요한 시점 이전까지는 미리 데이터를 생성하지 않다가 데이터가 필요한 시점이 되면 비로소 데이터를 생성하는 기법이다.
    - 위 예제에서 fibonacciFunc 함수는 무한 이터러블을 생성하지만 이를 사용하는 데이터 소비자, 즉 for…of 문 등이 실행되기 이전까지 데이터를 생성하지 않는다.