# 12.함수

---

### 1. 함수란

---

- 일련의 과정을 문(statement)으로 구현하고 코드 블록으로 감싸서 하나의 실행 단위로 정의한 것
- 매개변수와 인수, 반환값으로 구성
`매개변수(parameter)` : 함수 내부로 입력을 전달 받는 변수
`인수(arguemnt)` : 입력 값
`반환값(return value)` : 출력 값

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d57b5779-f737-46e1-a09e-9ed884f63e85/Untitled.png)

- 함수 정의와 함수 호출을 통해 사용

```jsx
//함수 정의
function add(x, y) {
	return x + y;
}

//함수 호출
var result = add(2,5);
console.log(result); //7
```

- 코드의 재사용 측면에서 유리
- 자바스크립트에서 함수는 객체 타입

### 2. 함수 리터럴

---

함수는 객체 타입의 값으로 함수 리터럴은 function키워드, 함수 이름, 매개변수 목록, 함수 몸체로 구성된다.

```jsx
//변수에 함수 리터럴을 할당
var f = function add(x,y) {
	return x + y;
}
```

**함수리터럴의 구성 요소**

### 3. 함수 정의

---

함수를 호출하기 이전 인수를 전달받을 매개변수와 실행할 문들, 반환할 값을 지정하는 것으로, 정의된 함수는 자바스크립트 엔진에 의해 평가되어 함수 객체가 된다. 

**함수 선언문**

함수 선언문은 함수 리터럴과 형태가 동일하지만, 함수 리터럴과 달리 함수 이름을 생략할 수 없다.

```jsx
//함수 선언문은 함수 이름을 생략할 수 없다.
function (x,y) {
	return x + y;
}

//SyntaxError : function statements require a function name
```