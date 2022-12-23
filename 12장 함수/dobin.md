# 12장 함수

### 함수란 ?

- 프로그래밍 언어의 함수는 일련의 과정을 문(statement)으로 구현하고 코드 블록으로 감싸서 하나의 실행 단위로 정의한 것이다.
- 이때 함수 내부로 입력을 전달받는 변수를 매개변수(parameter), 입력을 인수 (argument), 출력을 반환값(return value)이라 한다.

![https://dev-dobin.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fc21faefa-3cbd-43f8-83d1-8022c7e09473%2FUntitled.png?id=e65ecaae-1c58-49ee-bf30-94d48876f5ce&table=block&spaceId=f800ef36-5d6f-4e98-a2f1-f4b2d48cc3fe&width=770&userId=&cache=v2](https://dev-dobin.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fc21faefa-3cbd-43f8-83d1-8022c7e09473%2FUntitled.png?id=e65ecaae-1c58-49ee-bf30-94d48876f5ce&table=block&spaceId=f800ef36-5d6f-4e98-a2f1-f4b2d48cc3fe&width=770&userId=&cache=v2)

### 함수를 사용하는 이유

- 함수는 몇 번이든 호출할 수 있으므로 코드의 재사용이라는 측면에서 매우 유용하다. 아래는 함수를 통해 중복을 제거하여 코드를 재사용한 예

![https://dev-dobin.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fd9cb0b0e-6afe-41c8-aa52-f80e5ba69465%2FUntitled.png?id=9f7a4a11-ad74-4770-8c4d-16dcf3e76c91&table=block&spaceId=f800ef36-5d6f-4e98-a2f1-f4b2d48cc3fe&width=1000&userId=&cache=v2](https://dev-dobin.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fd9cb0b0e-6afe-41c8-aa52-f80e5ba69465%2FUntitled.png?id=9f7a4a11-ad74-4770-8c4d-16dcf3e76c91&table=block&spaceId=f800ef36-5d6f-4e98-a2f1-f4b2d48cc3fe&width=1000&userId=&cache=v2)

- 또 다른 장점으로는 코드의 중복을 억제하고 재사용성을 높히는 함수는 유지보수의 편의성을 높이고 실수를 줄여 코드의 신뢰성을 높이는 효과가 있다라고 볼 수 있다.
- 함수는 객체 타입의 값이다. 따라서 식별자를 붙일 수 있다.

### 함수 리터럴

- 자바스크립트의 함수는 객체타입이기 때문에 리터럴 사용이 가능하다 function 키워드, 함수 이름, 매개변수 목록, 함수 몸체로 구성된다.

```jsx
// 변수에 함수 리터럴을 할당할 경우
var f = function add(x,y) {
	return x+y;
};
```

- 함수 리터럴의 구성 요소

| 구성 요소 | 설명 |
| --- | --- |
| 함수이름 | - 함수 이름은 식별자다. 따라서 식별자 네이밍규칙을 준수해야 한다.
- 함수 이름은 함수 몸체 내에서만 참조할 수 있는 식별자다.
- 함수 이름은 생략할 수 있다. 이름이 있는 함수를 기명함수, 이름이 없는 함수를 무명/익명함수라 한다. |
| 매개변수 목록 | - 0개 이상의 매개변수를 소괄호로 감싸고 쉼표로 구분한다.
- 각 매개변수에는 함수를 호출할 때 지정한 인수가 순서대로 할당된다. 즉, 매개변수 목록은 순서에 의미가 있다.
- 매개변수는 함수 몸체 내에서 변수와 동일하게 취급된다. 따라서 매개변수도 변수와 마찬가지로 식별자 네이밍 규칙을 준수해야 한다. |
| 함수 몸체 | - 함수가 호출되었을 때 일괄적으로 실행될 문들을 하나의 실행 단위로 정의한 코드 블록이다.
- 함수 몸체는 함수 호출에 의해 실행된다. |
- 함수는 객체지만 일반 객체와는 다르다. 일반 객체는 호출할 수 없지만 함수는 호출할 수 있다. 그리고 일반 객체에는 없는 함수 객체만의 고유한 프로퍼티를 갖는다.

### 함수 정의

- 함수 정의에는  함수 선언문, 함수 표현식, Function 생성자 함수, 화살표함수(ES6)이렇게 4가지가 있다. (각 정의 방식에 대해선 별도로 설명함)
- 변수는 선언이라하고 함수는 정의라고 표하는게 맞다고 한다. 이유는 함수 선언문이 평가되면 식별자가 암묵적으로 생성되고 함수 객체가 할당된다.

> 이것이 무슨말인가..? 식별자까지는 이해가 되는데 왜 함수 객체가 할당된다는 거지? 그 이유는.. 1급 객체라는 것에 있다.. 프로그래밍 언어에서는 어떠한 type이 전달, 반환 및 할당 할 수 있는 경우 해당 type을 1급 객체로 간주하는데 JavaScript에서의 함수는 반환(리턴)할 수 있을 뿐만 아니라 **함수를 받을 수 있는 함수를 만들 수 있기 때문**이다..

물론, 일급객체에 대해선 18장에서 자세히 다룬다.
하지만 잠시 알아보자면 일급객체가 되려면 조건이 있는데 그것은 아래와 같다.
- 변수나 데이터 구조안에 담을 수 있다.
- 파라미터로 전달 할 수 있다.
- 리턴 값으로 사용할 수 있다.
- 무명의 리터럴로 생성 가능 즉, 런타임에 생성이 가능

어라..? 그러네.. JavaScript 함수는.. 저거 다 해당되네.. 마치 데이터 처럼..
>