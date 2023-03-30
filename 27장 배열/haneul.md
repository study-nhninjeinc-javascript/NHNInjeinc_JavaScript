# 배열

> 여러 개의 값을 순차적으로 나열한 자료구조
> 

 const arr = [’apple’ , ‘banana’ , ‘orange’];

- 배열이 가지고 있는 값을 요소라고 부른다.
    - 자바스크립트의 모든 값은 배열의 요소가 될 수 있다. → 원시값, 객체, 함수, 배열 등
- 배열의 요소는 자신의 위치를 나타내는 0 이상의 정수인 인덱스를 갖는다. → 요소 접근 시 사용 (대부분 0부터 시작)
- 배열은 요소의 개수, 즉 길이를 나타내는 length 프로퍼티를 갖는다.

<aside>
💡 자바스크립트에 배열이라는 타입은 존재하지 않는다. 배열은 객체 타입이다.

</aside>

### 일반 객체와 구별되는 배열의 특징

- 배열의 구조는 인덱스와 요소로 구성되며 값을 참조할 때 인덱스를 활용한다.
- 배열의 요소, 즉 값에는 순서가 존재하며 길이를 나타내는 length 프로퍼티를 가진다.

```jsx
const arr = [1, 2, 3];

for (let i = 0; i < arr.length; i++) {
	console.log(arr[i]);
}
```

## 자바스크립트의 배열은 배열이 아니다?

> 자료구조에서 말하는 배열은 동일한 크기의 메모리 공간이 빈틈없이 연속적으로 나열된 구조를 말한다. → 밀집 배열
> 
- 배열의 요소가 메모리 주소 1000 에서 시작하며 각 요소가 8바이트의 메모리 공간을 가진다고 할때
    - 인덱스가 0인 요소의 메모리 주소 = 1000 (1000 + 0 * 8)
    - 인덱스가 1인 요소의 메모리 주소 = 1008 (1000 + 1 * 8)
    - 인덱스가 2인 요소의 메모리 주소 = 1000 (1016 + 2 * 8)
- 배열은 인덱스를 통해 효율적으로 요소에 접근할 수 있지만 정렬되지 않은 배열에서 특정한 요소를 검색하는 경우 선형 검색을 해야한다.
- 배열에 요소를 삽입하거나 삭제하는 경우 배열의 요소를 연속적으로 유지하기 위해 요소를 이동시켜야 하는 단점이 있다.

<aside>
💡 자바스크립트의 배열은 요소를 위한 각 메모리 공간이 동일한 크기를 갖지 않을 수 있으며, 연속적으로 이어져 있지 않을 수도 있다. (희소 배열)

</aside>

```jsx
const arr = [1, 2, 3];

console.log(Object.getOwnPropertyDescriptors(arr));
/*
0: {value: 1, writable: true, enumerable: true, configurable: true}
1: {value: 2, writable: true, enumerable: true, configurable: true}
2: {value: 3, writable: true, enumerable: true, configurable: true}
length: {value: 3, writable: true, enumerable: false, configurable: false}
[[Prototype]]: Object
*/
```

### 일반적인 배열과 자바스크립트 배열의 장단점

- 일반적인 배열은 인덱스로 요소에 빠르게 접근 가능하지만, 요소를 삽입 또는 삭제하는 경우 효율적이지 않다.
- 자바스크립트 배열은 해시 테이블로 구현된 객체이므로 인덱스로 요소에 접근하는 경우 일반적인 배열보다 성능적인 면에서 느릴 수밖에 없는 구조적 단점이 있다.
    - 하지만 요소를 삽입 또는 삭제하는 경우 일반적인 배열보다 빠른 성능을 기대할 수 있다.

<aside>
💡 자바스크립트 배열에서 구조적 단점을 보완하기 위해 대부분 모던 자바스크립트 엔진에서 배열을 일반 객체와 다르게 일반적인 배열처럼 동작하도록 구현했다고 한다.

</aside>

