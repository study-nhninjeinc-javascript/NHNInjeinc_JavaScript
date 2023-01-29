## 16.1 내부 슬롯과 내부 매서드

- 내부 슬롯과 내부메서드는 자바스크립트 엔진의 구현 알고리즘을 설명하기 위해 ECMAScript 사양에서 사용하는 의사프로퍼티와 의사메서드다
- 내부 슬롯 : ECMAScript 사양에서 사용하는 의사프로퍼티
- 내부 메서드 : ECMAScript 사양에서 사용하는 의사메서드
- ECMAScript 사양에 등장하는 이중대괄호로 감싼 이름들이 내부슬롯과 내부메서드다.

[http://ecma-international.org/ecma-262/11.0/#sec-object-internal-methods-and-internal-slots](http://ecma-international.org/ecma-262/11.0/#sec-object-internal-methods-and-internal-slots)

- 내부슬롯과 내부메서드는 ECMAScrip사양에 정의된 대로 구현되어 자바스크립트엔진에서 실제로 동작하지만 개발자가 직접 접근할수있도록 외부로 공개된 객체의 프로퍼티가아니다.
- 즉 내부슬롯과 내부 메서드는 자바스크립트 엔진의 내부로직이므로 원칙적으로 자바스크립트는 내부슬롯과 내부메서드에 직접적으로 접근하거나 호출할수있는 방법을 제공하지 않는다
- 단, 일부내부 슬롯과 내부 메서드에 한하여 간접적으로 접근할수 있는 수단을 제공하기는한다.

```jsx
const o = {};
o.[[Prototype]]; //Uncaught SyntaxError: Unexpected token '['
o.__proto__; //{constructor: ƒ, __defineG...
```

의사프로퍼티?

의사메서드?

ECMAScript : 

왜 직접 접근하지 못하게 만들었을까?
대괄호표기법이 있어서 그런건가?

## 16.2 프로퍼티 어트리뷰트와 프로퍼티 디스크립터 객체

- **자바스크립트 엔진은 프로퍼티를 생성할때 프로퍼티의 상태를 나타내는 프로퍼티 어트리뷰트를 기본값으로 자동정의한다.**
- 프로퍼티의 상태 : 프로퍼티의 값(Value), 값의 갱신여부(Writable), 열거 가능 여부(Enumerable). 재정의 가능 여부(Configurable)를 말한다
- 프로퍼티 어트리뷰트는 자바스크립트 엔진이 관리하는 내부 상태 값 인 내부슬롯[[Value]], [[Writable]], [[Enumerable]], [[Configurable]]이다
- 따라서 프로퍼티 어트리뷰트에 직접 접근 할 수 없지만 Object.getOwnPropertyDescriptor 메서드를 사용하여 간접적으로 확인할수는있다.

```jsx
const person = {
	name : 'Lee',
	age : 28
}
//프로퍼티 어트리뷰트 정보를 제공하는 프로퍼티 디스크립터 객체를 반환
//만약 존재하지 않으면 undefined가 반환
console.log(Object.getOwnPropertyDescriptor(person, 'name'));
//{value: 'Lee', writable: true, enumerable: true, configurable: true}

//모든 프로퍼티의 프로퍼티 어트리뷰트 정보를 제공하는 프로퍼티 디스크립터 객체 반환
console.log(Object.getOwnPropertyDescriptors(person));
//{name: {…}, age: {…}}
```

값의 갱신과 재정의 가능 여부의 차이점?

Object.getOwnPropertyDescriptor

## 16.3 데이터 프로퍼티와 접근자 프로퍼티

- 데이터 프로퍼티 : 키와 값으로 구성된 일반적인 프로퍼티.
- 접근자 프로퍼티 : 자체적으로 값을 갖지 않고 다른데이터 프로퍼티의 값을 읽거나 저장할때 호출되는 접근자 함수로 구성된 프로퍼티

### 16.3.1 데이터프로퍼티

| 프로퍼티
어트리뷰트 | 프로퍼티 디스크립터
객체의 프로퍼티 | 설명 |
| --- | --- | --- |
| [[Value]] | value | 프로퍼티 키를 통해 프로퍼티 값어 접근하면 반환되는 값
프로퍼티 키를 통해 프로퍼티 값을 변경하면 [[Value]]에 값을 재할당. 
이때 프로퍼티가 없으면 프로퍼티를 동적 생성, 생성된 프로퍼티의 [[Value]]에 값을 저장 |
| [[Writable]] | writable | 프로퍼티 값의 변경 가능 여부, 불리언값을 갖는다
[[Writable]]의 값이 false인 경우 해당 프로퍼티의  [[Value]]의 값을 변경할 수 없는 읽기 전용 프로퍼티가 된다. |
| [[Enumerable]] | enumerable | 프로퍼티 값의 변경 가능 여부, 불리언값을 갖는다
[[Enumerable]]의 값이 false인 경우 해당 프로퍼티는 for…in 문이나 Object.keys 메서드 등으로 열거할 수 없다. |
| [[Configurable]] | configurable | 프로퍼티 재정의 가능 여부, 불리언값을 갖는다
[[Configurable]]의 값이 false인 경우 해당 프로퍼티의 삭제, 프로퍼티 어트리뷰트 값의 변경이 금지된다.
단, [[Writable]]이 true인 경우 [[Value]]의 변경과 [[Writable]]을 false로 변경하는것은 허용된다. |
- 프로퍼티가 생성될때 [[Value]]는 프로퍼티의 값으로 [[Writable]], [[Enumerable]], [[Configurable]]의 값은 true로 초기화된다.

[[Writable]] [[Configurable]] 의 차이점?

### 16.3.2 접근자 프로퍼티

- 자체적으로 값을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할때 사용하는 접근자 함수로 구성된 프로퍼티

| 프로퍼티
어트리뷰트 | 프로퍼티 디스크립터
객체의 프로퍼티 | 설명 |
| --- | --- | --- |
| [[Get]] | get | 접근자 프로퍼티를 통해 데이터 프로퍼티의 값을 읽을때 호출되는 접근자 함수
접근자 프로퍼티 키로 프로퍼티 값에 접근하면 프로퍼티 어트리뷰트 [[Get]]의 값, 즉 getter 함수가 호출되고, 그결과 프로퍼티 값으로 반환 |
| [[Set]] | set | 접근자 프로퍼티를 통해 데이터 프로퍼티의 값을 저장할때 호출되는 접근자함수
접근자 프로퍼티 키로 프로퍼티 값을 저장하면 프로퍼티 어트리뷰트 [[Set]]의 값, 즉 setter 함수가 호출되고, 그결과 프로퍼티 값으로 반환 |
| [[Enumerable]] | enumerable | 데이터 프로퍼티의 [[Enumerable]]와 같다 |
| [[Configurable]] | configurable | 데이터 프로퍼티의 [[Configurable]]와 같다 |
- 접근자 함수는 setter/getter 함수라고도 부른다
- 접근자 프로퍼티는 getter와 setter 함수를 모두 정의할 수도 있고 하나만정의할수도있다.

```jsx
const person = {
	firstName : 'Suin',
	lastName : 'Lee',
	//fullName은 접근자 함수로 구성된 접근자 프로퍼티
	//getter 함수
	get fullName(){
		return '${this.firstName} ${this.lastName}';
	},
	//setter 함수
	set fullName(name){
		//배열디스트럭처링 할당 : 31.1 부분 참고
		[this.firstName, this.lastName] = name.split(' ');
	}
}

console.log(person.firstName + ' ' + person.lastName);//Suin Lee

//접근자 프로퍼티를 통한 프로퍼티 값의 저장
//접근자 프로퍼티 fullName에 값을 저장하면 setter 함수가 호출된다.
person.fullName = 'Sarang Lee';
console.log(person);

//접근자 프로퍼티를 통한 프로퍼티 값의 참조
//접근자 프로퍼티  fullName에 접근하면 getter 함수가 호출된다,
console.log(person.fullName); //Sarang Lee

//firstName 데이터 프로퍼티다.
//데이터 프로퍼티는 [[Value]],[[Writable]],[[Enumerable]],[[Configurable]]
//프로퍼티 어트리뷰트를 갖는다
let descriptor = Object.getOwnPropertyDescriptor(person, 'firstName');
console.log(descriptor);
//{value: 'Sarang', writable: true, enumerable: true, configurable: true}

//fullName은 접근자 프로퍼티다.
//데이터 프로퍼티는 [[Get]],[[set]],[[Enumerable]],[[Configurable]]
//프로퍼티 어트리뷰트를 갖는다
descriptor = Object.getOwnPropertyDescriptor(person, 'fullName');
console.log(descriptor);
//{enumerable: true, configurable: true, get: ƒ, set: ƒ}

//일반객체의 __proto__는 접근자 프로퍼티다.
Object.getOwnPropertyDescriptor(Object.prototype, '__proto__');
//{enumerable: false, configurable: true, get: ƒ, set: ƒ}

//일반객체의 __proto__는 데이터 프로퍼티다.
Object.getOwnPropertyDescriptor(function(){}, 'prototype');
//{value: {…}, writable: true, enumerable: false, configurable: false}
```

1. 프로퍼티 키가 유효한지 확인한다. 프로퍼티 키는 문자열 또는 심벌이어야한다. 프로퍼티키 “fullName”은 문자열이므로 유효한프로퍼티키다
2. 프로토타입체인에서 프로퍼티를 검색한다. person 객체에 fullNmae 프로퍼티가 존재한다
3. 검색된 fullName프로퍼티가 데이터 프로퍼티인지 접근자 프로퍼티인지확인.
4. 접근자프로퍼티 fullName의 프로퍼티 어트리뷰트[[Get]]의 값 즉 getter 함수를 호출하여 결과를 반환
프로퍼티  fullName의 프로퍼티 어트리뷰트 [[Get]]의 값은 Object.getOwnPropertyDescriptor 매서드가 반환하는 프로퍼티 디스크립터 객체의 get프로퍼티 값과 같다.

getter 함수

setter 함수

프로토타입 : 어떤객체의 상위 객체의 역할을 하는 객체 → 19장에서 배울 수 있다.

## 16.4 프로퍼티 정의

- 프로퍼티 정의 : 새로운 프로퍼티를 추가하면서 프로퍼티 어트리뷰트를 명시적으로 정의하거나 기존프로퍼티의 프로퍼티 어트리뷰트를 재정의 하는 것
- Object.defineProperty 메서드를 사용하면 프로퍼티의 어트리뷰트를 정의할수있다. 인수로는 객체의 참조와 데이터 프로퍼티의 키인 문자열, 프로퍼티 디스크립터 객체를 전달한다.

```jsx
const person = {};

Object.defineProperty(person, 'firstName',{
	value : 'Suin',
	writable : true,
	enumerable : true,
	configurable : true
});

Object.defineProperty(person, 'lastName',{
	value : 'Lee'
});

let descriptor = Object.getOwnPropertyDescriptor(person, 'firstName');
console.log('firstName', descriptor);
//firstName {value: 'Suin', writable: true, enumerable: true, configurable: true}

//디스크립터 객체의 프로퍼티를 누락시키면 undefined, false가 기본값이다.
descriptor = Object.getOwnPropertyDescriptor(person, 'lastName');
console.log('lastname', descriptor);
//lastname {value: 'Lee', writable: false, enumerable: false, configurable: false}

//[[Enumerable]]의 값이 false인 경우
//해당프로퍼티는 for ... in문이나 Object.keys로 열거할수없다.
//lastNamevmfhvjxlsms [[Enumerable]]가 false이므로 값을 변경할수없다.
console.log(Object.keys(person));
//['firstName']

//[[Writable]]의 값이 false인경우 해당 프로퍼티의 [[Value]]의 값을 변경할수 없다.
//lastName프로퍼티는 [[Writable]] 값이 false이므로 값을 연결할수없다.
//이때 값을 변경하면 에러는 발생 X
person.lastName = 'Kim';
console.log(person.lastName);
//Lee

//[[Configurable]]의 값이 false인경우 해당 프로퍼티를 삭제할 수 없다.
//lastName프로퍼티는 [[Configurable]] 값이 false이므로 값삭제할수없다.
//이때 값을 변경하면 에러는 발생 X
 delete person.lastName;
console.log(person.lastName);
//Lee

descriptor = Object.getOwnPropertyDescriptor(person, 'lastName');
console.log('lastName', descriptor);
//lastName {value: 'Lee', writable: false, enumerable: false, configurable: false}

//접근자 프로퍼티 정의
Object.defineProperty(person, 'fullName',{
	//getter 함수
	get(){
		return '${this.firstName} ${this.lastName}';
	},
	//setter 함수
	set(name){
		[this.firstName, this.lastName] = name.split(' ');
	},
	enumerable : true,
	configurable : true
});

descriptor = Object.getOwnPropertyDescriptor(person, 'fullName');
console.log('fullName', descriptor);
//fullName {enumerable: true, configurable: true, get: ƒ, set: ƒ}

person.fullName = 'Kim keke';
console.log(person);
//{firstName: 'keke', lastName: 'Lee'}
```

- Object.defineProperty 메서드로 프로퍼티를 정의할때 프로퍼티 디스크립터 객체의 프로퍼티를 일부 생략할수있다.
- 생략된 어트리뷰트는 기본값이 적용된다.
[[Value]], [[Get]], [[Set]] - undefined
[[Writable]], [[Enumerable]], [[Configurable]] - false
- Object.defineProperties메서드를 사용하면 여러개의 프로퍼티를 한번에 정의할수있다.

## 16.5 객체 변경 방지

- 자바스프립트는 객체의 변경을 방지하는 다양한매서드를 제공
- 객체변경방지 메서드들은 객체의 변경을 금지하는 강도가 다르다

| 구분 | 메서드 | 프로퍼티 추가 | 프로퍼티 삭제 | 프로퍼티 값 읽기 | 프로퍼티 값 쓰기 | 프로퍼티 어트리뷰트 제정의 |
| --- | --- | --- | --- | --- | --- | --- |
| 객체 확장 금지 | Object.preventExtensions | X | O | O | O | O |
|  객체 밀봉 | Object.seal | X | X | O | O | X |
| 객체 동결 | Object.freeze | X | X | O | X | X |

### 16.5.1 객체 확장 금지

Object.preventExtensions메서드는 객체 확장을 금지

확장이 금지된 객체는 프로퍼티 추가금지

여부는 Object.isExtensions메서드로 확인 가능

### 16.5.2 객체 밀봉

Object.seal메서드는 객체를 밀봉

밀봉된객체는 읽기와 쓰긴만 가능

여부는 Object.isSeal메서드로 확인가능

### 16.5.3 객체 동결

Object.freeze메서드는 객체를 동결

동결된 객체는 읽기만 가능

여부는 Object.isFreeze메서드로 확인 가능

### 16.5.4 불변객체

위는 얕은 변경방지로 직속프로퍼티만 변경되고 중첩객체까지는 영향을 주지못한다.

Object.freeze메서드로 동결하여도 중첩객체까지 동결 할 수 없다.

중첩까지 불변객체로 구현하려면 재귀적으로 Object.freeze메서드를 호출해야한다.