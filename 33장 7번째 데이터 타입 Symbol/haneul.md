# 심볼

> ES6에서 도입된 7번째 데이터 타입으로 변경 불가능한 원시 타입의 값이다. + 다른 값과 중복되지 않는 유일무이한 값이다.
> 

## 심벌 값의 생성

### Symbol 함수

> 심벌 값은 Symbol 함수를 호출하여 생성한다.  이때 생성된 심벌 값은 외부로 노출되지 않아 확인할 수 없으며 다른 값과 절대 중복되지 않는 유일무이한 값이다.
> 

```jsx
const mySymbol = Symbol(); // new 연산자와 함께 호출하지 않는다.
console.log(typeof mySymbol); // symbol

// 심벌 값은 외부로 노출되지 않아 확인할 수 없다.
console.log(mySymbol); // Symbol()

// 문자열을 인수로 전달할 수 있으나 값에 대한 설명정도로만 사용되며 값 생성에 영향을 주지 않는다.
const mySymbol1 = Symbol('mySymbol');
const mySymbol2 = Symbol('mySymbol');

console.log(mySymbol1 === mySymbol2); // false

// 심벌 값도 객체처럼 접근하면 암묵적으로 래퍼 객체를 생성한다.
console.log(mySymbol.description); // mySymbol -> description 은 Symbol.prototype의 프로퍼티다.

// 심벌 값은 암묵적으로 문자열이나 숫자 타입으로 변환되지 않는다.
console.log(mySymbol + ''); // error

// 불리언 타입으로는 암묵적으로 타입 변환된다. (존재 확인 가능)
console.log(!!mySymbol); // true
if (mySymbol) console.log('mySymbol is not empty');
```

### Symbol.for / Symbol.keyFor 메서드

- Symbol.for 메서드는 인수로 전달받은 문자열을 키로 사용하여 키와 심벌 값의 쌍들이 저장되어 있는 전역 심벌 레지스트리에서 해당 키와 일치하는 심벌 값을 검색한다.
    - 검색 성공 시 새로운 심벌 값을 생성하지 않고 검색된 심벌 값을 반환한다.
    - 검색 실패 시 새로운 심벌 값을 생성하여 Symbol.for 메서드의 인수로 전달되 키로 전역 심벌 레지스트리에 저장한 후, 생성된 심벌 값을 반환한다.

```jsx
const s1 = Symbol.for('mySymbol'); // 전역 심벌 레지스트리에 mySymbol 이라는 키로 심벌 값 생성
const s2 = Symbol.for('mySymbol'); // 전역 심벌 레지스트리에 mySymbol 이라는 키로 저장된 심벌 값 가져옴

console.log(s1 === s2); // true
```

- Symbol.keyFor 메서드를 사용하면 전역 심벌 레지스트리에 저장된 심벌 값의 키를 추출할 수 있다.

```jsx
const s1 = Symbol.for('mySymbol'); // for 메서드를 통해서 전역 심벌 레지스트리에 지정된 키로 심벌 값 저장
Symbol.keyFor(s1); // mySymbol

const s2 = Symbol('foo'); // Symbol 함수로 생성한 심벌 값은 전역 심벌 레지스트리에 등록되어 관리되지 않는다.
Symbol.keyFor(s2); // undefined
```

## 심벌과 상수

```jsx
const Direction = {
	UP: 1,
	DOWN: 2,
	LEFT: 3,
	RIGHT: 4
};

const myDirection = Direction.UP;

if (myDirection === Direction.UP) console.log('You are going UP');
```

```jsx
const Direction = {
	UP: Symbol('up'),
	DOWN: Symbol('down'),
	LEFT: Symbol('left'),
	RIGHT: Symbol('right')
};

const myDirection = Direction.UP;

if (myDirection === Direction.UP) console.log('You are going UP');
```

## 심벌과 프로퍼티 키

> 객체의 프로퍼티 키는 빈 문자열을 포함하는 모든 문자열 또는 심벌 값으로 만들 수 있으며 동적으로 생성할 수 있다.
> 

```jsx
const obj = {
	// 심벌 값을 프로퍼티 키로 사용하려면 프로퍼티 키로 사용할 심벌 값에 대괄호를 사용해야 한다.
	[Symbol.for('mySymbol')]: 1
};

obj[Symbol.for('mySymbol')]; // 1
```

<aside>
💡 심벌 값은 유일무이한 값이므로 심벌 값으로 프로퍼티 키를 만들면 다른 프로퍼티 키와 절대 충돌하지 않는다.

</aside>

## 심벌과 프로퍼티 은닉

> 심벌 값을 프로퍼티 키로 사용하여 생성한 프로퍼티는 for…in 문이나 Object.keys, Object.getOwnPropertyNames 메서드로 찾을 수 없다.
> 

<aside>
💡 심벌 값을 프로퍼티 키로 사용하여 외부에 노출할 필요가 없는 프로퍼티를 은닉하며 생성할 수 있다. 그러나 ES6에서 도입된 Object.getOwnPropertySymbols 메서드를 사용하면 찾을 수 있다..

</aside>

## 심벌과 표준 빌트인 객체 확장

> 표준 빌트인 객체에 사용자 정의 메서드를 직접 추가하여 확장하는 것은 권장하지 않는다. → 개발자가 직접 추가한 메서드와 미래에 표준 사양으로 추가될 메서드의 이름이 중복될 수 있다.
> 
- 중복될 가능성이 없는 심벌 값으로 프로퍼티 키를 생성하여 포준 빌트인 객체를 확장하면 표준 빌트인 객체의 기존 프로퍼티 키와 충돌하지 않는다.

## Well-known Symbol

> 자바스크립트가 기본 제공하는 빌트인 심벌 값이 있다. 이는 Symbol 함수의 프로퍼티에 할당되어 있다. - 이를 Well-known Symbol이라 부른다.
> 
- Well-known Symbol은 자바스크립트 엔진의 내부 알고리즘에 사용된다.
    - Array, String, Map 등과 같이 for…of 문으로 순회 가능한 빌트인 이터러블은 Well-known Symbol인 Symbol.iterator를 키로 갖는 메서드를 가지며 Symbol.iterator 메서드를 호출하면 이터레이터를 반환하도록 규정되어 있다.

<aside>
💡 빌트인 이터러블이 아닌 일반 객체를 이터러블처럼 동작하도록 하려면 이터레이션 프로토콜을 따르면 된다. Well-known Symbol인 Symbol.iterator를 키로 갖는 메서드를 객체에 추가하고 이터레이터를 반환하도록 구현한다.

</aside>