```jsx
// 배열
const arr = [];

console.time('Array 1,000,000 Iter Test START');
for (let i = 0; i < 1000000; i++) {
	arr[i] = i;
}
console.timeEnd('Array 1,000,000 Iter Test END');
// 14ms

// 객체
const obj = {};

console.time('Object 1,000,000 Iter Test START');
for (let i = 0; i < 1000000; i++) {
	obj[i] = i;
}
console.timeEnd('Object 1,000,000 Iter Test END');
// 22ms
```

## length 프로퍼티와 희소 배열

> length 프로퍼티는 요소의 개수, 즉 배열의 길이를 나타내는 0이상 2^32 - 1미만의 양의 정수 값을 가진다. (40억 정도?)
> 

```jsx
// length 프로퍼티 값은 배열에 요소를 추가하거나 삭제하면 자동 갱신된다.
const arr = [1, 2, 3];
console.log(arr.length); // 3

// 요소 추가 (push)
arr.push(4);
console.log(arr.length); // 4

// 요소 삭제 (pop) -> 마지막 요소를 삭제한다.
arr.pop();
console.log(arr.length); // 3

// length 프로퍼티의 값을 임의의 숫자 값으로 할당할 수도 있다.
arr.length = 2;
console.log(arr.length); // 2
console.log(arr); // [1, 2] -> 실제 배열의 길이가 줄어든다.

arr.length = 3;
console.log(arr.length); // 3
console.log(arr); // [1, 2, empty]
// 여기서 empty 요소는 실제로 추가된 배열의 요소가 아니다. 즉 arr[2] 에는 값이 존재하지 않는다.
console.log(arr[2]); // undefined
```

```jsx
const sparse = [, 2, , 4]; // 희소 배열

console.log(sparse.length); // 4 -> 희소 배열의 length 프로퍼티 값은 요소의 개수와 일치하지 않는다.
console.log(sparse); // [empty, 2, empty, 4]

console.log(Object.getOwnPropertyDescriptors(sparse));
/*
인덱스가 0, 2 인 요소가 존재하지 않는다.
1: {value: 2, writable: true, enumerable: true, configurable: true}
3: {value: 4, writable: true, enumerable: true, configurable: true}
length: {value: 4, writable: true, enumerable: false, configurable: false}
[[Prototype]]: Object
*/
```

<aside>
💡 배열을 생성할 경우에는 희소 배열을 생성하지 않도록 주의하자. 배열에는 같은 타입의 요소를 연속적으로 위치시키는 것이 바람직하다.

</aside>

## 배열 생성

### 배열 리터럴

```jsx
const arr = [1, 2, 3]; // 배열 리터럴

const binArr = []; // 배열 리터럴에 요소를 하나도 추가하지 않으면 길이가 0인 배열이 된다.
console.log(binArr.length); // 0

const sparseArr = [1, , 3]; // 배열 리터럴에 요소를 생략하면 희소 배열이 생성된다.
console.log(sparseArr.length); // 3 -> 실제 요소 개수보다 크다. 실제는 2
```

### Array 생성자 함수

```jsx
const arr = new Array(10); // 전달된 인수가 1개이고 숫자인 경우 length 프로퍼티 값이 인수인 배열을 생성
// 이때 전달하는 인수가 0 ~ 2^32 - 1 사이의 값이 아닌 정수인 경우 RangeError 발생한다.

console.log(arr); // [empty * 10]

 const binArr = new Array(); // 전달된 인수가 없는 경우 빈 배열을 생성한다.

// 전달된 인수가 2개 이상이거나 숫자가 아닌 경우 인수를 요소로 갖는 배열을 생성
const newArr = new Array(1, 2, 3);
console.log(newArr); // [1, 2, 3]

const strArr = new Array('STRING');
console.log(strArr); // ['STRING']

// Array 생성자 함수는 new 연산자와 함께 호출하지 않더라도 배열을 생성하는 생성자 함수로 동작한다. (생성자 함수 내부에서 new.target 을 확인한다.)
const funcArr = Array(1, 2, 3);
console.log(funcArr); // [1, 2, 3]
```

```jsx
// new.target 을 활용해 생성자 함수가 new 연산자 없이 호출하더라도 생성자함수와 동일하게 동작
function CreationFunc() {
	// new 연선자 없이 호출하면 new 연산자를 통해 재귀호출하여 return
	if (!new.target) {
		return new CreationFunc();
	}

	// 인스턴스의 프로퍼티 초기화
	this.a = 'a';
	this.b = 'b';
}

const a = CreationFunc();
console.log(a);
```

