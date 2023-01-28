# 16. 프로퍼티 어트리뷰트

## 내부 슬롯과 내부 메서드

 ECMASciprt 에서 자바스크립트 엔진의 구현 알고리즘을 설명하기 위한 메서드라고 볼 수 있다.

```jsx
// 내부 슬롯과 내부 메서드는 이중 대괄호로 감싸져 있다.
// 원칙적으로 직접 접근 및 호출할 수 없도록 비공개된 객체의 프로퍼티이다. 단, 일부 내부 슬롯과 내부 메서드에 한해서 간접적으로 접근할 수 있다.
const o = {};

o.__proto__; // Object.[[Prototype]]
```

## 프로퍼티 어트리뷰트와 프로퍼티 디스크립터 객체

 프로퍼티 생성 시 프로퍼티의 상태를 나타네는 프로퍼티 어트리뷰트를 기본값으로 자동 정의한다.

- 값, 값의 갱신 가능 여부, 열거 가능 여부, 재정의 가능 여부를 말한다.
- 프로퍼티 어트리뷰트는 내부 슬롯 Value, Writable, Enumerable, Configurable 이다.

```jsx
// 프로퍼티 어트리뷰트는 내부 슬롯이기때문에 직접 접근이 안된다. 간접적으로 확인하려면 getOwnPropertyDescriptor 함수를 사용한다.
const person = {
	name: 'Lee',
	age: 27
};

// 프로퍼티 어트리뷰트 정보를 제공하는 프로퍼티 디스크립터 객체를 반환하는 함수다.
console.log(Object.getOwnPropertyDescriptor(person, 'name');
// value: 'lee', writable: true, enumerable: true, configurable: true

// ES8 에서 도입된 getOwnPropertyDescriptor 함수는 모든 프로퍼티의 프로퍼티 어트리뷰트의 정보를 제공한다.
console.log(Object.getOwnPropertyDescriptor(person);
/*
{
	name: { value: 'lee', writable: true, enumerable: true, configurable: true }
	age: { value: 27, writable: true, enumerable: true, configurable: true }
}
*/
```

## 데이터 프로퍼티와 접근자 프로퍼티

### 데이터 프로퍼티

