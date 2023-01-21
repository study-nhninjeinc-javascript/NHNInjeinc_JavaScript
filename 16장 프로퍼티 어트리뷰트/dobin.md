# 16장 프로퍼티 어트리뷰트

### 내부 슬롯과 내부 메서드

- 자바스크립트 엔진의 구현 알고리즘을 설명하기 위해 ECMAScript 사양에서 사용하는 의사 프로퍼티와 의사 메서드이다.zz
- 자바스크립트 엔진에서 실제로 동작하지만 개발자가 직접 접근할 수 있도록 외부로 공개된 객체의 프로퍼티는 아니다.
- 단, 일부 내부 슬롯과 내부 메서드는 간접적으로 접근할 수 있는 수단을 제공한다.
- 모든 객체는 [[Prototype]]이라는 내부 슬롯을 갖는다. __**proto**__를 통해 간접적으로 접근할 수 있다.

<aside>
💡 내부 메서드

</aside>

- 내부 메서드는 엔진 내부에 정의되어 있는 함수?

<aside>
💡 내부 슬롯

</aside>

내부 슬롯은 의사 프로퍼티라고 했다. ‘의사’란 프로그래밍 언어의 문법에 따라 쓰인 것이 아니라 일반적인 언어로 논리를 표현한 것이고, 프로퍼티란 쉽게 말해 `**`이름 : 값` 쌍의 실체**`로서, 동적으로 조작 가능함을 말한다. 그리고 슬롯은 슬롯안에 어떠한 값이 있느냐 없느냐에 따라 확보되는 것이 아닌 항상 확보되어 있는것이 슬롯이다. 이것들을 하나로 이어보면 내부 슬롯은 필요에따라 특정 어느 위치에서 존재하며 내부 슬롯은 프로퍼티이기 때문에 값이 없거나, 있거나, 있어도 변할 수 있다. 즉, 내부 슬롯은 개발자가 직접적으로 접근할 수 없는 엔진 내부에서 사용하는 어떠한 공간?이고 그 공간에는 어떠한 프로퍼티가 저장되어 쓰인다라고 생각해보았다.. 예를 들어 23장 실행 컨텍스트의 [[ThisValue]] 처럼 전역객체를 바인딩하기 위해 [[ThisValue]]는 엔진 내부에 전역객체와 연결해주는 어떠한 값이 저장되어 있지 않을까 싶다..

### 프로퍼티 어트리뷰트와 프로퍼티 디스크립터 객체

- 프로퍼티의 상태엔 프로퍼티의 값, 값의 갱신 가능 여부, 열거 가능 여부, 재정의 가능 여부가 있다.
- 프로퍼티 어트리뷰트는 엔진이 관리하는 내부 상태 값인 내부 슬롯 [[Value]], [[Writable]], [[Enumerable]], [[Configurable]]이다.
- 엔진은 프로퍼티를 생성할 때 프로퍼티의 상태를 나타내는 프로퍼티 어트리뷰를 기본값으로 자동 정의한다.
- 프로퍼티 어트리뷰트는 Object.getOwnPropertyDescriptor 메서드를 사용하여 간접적으로 확인이 가능하다.
- Object.getOwnPropertyDescriptor 메서드는 프로퍼티 어트리뷰트 정보를 제공하는 프로퍼티 디스크립터 객체를 반환.

```jsx
const person = {
	name : 'Lee'
}

// 프로퍼티 동적 생성
person.age = 20;

// 모든 프로퍼티의 프로퍼티 어트리뷰트 정보를 제공하는 디스크립터 객체
console.log(Object.getOwnPropertyDescriptors(person)); // ES8

/*
{
	name : {value: 'Lee', writable: true, enumerable: true, configurable: true},
  name : {age: 20, writable: true, enumerable: true, configurable: true}
}
*/
```

### 데이터 프로퍼티와 접근자 프로퍼티

프로퍼티는 데이터 프로퍼티와 접근자 프로퍼티로 구분할 수 있다.

**데이터 프로퍼티**

- 키와 값으로 구성된 일반적인 프로퍼티다.