### Array.of

```jsx
// ES6 에서 도입된 Array.of 메서드는 전달된 인수를 요소로 갖는 배열을 생성한다. (숫자 타입의 인수 1개만 전달하더라도 해당인수를 요소로 갖게끔)
Array.of(1); // [1]
```

### Array.from

```jsx
// ES6에서 도입된 Array.from 메서드는 유사 배열 객체 또는 이터러블 객체를 인수로 전달받아 배열로 변환하여 반환한다.
Array.from({
	length: 2,
	0: 'a',
	1: 'b'
});
// ['a', 'b']

Array.from('Hello'); // ['H', 'e', 'l', 'l', 'o']
// 문자열은 이터러블이기 때문에 이를 배열로 변환한다.

Array.from({ length: 3 }); // [undefined, undefined, undefined]

// 두 번째 인수로 전달한 콜백 함수의 반환값으로 구성된 배열을 반환한다.
Array.from({ length: 3 }, (_, i) => i); // [0, 1, 2]
```

## 배열 요소의 참조

```jsx
const arr = [1, 2, 3];

// 대괄호 표기법을 통해 인덱스가 0인 요소를 참조
console.log(arr[0]); // 1

// 존재하지 않는 요소에 접근 시 undefined
console.log(arr[3]); // undefined
```

<aside>
💡 배열은 인덱스를 나타내는 문자열을 프로퍼티 키로 갖는 객체다.

</aside>

## 배열 요소의 추가와 갱신

```jsx
const arr = [0, 1];

arr[10] = 11;

console.log(arr.length); // 11
console.log(arr); // [0, 1, empty * 8, 11]
```

```jsx
const arr = [];

arr[0] = 0; // 첫 번째 인덱스에 값 추가
arr[0] = 1; // 첫 번째 인덱스의 값 갱신

arr['0'] = '0'; // 문자열이 정수 형태라면 인덱스 값으로 취급 -> 값이 추가 or 갱신된다.
arr['pp'] = 'i am not element'; // 정수가 아닌 값을 인덱스 처럼 사용하면 프로퍼티가 생성된다. (length에 영향 x)

console.log(arr.length); // 1
console.log(arr); // ['0', pp: 'i am not elemnt']
```

## 배열 요소의 삭제

```jsx
// 배열도 객체이기 때문에 delete 연산자를 사용할 수 있다.
const arr = [1, 2, 3];

delete arr[0];
console.log(arr); // [empty, 2, 3]

delete arr[2];
console.log(arr); // [empty, 2, empty] -> 마지막 요소를 삭제해도 length에 영향이 없다. -> 희소 배열이 된다.
```

```jsx
// 희소 배열을 만들지 않으면서 배열의 특정 요소를 완전히 삭제하려면 Array.prototype.splice 메서드를 사용해야 한다.
const arr = [1, 2, 3];

// (삭제를 시작할 인덱스 값, 삭제할 요소의 개수)
arr.splice(1, 1); // 인덱스 1번부터 1개 삭제
console.log(arr); // [1, 3]
console.log(arr.length); // 2 -> 갱신된다.
```

## 배열 메서드

> 배열에는 원본 배열을 직접 변경하는 메서드와 원본 배열을 변경하지 않고 새로운 배열을 생성하여 반환하는 메서드가 있다.
> 

<aside>
💡 주로 ES5부터 도입된 배열 메서드는 배열을 직접 변경하지 않지만 초창기 배열 메서드는 원본 배열을 직접 변경하는 경우가 많다. → 변경하지 않는 걸 쓰는게 좋다.

</aside>

### Array.isArray

> Array 생성자 함수의 정적 메서드로 전달된 인수가 배열이면 true 아니면 false를 반환한다.
> 

```jsx
// true
Array.isArray([]);
Array.isArray([1, 2]);
Array.isArray(new Array());
Array.isArray(Array());

// false
Array.isArray();
Array.isArray({});
Array.isArray({ 0: '1', length: 1});
```

### Array.prototype.indexOf