- [[Value]] - value
    - 프로퍼티 키를 통해 접근 시 반환되는 값 ( [person.name](http://person.name) → lee )
    - 프로퍼티 키를 통해 값을 변경하면 해당 내부 슬롯에 값을 재할당하며, 프로퍼티가 없는 경우 프로퍼티를 동적 생성하고 내부 슬롯에 값을 저장한다.
- [[Writable]] - writable
    - 프로퍼티 값의 변경 가능 여부 (Boolean - true, false)
        - false 인 경우 내부 슬롯 [[Value]] 의 값을 변경할 수 없다.
- [[Enumerable]] - enumerable
    - 프로퍼티 열거 가능 여부 (Boolean - true, false)
        - false 인 경우 for … in 문이나 Object.keys 같은 메서드를 사용해 프로퍼티를 열거할 수 없다!
- [[Configurable]] - configurable
    - 프로퍼티 재정의 가능 여부 (Boolean - true, false)
        - false 인 경우 프로퍼티의 삭제, 프로퍼티 어트리뷰트 값의 변경 금지
            - [[Writable]] 이 true인 경우 [[Value]]의 변경과 [[Writable]]을 false로 변경하는 것은 허용된다.

```jsx
const person = {
	name: 'Lee'
};

// 프로퍼티 디스크립터 객체를 통해 데이터 프로퍼티를 조회할 수 있다.
console.log(Object.getOwnPropertyDescriptor(person, 'name'));
```

### 접근자 프로퍼티

- [[Get]] - get
    - 데이터 프로퍼티의 값을 읽을 때 호출되는 접근자 함수
- [[Set]] - set
    - 데이터 프로퍼티의 값을 저장할 때 호출되는 접근자 함수
- [[Enumerable]] - enumerable, [[Configurable]] - configurable
    - 데이터 프로퍼티의 [[Enumerable]], [[Configurable]] 과 같음.

🔎 접근자 프로퍼티를 통해 프로퍼티 값에 접근하면 get 내부 메서드가 호출되어 동작한다.

1. 프로퍼티 키가 유효한지 확인 (문자열 또는 심벌이어야 한다.)
2. 프로토타입 체인에서 프로퍼티를 검색
3. 검색된 프로퍼티가 데이터 프로퍼티인지 접근자 프로퍼티인지 확인한다.
4. getter 함수를 호출하여 결과를 반환한다.

<aside>
💡 프로토타입은 객체의 상위 객체의 역할을 하는 객체다.
프로토타입은 하위 객체에게 자신의 프로퍼티와 메서드를 상속하며 하위 객체는 이를 자유롭게 사용할 수 있다.

프로토타입 체인은 프로토타입이 단방향 링크드 리스트 형태로 연결되어 있는 상속 구조를 말한다. 객체의 프로퍼티나 메서드에 접근할 때 해당 프로퍼티 또는 메서드가 없다면 프로토타입 체인을 따라 차례대로 검색한다.

</aside>

```jsx
const person = {
	// 데이터 프로퍼티
	firstName: 'HaNeul',
	lastName: 'Lee',

	// 접근자 프로퍼티
	get fullName() {
		return `${this.firstName} ${this.lastName}`;
	},
	set fullName(name) {
		// 배열 디스트럭처링 (31장..)
		[this.firstName, this.lastName] = name.split(' ');
	}
};

// 데이터 프로퍼티를 통한 프로퍼티 값 참조
console.log(person.firstName); // HaNeul Lee
// 접근자 프로퍼티를 통함 값의 저장과 참조
person.fullName = 'GilDong Hong';
console.log(person.fullName); // GilDong Hong

```

## 프로퍼티 정의

```jsx
const person = {};

// 데이터 프로퍼티 정의
Object.defineProperty(person, 'firstName', {
	value: 'HaNeul',
	writable: true,
	enumerable: true,
	configurable: ture
});
// value: 'HaNeul', writable: true, enumerable: true, configurable: true

// 디스크립터 객체의 프로퍼티를 누락시키면 undefined 로 초기화 즉 false가 들어간다.
Object.defineProperty(person, 'lastName', {
	value: 'Lee'
});
// value: 'Lee', writable: false, enumerable: false, configurable: false

// Enumerable 값이 false 인 경우는 열거함수에서 제외됨 ( 열거되지 않는다. )
console.log(Object.keys(person)); // ['firstName']

// Writable 값이 false 인 경우 프로퍼티의 value 값을 변경할 수 없다!
person.lastName = 'Kim'; // 에러가 발생하지 않고 무시된다.
console.log(person.lastName); // Lee

// Configurable 값이 false 인 경우 해당 프로퍼티를 삭제 및 재정의 할 수 없다.
delete person.lastName; // 에러가 발생하지 않고 무시된다.
// Object.defineProperty(person, 'lastName', { enumerable: true });
// Error -> cannnot redefine property : lastName

// 접근자 프로퍼티 정의
Object.defineProperty(person, 'fullName', {
	get() {
		return `${this.firstName} ${this.lastName}`;
	},
	set(name) {
		[this.firstName, this.lastName] = name.split(' ');
	},
	enumerable: true,
	configurable: true
});

// 여러개의 프로퍼티 정의
Object.defineProperties(person, {
	age: {
		value: 27,
		writable: true,
		enumerable: true,
		configurable: true
	},
	hobby: {
		value: 'Game'
	}
});

```

## 객체 변경 방지

| 구분 | 메서드 | 프로퍼티 추가 | 프로퍼티 삭제 | 프로퍼티 값 읽기 | 프로퍼티 값 쓰기 | 프로퍼티 어트리뷰트 재정의 |
| --- | --- | --- | --- | --- | --- | --- |
| 객체 확장 금지 | Object.preventExtensions | X | O | O | O | O |
| 객체 밀봉 | Object.Seal | X | X | O | O | X |
| 객체 동결 | Object.freeze | X | X | O | X | X |

### 객체 확장 금지

```jsx
// 객체 확장 금지는 푸로퍼티 추가를 금지한다는 의미이다.
const person = { name: 'Lee' };

// 확장이 금지되었는지 확인
console.log(Object.isExtensible(person)); // true

// person 객체의 확장을 금지
Object.preventExtensions(person);

person.age = 27; // 에러 없이 무시된다. (Strict mode 에서는 에러 발생)

// 삭제는 가능하다.
delete person.name;
console.log(person); // {}
```

### 객체 밀봉

```jsx
// 객체 밀봉은 프로퍼티 추가 및 삭제와 프로퍼티 어트리뷰트 재정의 금지를 의미한다. (읽기 쓰기만 가능)
const person = { name: 'Lee' };

// 객체 밀봉 여부 확인
console.log(Object.isSealed(person)); // false

// 객체를 밀봉
Object.seal(person);

// 밀봉된 객체는 configurable이 false다.
console.log(Object.getOwnPropertyDecriptors(person);
// name: {value: 'lee', writable: true, enumerable: true, configurable: false}

// 프로퍼티 추가, 삭제 금지 및 프로퍼티 어트리뷰트 재정의 금지
person.age = 27; // 에러 없이 무시된다. (Strict mode에서는 에러 발생)
delete person.name; // 에러 없이 무시된다. (Strict mode에서는 에러 발생)
Object.defineProperty(person, 'name', { configurable: true });
// Error -> Cannot redefine property: name
```

### 객체 동결

```jsx
// 객체 동결은 프로퍼티 추가 및 삭제와 프로퍼티 어트리뷰트 재정의 금지, 프로퍼티 값 갱신 금지를 의미한다. (읽기만 가능하다.)
const person = { name: 'Lee' };

// 객체 동결 여부 확인
console.log(Object.isFrozen(person));

// 객체를 동결하여 읽기만 가능하도록 함
Object.freeze(person);

// 동결된 객체는 writable과 configurable이 fasle가 된다.
console.log(Object.getownPropertyDescriptoers(person));
// name: {value: 'lee', writable: false, enumerable: true, configurable: false}

// 동결된 객체는 프로퍼티 추가, 삭제, 값 갱신, 어트리뷰트 재정의 금지
person.age = 20; // 추가 시 에러 없이 무시됨 (Strict mode 에서는 에러)
delete person.name; // 삭제 시 에러 없이 무시됨 (Strict mode 에서는 에러)
person.name = 'Kim'; // 값 갱신 시 에러 없이 무시됨 (Strict mode 에서는 에러)
Object.defineProperty(person, 'name', { configurable: true }); // Error -> Cannot redefine property: name 
```

### 불변 객체

```jsx
const person = {
	name: 'Lee',
	address: { city: 'Seoul' }
};

// person 객체를 동결
Object.freeze(person);

concole.log(Object.isFrozen(person)); // true
console.log(Object.isFrozen(person.address)); // false
// 중첩 객체까지 동결되진 않는다.
person.address.city = 'Busan'; // 값 갱신이 가능하다.

// 객체의 중첩 객체까지 동결하여 불변 객체를 구현하려면 객체를 값으로 갖는 모든 프로퍼티에 대해 재귀적으로 Object.freeze 메서드를 호출해야 한다.
function deepFreeze(target) {
	// 객체이면서 동결되지 않은 객체인 경우만 실행
	if (target && typeof target === 'object' && !Object.isFrozen(target)) {
		Object.freeze(target));
		
		// Object.keys 메서드로 열거 가능한 프로퍼티 키를 배열로 반환하고 forEach 메서드를 통해 배열을 순회하여 각 요소에 대해 콜백함수를 실행한다.
		Object.keys(target).forEach(key => deepFreeze(target[key]));
	}
	return target;
}

const multiObj = {
	aObj: {
		a: 'a'
	}, 
	bObj: {
		b: 'b'
	}
};
deepFreeze(multiObj);

console.log(Object.isFrozen(multiObj)); // true
console.log(Object.isFrozen(multiObj.aObj)); // true
console.log(Object.isFrozen(multiObj.bObj)); // true
```