| 프로퍼티
어트리뷰트 | 프로퍼티 디스크립터
객체의 프로퍼티 | 설명 |
| --- | --- | --- |
| [[Value]] | value | - 프로퍼티 키를 통해 프로퍼티 값에 접근하면 반환되는 값.
- 프로퍼티 키를 통해 값을 변경하면[[Value]]에 값을 재할당한다. 이때 프로퍼티가 없으면 프로퍼티를 동적 생성하고 생성된 프로퍼티의 [[Value]]에 값을 저장 |
| [[Writable]] | writable | - 프로퍼티 값의 변경 가능 여부를 나타내며 불리언 값을 갖는다.
- [[Writable]]의 값이 false인 경우 해당 프로퍼티의 [[Value]]의 값을 변경할 수 없는 읽기 전용 프로퍼티가 된다. |
| [[Enumerable]] | enumerable | - 프로퍼티의 열거 가능 여부를 나타내며 불리언 값을 갖는다.
- [[Enumerable]]의 값이 false인 경우 해당 프로퍼티는 for … in문이나 Object.keys 메서드 등으로 열거할 수 없다. |
| [[Configurable]] | configurable | - 프로퍼티의 재정의 가능 여부를 나타내며 불리언 값을 갖는다.
- [[Configurable]]의 값이 false인 경우 해당 프로퍼티의 삭제, 프로퍼티 어트리뷰 값의 변경이 금지된다. 단, [[Writable]]이 true인 경우 [[Value]]의 변경과 [[Writable]]을 false로 변경하는 것은 허용된다. |

**접근자 프로퍼티**

- 자체적으로 값을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 호출되는 접근자 함수로 구성된 프로퍼티다.

| 프로퍼티 
어트리뷰트 | 프로퍼티 디스크립터
객체의 프로퍼티 | 설명 |
| --- | --- | --- |
| [[Get]] | get | 접근자 프로퍼티를 통해 데이터 프로퍼티의 값을 읽을 때 호출되는 접근자 함수다. 즉, 접근자 프로퍼티 키로, 프로퍼티 값에 접근하면 프로퍼티 어트리뷰트[[Get]]의 값, 즉 getter함수가 호출되고 그 결과가 프로퍼티 값으로 반환된다. |
| [[Set]] | set | 접근자 프로퍼티를 통해 데이터 프로프퍼티의 값을 저장할 때 호출되는 접근자 함수다. 즉, 접근자 프로퍼티 키로 프로퍼티 값을 저장하면 프로퍼티 어트리뷰트[[Set]]의 값, 즉 setter 함수가 호출되고 그 결과가 프로퍼티 값으로 저장된다. |
| [[Enumerable]] | enumerable | 데이터 프로퍼티의 [[Enumerable]]과 같다. |
| [[Configurable]] | configurable | 데이터 프로퍼티의 [[Configurable]]과 같다. |
- 접근자 함수는 getter/setter 함수라고도 부른다. 접근자 프로퍼티는 getter와 setter 함수를 모두 정의할 수 있고 하나만 정의할 수도 있다.

```jsx
const person = {
	// 데이터 프로퍼티
	firstName : 'Gildong'.
	lastName : 'Hong'

	// fullName은 접근자 함수로 구성된 접근자 프로퍼티
  // getter 함수
  get fullName() {
		return `${this.firstName} ${this.lastName}`;
	},
	// setter 함수
	set fullName(name) {
		// 배열 디스트럭 처링 할당 : 31장 배열에서 배움
		[this.firstName, this.lastName] = name.split(' ');
	}
};

// 데이터 프로퍼티를 통한 프로퍼티 값의 참조.
console.log(person.firstName + ' ' + person.lastName); // Gildong Hong

// 접근자 프로퍼티를 통해 프로퍼티 값의 저장
person.fullName = 'Dobin Shin'; // setter 함수가 호출

// 접근자 프로퍼티를 통해 프로퍼티 값의 참조
console.log(person.fullName); // Dobin Shin

// firstName은 어떤 프로퍼티?
let descriptor = Object.getOwnPropertyDescriptor(person, 'firstName');
console.log(desciptor); 
// 데이터 프로퍼티인 것을 알 수 있다.
// {value: "dobin", writable: true, enumerable: true, configurable: true);

// fullName은 어떤 프로퍼티?
descriptor = Object.getOwnPropertyDesciptor(person, 'fullName');
console.log(descriptor);
// {get: f, set : f, enumerable: true, configurable: true}
// 접근자 프로퍼티인 것을 알 수 있다.
```