> 원본 배열에서 인수로 전달된 연소를 섬색하여 인덱스를 반환
> 

```jsx
const arr = ['a', 'b', 'b' ,'c', 'd'];

// 배열에서 요소 'b'를 검색해서 첫 번째로 검색된 요소의 인덱스를 반환해준다.
arr.indexOf('b'); // 1

// 인수로 전달한 요소가 존재하지 않으면 -1을 반환한다.
arr.indexOf('e'); // -1

// 두 번재 인수로 검색을 시작할 인덱스를 지정할 수 있다.
arr.indexOf('b', 2); // 2 -> 1 과 2의 인덱스에 'b' 요소가 있고 인덱스 2 부터 시작하여 검색하니 2가 나온다.

// 배열에 특정 요소가 존재하는지 확인할 때 주로 사용
if (arr.indexOf('e') === -1) {
	arr.push('e'); // 배열에 요소가 없다면 해당 요소를 추가한다.
}
console.log(arr); // ['a', 'b', 'b', 'c', 'd', 'e']

// ES7에서 도입된 Array.prototype.includes 메서드를 사용하면 가독성이 더 좋다.
if (!arr.includes('f') {
	arr.push('f');
}
console.log(arr); // ['a', 'b', 'b', 'c', 'd', 'e', 'f']
```

### Array.prototype.push

> 인수로 전달받은 모든 값을 원본 배열의 마지막 요소로 추가하고 변경된 length 프로퍼티 값을 반환한다. → 원본 배열을 직접 변경
> 

```jsx
const arr = ['a', 'b'];

let result = arr.push('c', 'd');
console.log(result); // 4 (length 값)

console.log(arr); // ['a', 'b', 'c', 'd']
```

<aside>
💡 push 메서드는 성능 면에서 좋지 않다.

</aside>

```jsx
// 추가하려는 요소가 하나라면 length 프로퍼티를 사용해 배열의 마지막 요소에 직접 추가할 수 있다.
const arr = ['a', 'b'];

arr[arr.length] = 'c'; // push 메서드보다 빠르다고 한다.
// 희소 배열인 경우 예상지 못한 결과가 나올 것 같다. 희소 배열을 쓰지 않도록 조심하자.

// 혹은 ES6 스프레드 문법을 사용하는 방법이 있다. -> 요 방법을 추천하는 듯 하다.
const newArr = [...arr, 'd', 'e'];
console.log(arr); // ['a', 'b', 'c']
console.log(newArr); // ['a', 'b', 'c', 'd', 'e']
```

### Array.prototype.pop

> 원본 배열에서 마지막 요소를 제거하고 제거한 요소를 반환한다. (빈 배열이면 undefined를 반환한다.) → 원본 배열을 직접 변경
> 

```jsx
const arr = ['a', 'b', 'c'];

let result = arr.pop();
console.log(result); // 'c'

console.log(arr); // ['a', 'b']
```

```jsx
// 스택을 생성자 함수로 구현해보자
const Stack = (function() {
	function Stack(array = []) {
		if (!Array.isArray(array)) {
			throw new TypeError(`$array} is not an array.`);
		}
		this.array = array;
	}

	Stack.prototype = {
		constructor: Stack,
		push(value) {
			this.array[this.array.length] = value;
			return this.array.length;
		},
		pop() {
			return this.array.pop();
		},
		entries() {
			return [...this.array];
		}
	};

	return Stack;
}());

const stack = new Stack([1, 2]);
console.log(stack.entries()); // [1, 2]

stack.push(3);
console.log(stack.entries()); // [1, 2, 3]

stack.pop();
console.log(stack.entries()); // [1, 2]
```

### Array.prototype.unshift

> 인수로 전달받은 모든 값을 원본 배열의 선두에 요소로 추가하고 변경된 length 프로퍼티 값을 반환한다. → 원본 배열을 직접 변경
> 

```jsx
const arr = ['c', 'd'];

let result = arr.unshift('a', 'b');
console.log(result); // 4 -> length

console.log(arr); // ['a', 'b', 'c', 'd']
```

```jsx
const arr = ['c', 'd'];

const newArr = ['a', 'b', ...arrr];
console.log(newArr); // ['a', 'b', 'c', 'd']
```

