# 16_프로퍼티 어트리뷰트

for in 문 

- 해당 객체의 모든 열거할 수 있는 프로퍼티(enumerable properties)를 순회할 수 있도록 도움

## **내부 슬롯과 내부 메서드**

내부 슬롯과 내부 메서드는 자바스크립트 엔진의 구현 알고리즘을 설명하기 위해 ECMAScript 사양에서 사용하는 의사 프로퍼티(pseudo property)와 의사 메서드(pseudo method)다.

```jsx
const o = {};
// o.[[prototype]]  //SyntaxError
console.log(o.__proto__) //[Object: null prototype] {}
```

## **프로퍼티 어트리뷰트와 프로퍼티 디스크립터 객체**

자바스크립트 엔진은 프로퍼티 생성 시 프로퍼티의 상태(**value, writable, enumerable, configurable**)를 나타낸는 프로퍼티 어트리뷰트를 기본적으로 자동 정의

Object.getOwnPropertyDescriptor 메서드를 사용해서 간접적으로 확인은 가능.

```jsx
const person = {
    name : 'kim'
}

console.log(Object.getOwnPropertyDescriptor(person(객체참조), 'name'(프로퍼티 키)))
//{ value: 'kim', writable: true, enumerable: true, configurable: true }
```

뒤에 -s를 붙인 **getOwnPropertyDescriptors** 메서드는 모든 프로퍼티의 프로퍼티 어트리뷰트 정보를 제공하는 프로퍼티 디스크립터 객체를 반환.

```jsx
const person = {
    name : 'kim'
}

person.age = 20;

console.log(Object.getOwnPropertyDescriptors(person))
//{name: {…}, age: {…} }
// //{
//     name: {
//         value: 'kim',
//         writable: true,
//         enumerable: true,
//         configurable: true
//       },
//       age: { value: 20, writable: true, enumerable: true, configurable: true }
//     }
```

## **데이터 프로퍼티와 접근자 프로퍼티**

프로퍼티는 **데이터 프로퍼티**와 **접근자 프로퍼티**로 구분

- [[Value]] : 프로퍼티 키를 통해 값에 접근하면 반환되는 값
- [[Writable]] : 프로퍼티 값의 변경 가능 여부를 나타내벼 불리언 값을 갖는다.
- [[Enumerable]]: 프로퍼티 열거 가능 여부
- [[Configuralbe]] : 재정의 가능 여부

프로퍼티가 생성될 때 [[Value]]의 값은 프**로퍼티 값으로 초기화** 

[[Writable]] 등의 값은 **true로 초기화**. 이것은 프로퍼티를 동적으로 추가해도 마찬가지.

접근자 프로퍼티

자체적으로는 값을 갖지 않고 

다른 데이터프로퍼티의 값을 읽거나 저장할 때 호출되는 접근자 함수로 구성된 프로퍼티