- 접근자 프로퍼티는 자체적으로 값(프로퍼티 어트리뷰트[[Value]])을 가지지 않으며 다만 데이터 프로퍼티의 값을 읽거나 저장할 때 관여할 뿐이다.

**내부 슬롯/ 메서드 관점에서 설명**

- fullName으로 프로퍼티 값에 접근하는 경우 내부적으로 [[Get]] 내부 메서드가 호출되어 동작하는 과정
1. 프로퍼티 키가 유효한지 확인한다. 프로퍼티 키는 문자열 또는 심벌이어야 한다. 프로퍼티 키 “fullName”은 문자열이므로 유효한 프로퍼티 키다.
2. 프로토타입 체인에서 프로퍼티를 검색한다. person 객체에 fullName 프로퍼티가 존재한다.
3. 검색된 fullName 프로퍼티가 데이터 프로퍼티인지 접근자 프로퍼티인지 확인한다. fullName  프로퍼티는 접근자 프로퍼티다.
4. 접근자 프로퍼티 fullName의 프로퍼티 어트리뷰트 [[Get]]의 값, 즉 getter 함수를 호출하여 그 결과를 반환한다. 프로퍼티 fullName의 프로퍼티 어트리뷰트 [[Get]]의 값은 Object.getOwnPropertyDescriptor 메서드가 반환하는 프로퍼티 디스크립터 객체의 get 프로퍼티 값과 같다.

> 프로토 타입
프로토타입은 어떤 객체의 상위(부모) 객체의 역할을 하는 객체다. 프로토 타입은 하위(자식) 객체에게 자신의 프로퍼티와 메서드를 상속한다. 프로토타입 객체의 프로퍼티나 메서드를 상속받은 하위 객체는 자신의 프로퍼티 또는 메서드인 것처럼 자유롭게 사용할 수 있다.

프로토타입 체인은 프로토타입이 단방향 링크드 리스트 형태로 연결되어 있는 상속 구조를 말한다. 객체의 프로퍼티나 메서드에 접근하려고 할 때 해당 객체에 접근하려는 프로퍼티 또는 메서드가 없다면 프로토타입 체인을 따라 프로토타입의 프로퍼티나 메서드를 차례대로 검색한다.
> 

### 프로퍼티 정의

- 새로운 프로퍼티를 추가하면서 프로퍼티 어트리뷰를 명시적으로 정의하거나, 기존 프로퍼티의 프로퍼티 어트리뷰트를 재정의하는 것을 말함
- 프로퍼티 값을 갱신 가능하도록 할 것인지, 프로퍼티를 열거 가능하도록 할 것인지, 프로퍼티를 재정의 가능하도록 할 것인지 정의할 수 있다.
- Object.defineProperty 메서드를 사용하면 프로퍼티의 어트리뷰트를 정의할 수 있다. (인수로는 객체의 참조와 데이터 프로퍼티의 키인 문자열, 프로퍼티 디스크립터 객체를 전달)

```jsx
const person = {};

// 데이터 프로퍼티 정의
Object.defineProperty(person, 'firstName', {
	value : 'Dobin',
	writable : true,
	enumerable : true,
	configurable : true
});

Object.defineProperty(person, 'lastName', {
	value : 'Shin'
});

let descriptor = Object.getOwnPropertyDescriptor(person, 'firstName');
console.log('firstName', descriptor);
// firstName { value: 'Dobin', writable: true, enumerable: true, configurable: true}

// 디스크립터 객체의 프로퍼티를 누락시키면 undefined, false가 기본값
descriptor = Object.getOwnPropertyDescriptor(person, 'lastName');
console.log('lastName', descriptor);
// lastName { value: 'Shin', writable: false, enumerable: false, configurable: false}

// enumerable이 false인 경우 열거되지 않음
console.log(Object.keys(person)); // ["firstName"] → lastName은 false여서 열거되지 않음

// writable의 값이 false인 경우 value의 값을 변경할 수 없음
person.lastName = "Lee";

// configurable의 값이 false인 경우 해당 프로퍼티 삭제 불가
delete person.lastName; // 이때 프로퍼티를 삭제하면 에러는 발생 x, 무시됨

// configurable의 값이 false인 경우 해당 프로퍼티를 재정의할 수 없음
Object.defineProperty(person, 'lastName', { enumerable: true });
// TypeError: Cannot redefine property: lastName

// 접근자 프로퍼티 정의
Object.defineProperty(person, 'fullName', {
	// getter 함수
	get() {
		return `${this.firstName} ${this.lastName}`;
	},
	// setter 함수
	set(name) {
		[this.firstName, this.lastName] = name.split(' ');
	},
	enumerable: true,
	configurable: true
});

descriptor = Object.getOwnPropertyDescriptor(person, 'fullName');
console.log('fullName', descriptor);
// fullName { get: [Function: get], set: [Function: set], enumerable: true, configurable: true }
```

 