### Array.prototype.shift

> 원본 배열에서 첫 번째 요스를 제거하고 제거한 요소를 반환한다. 빈 배열이면 undefined 반환 → 원본 배열을 직접 변경
> 

```jsx
const arr = ['a', 'b'];

let result = arr.shift();
console.log(result); // 'a'

console.log(arr); // ['b']
```

<aside>
💡 shift 메서드와 push 메서드(length 프로퍼티나 스프레드 문법 쓰는게 좋지 않을까 싶다.) 를 사용하면 큐를 쉽게 구현할 수 있다.

</aside>

### Array.prototype.concat

> 인수로 전달된 값들을 원본 배열의 마지막 요소로 추가한 새로운 배열을 반환한다. → 원본 배열은 변경되지 않는다.
> 

```jsx
const arr1 = ['a', 'b'];
const arr2 = ['c', 'd'];

let result = arr1.concat(arr2); // 배열을 인수로 전달하면 배열을 해체해서 새로운 배열의 요소로 추가한다.
console.log(result); // ['a', 'b', 'c', 'd']
```

<aside>
💡 push와 unshift 메서드를 concat으로 대체할 수 있고 concat 메서드는 스프레드 문법으로 대체할 수 있다. 가능하면 한 가지만 일관성 있게 사용하라는 듯하다..

</aside>

### Array.prototype.splice

> 원본 배열의 중간에 요소를 추가하거나 삭제하는 메서드.. → 원본 배열을 직접 변경한다.
> 

```jsx
const arr = ['a', 'b', 'c', 'd', 'e'];

// 인덱스 1부터 2개의 요소를 제거하고 그 자리에 새로운 요소 20, 30을 삽입..
const result = arr.splice(1, 2);

console.log(result); // ['b', 'c'] -> 제거된 요소가 배열로 반환된다.

console.log(arr); // ['a', 'bb', 'cc', d', 'e']
```

- 두 번째 인수(제거할 요소 개수)를 0으로 지정하면 제거하지 않고 다음 인수들을 삽입한다.
- 세 번째 이후의 인수들(추가할 요소들)을 전달하지 않으면 추가는 하지 않고 제거만한다.
- 첫 번째 인수만 전달할 경우 해당 인덱스 부터 뒤에 모든 요소를 제거한다.

### Array.prototype.slice

> 인수로 전달된 범위의 요소들을 복사하여 배열로 반환한다. → 원본 배열 변경 안됨
> 

```jsx
const arr = ['a', 'b', 'c', 'd'];

// 인덱스  0부터 인덱스 2까지 복사 (인덱스 2는 미포함이다.)
const newArr = arr.slice(0, 2);
console.log(newArr); // ['a', 'b']

// 두 번재 인수를 생략하면 첫 번째 인수에 해당하는 인덱스 이후 모든 요소를 복사한다.
const newNewArr = arr.slice(1);
console.log(newNewArr); // ['b', 'c', 'd']

// 첫 번째 인수가 음수인 경우 배열의 끝에서부터 요소를 복사한다.
const backArr = arr.slice(-1);
console.log(backArr); // ['d']

// 인수를 생략하면 얕은 복사된 배열을 반환한다.
const copyArr = arr.slice();
console.log(copyArr === arr); // false
```

<aside>
💡 slice 메서드 또는 from 메서드를 통해 유사 배열 객체를 배열로 변환할 수 있다. 스프레드 문법으로도 된다고 한다.

</aside>

### Array.prototype.join

> 원본 배열의 모든 요소를 문자열로 변환 후, 인수로 전달받은 문자열(구분자)로 연결한 문자열을 반환한다.
> 

```jsx
const arr = [1, 2, 3, 4, 5];

console.log(arr.join()); // '1,2,3,4,5'

console.log(arr.join(''); // '12345'

console.log(arr.join(', '); // '1, 2, 3, 4, 5'
```

### Array.prototype.reverse

> 원본 배열의 순서를 반대로 뒤집는다. → 원본 배열이 변경된다.
> 

```jsx
const arr = ['a' ,'b', 'c'];

const result = arr.reverse();

console.log(result); // ['c', 'b', 'a']
console.log(arr); // ['c', 'b', 'a']
```

### Array.prototype.fill

> 인수로 전달받은 값을 배열의 처음부터 끝까지 요소로 채운다. → 원본 배열이 변경된다.
> 

```jsx
const arr = ['a', 'b', 'c'];

arr.fill('hi');

console.log(arr); // ['hi', 'hi', 'hi]
```

### Array.prototype.includes

> 배열 내에 특정 요소가 포함되어 있는지 확인하여 true 또는 false 를 반환한다.
> 

```jsx
const arr = ['a', 'b', 'c'];

arr.includes('a'); // true
arr.includes(1); // false

arr.includes('a', 1); // false -> 인덱스 1 부터 시작해서 검색
arr.includes('c', -1); // true -> length - 1 부터 확인
```

### Array.prototype.flat

> 인수로 전달한 깊이만큼 재귀적으로 배열을 평탄화 한다. → 원본 배열을 변경하지 않고 평탄화한 배열을 반환해준다.
> 

```jsx
const arr = [1, [2, [3, [4, [5]]]]];

arr.flat();
console.log(arr); // [1, [2, [3, [4, [5]]]]] -> 원본 배열을 변경하지 않는다.

console.log(arr.flat()); // [1, 2, [3, [4, [5]]]] -> 인수를 전달하지 않으면 기본값은 1이다.

console.log(arr.flat(Infinity)); // [1, 2, 3, 4, 5] -> 무한대를 인수로 전달하면 전부 평탄화한다.
```

## 배열 고차 함수

> 고차 함수는 함수를 인수로 전달받거나 함수를 반환하는 함수를 말한다. → 외부 상태의 변경이나 가번 데이터를 피하고 불변성을 지향
> 

<aside>
💡 함수형 프로그래밍은 순수 함수와 보조 함수의 조합을 통해 조건문과 반복문을 제거하여 복잡성을 해결하고 변수의 사용을 억제하여 상태 변경을 피하려는 프로그래밍 패러다임이다.

</aside>

### Array.prototype.sort

> 배열의 요소를 정렬한다.(기본적으로 오름차순) → 원본 배열을 직접 변경
> 

```jsx
const arr = ['Banana', 'Orange', 'Apple'];

// 오름차순 정렬
arr.sort();
console.log(arr); // ['Apple', 'Banana', 'Orange']

const 배열 = ['바나나', '오렌지', '사과'];

배열.sort();
배열.reverse(); // 내림차순 정렬
console.log(배열); // ['오렌지', '사과' ,'바나나']

const numArr = [1, 5, 25, 2, 10];

numArr.sort();
console.log(numArr); // [1, 10, 2, 5, 25, 5] ?????
// 숫자 요소로 이루어진 배열을 정렬할 때는 의도한 대로 정렬되지 않는다.
// sort 메서드는 유니코드 코드 포인트에 따라 정렬하는데 이 때 숫자는 문자열로 변환 후 유니코드 코드 포인트를 비교한다.
// 따라서 숫자 요소를 정렬할 때는 sort 메서드에 정렬 순서를 정의하는 비교 함수를 인수로 전달해야 한다.

numArr.sort((a, b) => a - b);
// 비교 함수는 양수나 음수 또는 0을 반환해야하며 비교 함수의 반환값이 0보다 작으면 첫번째 인수를 우선하여 정렬. 0이면 정렬하지 않고 0보다 크면 두 번째 인수를 우선으로 정렬한다.
console.log(numArr); // [1, 2, 5, 10, 25]

// 객체를 요소로 갖는 정렬인 경우
const objArr = [
	{ id: 4, content: 'JavaScript' },
	{ id: 1, content: 'HTML' },
	{ id: 2, content: 'CSS' }
];

function compareObj(key) {
	// 프로퍼티 값이 문자열인 경우 산술 연산으로 비교 시 NaN이 나오므로 비교 연산을 사용
	return (a, b) => (a[key] > b[key] ? 1 : (a[key] < b[key] ? -1 : 0));
}

objArr.sort(compareObj('id')); // id 키 값을 기준으로 정렬하겠다는 코드이다..
console.log(objArr);
/*
[
	{ id: 1, content: 'HTML' },
	{ id: 2, content: 'CSS' },
	{ id: 4, content: 'JavaScript' }
]
*/
```