| 프로퍼티 디스크립터 
객체의 프로퍼티 | 대응하는 
프로퍼티 어트리뷰트 | 생략했을 때의 기본값 |
| --- | --- | --- |
| value | [[Value]] | undefined |
| get | [[Get]] | undefined |
| set | [[Set]] | undefined |
| writable | [[Writable]] | false |
| enumerable | [[Enumerable]] | false |
| configurable | [[Configurable]] | false |
- Object.defineProperties 메서드를 사용하면 여러 개의 프로퍼티를 한 번에 정의할 수 있다.

```jsx
const person = {};

Object.defineProperties(person, {
	// 데이터 프로퍼티 정의
	firstName: {
		value: 'Dobin',
		writable: true,
		enumerable: true,
		configurable: true
	},
	lastName: {
		value: 'Shin',
		writable: true,
		enumerable: true,
		configurable: true
	},
	// 접근자 프로퍼티 정의
	fullName: {
		// getter 함수
		get() {
			return `${this.firstName} ${this.lastName}`;
		},
		// setter 함수
		set(name) {
			[this.firstName, this.lastName] = name.split(' ');
		},
		enumerable: true,
		configurable: true
	}
});

person.fullName = 'Gildong Hong';
console.log(person); // {firstName: "Gildong", lastName: "Hong"}
```

### 객체 변경 방지

- 자바스크립트는 객체의 변경을 방지하는 다양한 메서드를 제공
- 객체 변경 방지 메서드들은 객체의 변경을 금지하는 강도가 다름

| 구분 | 메서드 | 프로퍼티 
추가 | 프로퍼티 
삭제 | 프로퍼티
값 읽기 | 프로퍼티
값 쓰기 | 프로퍼티  어트리뷰트 재정의 |
| --- | --- | --- | --- | --- | --- | --- |
| 객체 확장 금지 | Object.preventExtensions | X | O | O | O | O |
| 객체 밀봉 | Object.seal | X | X | O | O | X |
| 객체 동결 | Object.freeze | X | X | O | X | X |

### 객체 확장 금지

- 객체 확장 금지란 프로퍼티 추가 금지를 의미
- 동적추가와 Object.defineProperty 메서드를 통한 추가 모두 금지됨
- 확장이 가능한 객체인지 여부는 Object.isExtensible 메서드로 확인할 수 있다.

```jsx
const person = {name: 'Shin'};

// 확장 가능 여부 확인
console.log(Object.isExtensible(person)); // true

// person 객체의 확장을 금지하여 프로퍼티 추가를 금지
Object.preventExtensions(person);

// person 객체는 확장이 금지된 객체다.
console.log(Object.isExtensible(person)); // false

// 프로퍼티 추가가 금지된다.
person.age = 20; // 무시. strict mode에서는 에러

// 프로퍼티 추가는 금지되지만 삭제는 가능
delete person.name;
console.log(person); // {}

// 프로퍼티 정의에 의한 프로퍼티 추가도 금지된다.
Object.defineProperty(person, 'age', { value : 20 });
// TypeError: Cannot define property age, object is not extensible
```

### 객체 밀봉

- 객체 밀봉이란 프로퍼티 추가 및 삭제와 프로퍼티 어트리뷰트 재정의 금지를 의미한다. 즉, 밀봉된 객체는 읽기와 쓰기만 가능
- 밀봉된 객체인지 여부는 Object.isSealed 메서드로 확인할 수 있다.