### Array.prototype.forEach

> for 문을 대체할 수 있는 고차 함수로, 자신의 내부에서 반복문을 실행한다. (반복문을 추상화한 고차 함수)
> 

```jsx
const numArr = [1, 2, 3, 4, 5];
const resultArr = [];

// 배열의 모든 요소를 순회하면서 콜백 함수를 반복 호출한다.
// 이때 item 은 요소의 값
numArr.forEach(item => resultArr.push(item ** 2));
console.log(resultArr); // [1, 4, 9, 16, 25]

// 총 3개의 인수를 전달할 수 있다. (요소값, 인덱스, this)
numArr.forEach((item, index, arr) => {
	console.log(`요소값 : ${item} , 인덱스 : ${index} , this : ${JSON.stringify(arr)}`);
});
// JSON.stringify 메서드는 객체를 JSON 포맷의 문자열로 변환한다.
```

<aside>
💡 forEach 메서드 자체가 원본 배열을 변경하진 않지만 콜백 함수를 통해 원본 배열을 변경할 수 있다. (메서드의 반환값은 언제나 undefined)

</aside>

```jsx
// forEach 메서드의 폴리필 (최신 사양 기능을 지원하지 않아 forEach 메서드를 사용할 수 없는 경우 구현하여 사용할 수 있다.)
if (!Array.prototype.forEach) {
	Array.prototype.forEach = function (callback, thiArg) {
		if (typeof callback !== 'function') {
			throw new TypeError(callback + ' is not a function');
		}

		thisArg = thisArg || window;

		for (var i = 0; i < this.length; i++) {
			callback.call(thisArg, this[i], i, this);
		}
	};
}
```

<aside>
💡 forEach 메서드 내에서는 break, continue 문을 사용할 수 없다.

</aside>

- 존재하지 않는 요소는 순회 대상에서 제외 (배열을 순회하는 다른 메서드에서도 동일함)
- forEach 메서드는 for문보다 성능이 좋진 않지만 가독성이 좋다. (어차피 메서드 내에서 for 문을 도니까 좋을수가 없긴 하겠다..)

### Array.prototype.map

> 자신을 호출한 배열의 모든 요소를 순회하면서 인수로 전달받은 콜백 함수를 반복 호출한다. 이후 콜백 함수의 반환값들로 구성된 새로울 배열을 반환한다.
> 

```jsx
const numArr = [1, 4, 9];

const result = numArr.map(item => Math.sqrt(item));

console.log(result); // [1, 2, 3]
console.log(numArr); // [1, 4, 9] -> 원본 배열이 변경되지 않는다.
```

<aside>
💡 map 메서드가 반환하는 배열의 길이는 map 메서드를 호출한 배열의 길이와 반드시 일치한다. (1 : 1 매핑)

</aside>

- forEach 문과 마찬가지로 this를 전달받을 수 있다.

### Array.prototype.filter

> 자신을 호출한 배열의 모든 요소를 순회하면서 인수로 전달받은 콜백 함수를 반복 호출하며, 콜백 함수의 반환값이 true인 요소로만 구성된 배열을 반환한다.
> 

```jsx
const numArr = [1, 2, 3, 4, 5, 6, 7, 8, 9];

const result = numArr.filter(item => item % 2); // 요소를 2로 나눈 나머지 값이 true 즉 0이 아닌 값만 가져오게 된다. > 홀수만 가져옴
console.log(result); // [1, 3, 5, 7, 9]
console.log(numArr); // [1, 2, 3, 4, 5, 6, 7, 8, 9] -> 원본 배열을 변경하지 않는다.
```

- this 를 전달받을 수 있다.
- 특정 요소를 제거하기 위해 사용할 수도 있으나 특정 요소가 중복되어 있다면 중복된 요소가 모두 제거된다. → indexOf 메서드로 인덱스 얻어서 splice 사용하면 해결

### Array.prototype.reduce

> 배열의 모든 요소를 순회하며 인수로 전달받은 콜백 함수를 반복 호출한다. 이 때 콜백 함수의 반확값을 다음 순회 시에 콜백 함수의 첫 번째 인수로 전달하며 하나의 결과값을 만들어 반환한다.
> 