```jsx
const person = { name : 'Shin' };

// person 객체는 밀봉(seal)된 객체가 아니다.
console.log(Object.isSealed(person)); // false

// person 객체를 밀봉(seal)하여 프로퍼티 추가, 삭제, 재정의를 금지한다.
Object.seal(person);

// person 객체는 밀봉(seal)된 객체다.
console.log(Object.isSealed(person)); // true

// 밀봉된(seal)된 객체된 configurable이 false다.
console.log(Object.getOwnPropertyDescriptors(person));
// { name {value: "Shin", writable: true, enumerable: true, configurable: false},}

// 프로퍼티 추가가 금지된다.
person.age = 20; // 무시. strict mode에서는 에러
console.log(person); // {name: "Shin"}

// 프로퍼티 삭제가 금지된다.
delete person.name; // 무시. strict mode에서는 에러
console.log(person); // {name: "Shin"}

// 프로퍼티 값 갱신은 가능하다.
person.name = "Lee";
console.log(person); // {name: "Lee"}

// 프로퍼티 어트리뷰트 재정의가 금지된다.
Object.defineProperty(person, 'name', { configurable: true });
// TypeError: Cannot redefine property: name
```

### 객체 동결

- 객체 동결이란 프로퍼티 추가 및 삭제와 프로퍼티 어트리뷰트 재정의 금지, 프로퍼티 값 갱신 금지를 의미. 즉, 동결된 객체는 읽기만 가능
- 동결된 객체인지 여부는 Object.isFrozen 메서드로 확인할 수 있다.

```jsx
const person = {name: 'Shin'};

// person 객체는 동결(freeze)된 객체가 아니다.
console.log(Object.isFrozen(person)); // false

// person 객체를 동결(freeze)하여 프로퍼티 추가, 삭제, 재정의, 쓰기 금지
Object.freeze(person);

// person 객체는 동결(freeze)된 객체다.
console.log(Object.isFrozen(person)); // true

// 동결(freeze)된 객체는 writable과 configurable이 false다.
console.log(Object.getOwnPropertyDescriptors(person));
// { name {value: "Shin", writable: false, enumerable: true, configurable: false},}

// 프로퍼티 추가가 금지된다.
person.age = 20; // 무시. strict mode에서는 에러
console.log(person); // {name: "Shin"}

// 프로퍼티 삭제가 금지된다.
delete person.name; // 무시. strict mode에서는 에러
console.log(person); // {name: "Shin"}

// 프로퍼티 값 갱신이 금지된다.
person.name = "Lee"; // 무시. strict mode에서는 에러
console.log(person); // {name: "Shin"}

// 프로퍼티 어트리뷰트 재정의가 금지된다.
Object.defineProperty(person, 'name', { configurable: true });
// TypeError: Cannot redefine property: name
```

### 불변 객체

- 위 변경 방지 메서드들은 얕은 변경 방지로 직속 프로퍼티만 변경이 방지되고 중첩 객체까지는 영향을 주지는 못한다. 따라서 Object.freeze 메서드로 객체를 동결하여도 중첩 객체까지 동결할 수 없다.
- 객체의 중첩 객체까지 동결하여 변경이 불가능한 읽기 전용의 불변 객체를 구현하려면 객체를 값으로 갖는 모든 프로퍼티에 대해 재귀적으로 Object.freeze 메서드를 호출해야 한다.

```jsx
function deepFreeze(target) {
	// 객체가 아니거나 동결된 객체는 무시하고 동결되지 않은 객체만 동결
	if(target && typeof target == 'object' && !Object.isFrozen(target)) {
		Object.freeze(target);
		// 모든 프로퍼티를 순회하며 재귀적으로 동결
		// Object.keys 메서드는 객체 자신의 열겨 가능한 프로퍼티 키를 배열로 반환
		// forEach 메서드는 배열을 순회하며 배열의 각 요소에 대하여 콜백 함수를 실행한다.
		Object.keys(target).forEach(key => deepFreeze(target[key]));
	}
	return target;
}

const person = {
	name: 'Shin',
	address: {city: 'Seoul'}
};

// 깊은 객체 동결
deepFreeze(person);

console.log(Object.isFrozen(person)); // true
// 중첩 객체까지 동결한다.
console.log(Object.isFrozen(person.address)); // true

person.address.city = "Busan";
console.log(person); // {name: "Shin", address: {city: "Seoul"}}
```