```jsx
const numArr = [1, 2, 3, 4, 5];

const result = numArr.reduce((accumulator, currentValue, index, array) => accumulator + currentValue, 0);

//화살표 함수로 보니까 오히려 모르겠다. 그냥 함수로 써보자
const newResult = numArr.reduce(function(acc, curr, idx, arr) {
	// 콜백 함수의 첫 번째 인수는 이전 콜백 함수의 반환값 ( 첫 번째 콜백 함수 호출이라면 reduce의 두 번째 인수 값이 된다 )
	// 즉 이 콜백 함수가 첫 번째 요소를 대상으로 한다면 acc 값은 0이 된다.
	
	return acc + curr;
}, 0); // reduce 메서드의 2번째 인수는 이후에 콜백 함수 호출 시 보낼 값의 초기 값을 의미한다.

console.log(result); // 15
console.log(newResult); // 15

// 즉 reduce 메서드의 첫번째 인수는 콜백함수, 두번째 인수는 초기값이다.
// 콜백 함수의 첫번째 인수는 이전 콜백 함수의 반환값, 두번째 인수는 현재요소값, 세번째 인수는 인덱스, 네번째 인수는 reduce 메서드를 호출한 객체가 전달된다.
```

<aside>
💡 reduce 메서드를 통해 평균, 최대값, 중복 횟수, 평탄화, 중복 요소 제거 등 여러 활용이 가능하다..

</aside>

### Array.prototype.some

> 배열의 요소를 순회하면서 인수로 전달된 콜백 함수를 호출한다. 이때 some 메서드는 콜백 함수의 반환값이 단 한번이라도 참이면 true, 모두 거짓인 경우에 false를 반환한다.
> 

```jsx
const arr = [1, 2, 3, 4, 5, 6, 7, 8];

arr.some(item => item > 10); // -> false
arr.some(item => item === 1); // -> true
```

### Array.prototype.every

> 배열의 요소를 순회하면서 인수로 전달된 콜백함수를 호출 → 콜백 함수의 반환값이 모두 참이면 true, 단 한번이라도 거짓이면 false 반환 (빈 배열인 경우 true)
> 

```jsx
const arr = [1, 2, 3, 4, 5, 6, 7];

arr.every(item => item > 3); // -> false
arr.every(item => typeof item === 'number'); // -> true
[].every(item => item === 1); // true
```

### Array.prototype.find

> 배열의 요소를 순회하면서 인수로 전달된 콜백 함수를 호출하여 반환값이 true인 첫 번째 요소를 반환한다. (true인 요소가 없다면 undefined)
> 

```jsx
const arr = [
	{ id: 1, name: 'Lee' },
	{ id: 2, name: 'Kim' },
	{ id: 3, name: 'Park' },
	{ id: 4, name: 'Choi' },
	{ id: 5, name: 'Lim' }
];

arr.find(item => item.id === 2); // { id: 2, name: 'Kim' }
```

### Array.prototype.findIndex

> 배열의 요소를 순회하면서 인수로 전달된 콜백 함수를 호출하여 반환값이 true 인 첫 번째 요소의 인덱스를 반환한다.
> 

```jsx
const arr = [
	{ id: 1, name: 'Lee' },
	{ id: 2, name: 'Kim' },
	{ id: 3, name: 'Park' },
	{ id: 4, name: 'Choi' },
	{ id: 5, name: 'Lim' }
];

arr.findIndex(item => item.id === 2); // 1
```

### Array.prototype.flatMap

> map 메서드를 통해 생성된 새로운 배열을 평탄화하는 메서드..
> 

```jsx
const arr = ['hello', 'universe'];

arr.map(x => x.split('')).flat();
// ['h', 'e' ,'l', 'l', 'o', 'u', 'n', 'i', 'v', 'e', 'r', 's', 'e']

arr.flatMap(x => x.split('');
// ['h', 'e' ,'l', 'l', 'o', 'u', 'n', 'i', 'v', 'e', 'r', 's', 'e']
```

<aside>
💡 flatMap 메서드는 1단계 평탄화만 한다.

</aside>