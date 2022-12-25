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
| 함수이름 | → 함수 이름은 식별자다. 따라서 식별자 네이밍규칙을 준수해야 한다.<br>→ 함수 이름은 함수 몸체 내에서만 참조할 수 있는 식별자다.<br>→ 함수 이름은 생략할 수 있다. 이름이 있는 함수를 기명함수, 이름이 없는 함수를 무명/익명함수라 한다. |
| 매개변수 목록 | → 0개 이상의 매개변수를 소괄호로 감싸고 쉼표로 구분한다.<br>→ 각 매개변수에는 함수를 호출할 때 지정한 인수가 순서대로 할당된다. 즉, 매개변수 목록은 순서에 의미가 있다.<br>→ 매개변수는 함수 몸체 내에서 변수와 동일하게 취급된다. 따라서 매개변수도 변수와 마찬가지로 식별자 네이밍 규칙을 준수해야 한다. |
| 함수 몸체 | → 함수가 호출되었을 때 일괄적으로 실행될 문들을 하나의 실행 단위로 정의한 코드 블록이다.<br>→ 함수 몸체는 함수 호출에 의해 실행된다. |<br>→ 함수는 객체지만 일반 객체와는 다르다. 일반 객체는 호출할 수 없지만 함수는 호출할 수 있다. 그리고 일반 객체에는 없는 함수 객체만의 고유한 프로퍼티를 갖는다.

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

### 함수 선언문

```jsx
// 방식
function add(x,y) {
	return x + y;
}

console.dir(add);
// console.dir은 함수 객체의 프로퍼티까지 출력한다.
// 단, Node.js 환경에서는 console.log와 같은 결과가 출력
```

- 함수 선언문은 함수 이름을 생략할 수 없다.
- 함수 선언문과 함수 리터럴 표현식은 함수 객체를 생성한다는 점에서 동일하지만 호출에 차이가 있다.

```jsx
// 아래와 같은 코드가 있을 때
(function bar() {console.log('bar'); });
bar(); // ReferenceError : bar is not defined
```

![https://dev-dobin.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Faf1bbd1d-6d55-4a9c-922f-bf6c1dafa73a%2FUntitled.png?id=470e5dc4-7861-4c27-bcb4-c395da4d0f32&table=block&spaceId=f800ef36-5d6f-4e98-a2f1-f4b2d48cc3fe&width=1030&userId=&cache=v2](https://dev-dobin.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Faf1bbd1d-6d55-4a9c-922f-bf6c1dafa73a%2FUntitled.png?id=470e5dc4-7861-4c27-bcb4-c395da4d0f32&table=block&spaceId=f800ef36-5d6f-4e98-a2f1-f4b2d48cc3fe&width=1030&userId=&cache=v2)

- ( )그룹 연산자를 통해 생성한 함수 리터럴 bar는 함수 선언문으로 해석되지 않고 함수 리터럴 표현식으로 해석되며, 함수 이름 bar는 함수 몸체 내에서만 참조할 수 있는 식별자이므로 외부(그룹 연산자 밖)에서는 식별자가 없어서 호출할 수 없다.
- 반대로 함수 선언문으로 정의된 foo라는 함수가 있을 때 호출할 수 있는 이유는 아래와 같다.

![https://dev-dobin.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F1f130410-ceb3-42fd-97c7-650319d0457e%2FUntitled.png?id=7cf5523b-ac82-4dba-bec3-1366a7f6fed1&table=block&spaceId=f800ef36-5d6f-4e98-a2f1-f4b2d48cc3fe&width=860&userId=&cache=v2](https://dev-dobin.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F1f130410-ceb3-42fd-97c7-650319d0457e%2FUntitled.png?id=7cf5523b-ac82-4dba-bec3-1366a7f6fed1&table=block&spaceId=f800ef36-5d6f-4e98-a2f1-f4b2d48cc3fe&width=860&userId=&cache=v2)

- 함수 선언문을 변수에 할당하면 어떻게 될까? 아래 그림처럼 자바스크립트 엔진은 변수명을 식별자로 사용하게 된다. 그리고 아래와 같은 경우를 **의사코드**라고 한다.

![https://dev-dobin.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F15b98ab6-8e2d-43e4-8d3f-e6c63f8d4c11%2FUntitled.png?id=8a76ec37-d991-4e56-8cc8-87d1e7245d0a&table=block&spaceId=f800ef36-5d6f-4e98-a2f1-f4b2d48cc3fe&width=980&userId=&cache=v2](https://dev-dobin.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F15b98ab6-8e2d-43e4-8d3f-e6c63f8d4c11%2FUntitled.png?id=8a76ec37-d991-4e56-8cc8-87d1e7245d0a&table=block&spaceId=f800ef36-5d6f-4e98-a2f1-f4b2d48cc3fe&width=980&userId=&cache=v2)

### 함수 표현식

- 함수는 일급 객체이므로 함수 리터럴로 생성한 함수 객체를 변수에 할당할 수 있다. 이러한 함수 정의 방식을 표현식이라 한다.

```jsx
// 함수 표현식
var add = function(x,y) {
	return x+y;
}
```

- 함수 리터럴의 함수 이름은 생략할 수 있다. 이러한 함수를 익명 함수라 한다. 그리고 함수 표현식의 함수 리터럴은 함수 이름을 생략하는 것이 일반적이다.

```jsx
// 기명 함수 표현식
var add = function foo(x,y) {
	return x + y;
}

// 함수 객체를 가리키는 식별자로 호출
console.log(add(1,2)); // 3
// 함수 이름으로 호출하면 함수 몸체 내부에서만 유효함
console.log(foo(1,2)); // ReferenceError : foo is not defined

// 객체와 같은 접근자로도 불가능하다.
// 함수 몸체 내부 == 할당 시점
cconsole.log(add.foo(1,2));
console.log(add['foo'](1,2));
```

### 함수 생성 시점과 함수 호이스팅

```jsx
add(1,2);
sub(1,2);

function add(x,y) {
	return x + y;
}

var sub = function (x,y) {
	return x - y;
};

// 실행결과 : TypeError: sub is not a function
```

- 위 와 같이 함수 표현식으로 정의한 함수는 함수 표현식 이전에 호출할 수 없다. 
이는 함수 선언문으로 정의한 함수와 함수 표현식으로 정의한 함수의 생성 시점이 다르기 때문이다.
- 코드가 한 줄씩 순차적으로 실행되기 시작하는 런타임에는 이미 함수 객체가 생성되어 있고 함수 이름과 동일한 식별자에 할당까지 완료되었기 때문에 add()함수는 선언문 이전에 함수를 참조할 수 있고, 호출을 할 수 있다. 이처럼 함수 선언문이 코드의 선두로 끌어 올려진 것처럼 동작하는 자바스크립트 고유의 특징을 **함수 호이스팅**이라 한다.

### 호이스팅에서 변수와 함수 선언문, 함수 표현식의 관계

- var 키워드를 사용한 변수 선언문과 함수 선언문은 런타임 이전에 자바스크립트 엔진에 의해 먼저 실행되어 식별자를 생성한다는 점에서 동일하다.
- 하지만 var 키워드로 선언된 변수는 undefined로 초기화되고, 함수 선언문을 통해 암묵적으로 생성된 식별자는 함수 객체로 초기화된다.
- 따라서 var 키워드를 사용한 변수 선언문 이전에 변수를 참조하면 변수 호이스팅에 의해 undefined로 평가 되지만 함수 선언문으로 정의한 함수를 함수 선언문 이전에 호출하면 함수 호이스팅에 의해 호출이 가능하다.
- 변수 할당문의 값은 할당문이 실행되는 시점, 즉 런타임에 평가되므로 함수 표현식의 함수 리터럴도 할당문이 실행되는 시점에 평가되어 함수 객체가 된다.
- 따라서 함수 표현식으로 함수를 정의하면 함수 호이스팅이 발생하는 것이 아니라 변수 호이스팅이 발생한다.

![https://dev-dobin.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F8dece6db-ccb4-4694-aeea-0a8513e3b100%2FUntitled.png?id=38b45f67-6678-41f7-9f65-cbdd2082a905&table=block&spaceId=f800ef36-5d6f-4e98-a2f1-f4b2d48cc3fe&width=1340&userId=&cache=v2](https://dev-dobin.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F8dece6db-ccb4-4694-aeea-0a8513e3b100%2FUntitled.png?id=38b45f67-6678-41f7-9f65-cbdd2082a905&table=block&spaceId=f800ef36-5d6f-4e98-a2f1-f4b2d48cc3fe&width=1340&userId=&cache=v2)


<code>💡 <strong>정리</strong> <br>
먼저 변수 호이스팅을 보면 변수 호이스팅은 코드의 실행순서와 상관없이 선언된 변수들을 마치 해당 선언문이 실행된 것 처럼 자바스크립트 엔진이 식별자를 생성한다.
그런데 이 시점에서 var 키워드는 선언과 초기화 단계가 동시에 실행되는 점과 
모든 변수는 초기화 단계 때 undefined로 초기화가 된다는 점이 있다.
그렇기에 변수 호이스팅이 발생한 시점에서 이미 var로 선언된 변수들은 undefined 값을 갖고 있는 것이다. <br>
<br>
그래서 위 예제 처럼 함수 표현식의 값을 갖는 sub라는 변수를 선언 이전에 sub(1,2); 와 같이 함수 호출형태가 아닌 console.log(sub)와 같이 단순히 변수를 출력해보면 해당 변수는 undefined라는 값을 갖고 있기 때문에 아무런 에러가 발생하지 않는다.<br>
그리고 함수 표현식은 값이기 때문에 할당 문(코드 실행 시점)이 실행이 되어야 비로소 sub라는 변수에 값(함수)이 할당되어 정상동작을 할 수 있게 되는것이다. <br>
<br>
그리고 함수 호이스팅은 호이스팅이 발생한 순간 코드 실행 전에 식별자가 생성되고 함수 객체 또한 생성된다. 이 특이점은 변수 호이스팅과 비교된다. 왜냐하면 변수 호이스팅은 선언문 이전에 참조만 될 뿐이지 할당 값도 같이 생성해 두진 않는다.(물론 var 키워드 때문에 undefined라는 값을 갖고 있기는 하다만 이건 var 키워드의 특성이다.) 그런데 함수 호이스팅은 함수 선언문을 만나는 순간 그 함수의 모든 것을 생성해버리기 때문에 선언 전(코드 실행 전) 호출하면 해당 함수가 동작이 되어버린다.<br>
<br>
그리고 또 다른 특이점은 함수 표현식의 경우엔  변수의 할당 값으로 취급되기에 함수 표현식은 함수 호이스팅이 아닌 변수 호이스팅이 적용된다는 점이다.<br>
<br>
<strong>(오해하기 전에 참고)</strong><br>
let과 const는 var 키워드와 다르게 호이스팅으로 인해 변수 선언전(코드 실행 전) 참조를 방지하기 위해 ES6부터 도입된 키워드이다. let과 const도 호이스팅을 거치지만 변수 선언문을 만나기 전까지 일시적 사각지대 (TDZ)라는 곳에 빠지기 때문에 코드 실행 전(선언문) 참조를 하면 ReferenceError가 발생한다. 그래서 선언단계와 초기화단계가 분리되어 실행된다 볼 수 있다. 그렇기 때문에 위 코드를 다시 예로 들면 var였을 때는 이미 undefined로 초기화 되어 버려서 sub(1,2)와 같이 함수 호출형태를 하면 sub는 undefined인데 함수 처럼 호출하려고 하니 TypeError가 발생하고, sub 변수를 만약 let으로 바꾼다면 console.log(sub) 처럼 참조를 하던 sub(1,2); 처럼 호출을 하던 let은 선언 문(코드 실행)문을 만날 때 까지 일시적 사각지대에 빠져있기 때문에 식별자가 생성이 안되어서 ReferenceError가 발생한다.  다시 말하자면 var는 선언문 이전에 참조는 되었으나 함수 표현식이 할당이 안되어서 TypeError가 발생한 것 이고, let은 선언문 이전에 참조를 방지하는 키워드이기 때문에 ReferenceError가 발생한 것 이다.<br>
<br>
var, let, const과 위에서 언급한 TDZ는 15장에서 자세히 다루자.

</code>

### Function 생성자 함수

- Function 생성자는 자바스크립트가 기본 제공하는 빌트인 함수(내장 객체)이다.
- Function 생성자 함수에 매개변수 목록과 함수 몸체를 문자열로 전달하면서 new 연산자와 함께 호출하면 함수 객체를 생성해서 반환한다.


<code>💡 생성자 함수는 객체를 생성하는 함수를 말한다. 객체를 생성하는 방식은 객체 리터럴 이외에 다양한 방법이 있다. 생성자 함수에 대해서는 17장을 학습할 때 자세히 알아보자
그리고 new 연산자에 대해선 <a href="https://velog.io/@jakeseo_me/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EA%B0%9C%EB%B0%9C%EC%9E%90%EB%9D%BC%EB%A9%B4-%EC%95%8C%EC%95%84%EC%95%BC-%ED%95%A0-33%EA%B0%80%EC%A7%80-%EA%B0%9C%EB%85%90-16-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-new-%EC%97%B0%EC%82%B0%EC%9E%90-sojvdjln1q" targer="_blank">이곳</a>을 확인하자

</code>

```jsx
// Function 생성자 함수로 함수를 생성
var add = new Function('x', 'y', 'return x + y');

console.log(add(2,5)); // 7
```

- Function 생성자 함수로 함수를 생성하는 방식은 일반적이지 않으며 바람직하지도 않다. 그리고 이렇게 생성된 함수는 클로저(24장에서 배움)를 생성하지 않는 등, 함수 선언문이나 함수 표현식으로 생성한 함수와 다르게 동작한다.
- 지금은 아직 생성자 함수와 클로저에 대해 자세히 모르기 때문에 함수 선언문이나 함수 표현식으로 생성한 함수와 Function 생성자 함수로 생성한 함수가 동일하게 동작하지 않는다는 것만 기억해두자.

### 화살표 함수

- ES6부터 도입됨
- 사용법은 function 키워드 대신 화살표 ( ⇒ )를 사용해 좀 더 간략한 방법으로 함수를 선언할 수 있으며, 화살표 함수는 항상 익명 함수로 정의한다.
- 화살표 함수는 기존의 함수 선언문 또는 함수 표현식을 완전히 대채하기 위해 디자인 된 것이 아니며, 화살표 함수는 기존의 함수보다 표현한 간략한 것이 아니라 내부 동작 또한 간략화 되어 있다.
- 특이점으로는 생성자 함수로 사용할 수 없으며, 기존 함수와 this 바인딩 방식이 다르고, prototype 프로퍼티가 없으며 arguments 객체를 생성하지 않는다. 이 3가지에 대해선 아직 모르니 26장 화살표 함수에서 자세히 살펴보자

### 매개변수와 인수

- 인수는 함수를 호출할 때 지정하며, 개수와 타입에 제한이 없다.
- 매개변수는 함수를 정의할 때 선언하며, 함수 몸체 내부에서 변수와 동일하게 취급되기 때문에 함수가 호출되면 함수 몸체 내에서 암묵적으로 매개변수가 생성되고 일반 변수와 마찬가지로 undefined로 초기화 된 이후 인수가 순서대로 할당 됨
- 매개변수 보다 초과된 인수는 그냥 버려지지 않고 모든 인수는 암묵적으로 arguments 객체의 프로퍼티로 보관된다.
- arguments 객체는 함수를 정의할 때 매개변수 개수를 확정할 수 없는 가변 인자 함수를 구현할 때 유용하게 사용된다. arguments 객체대해서는 18장에서 자세히 살펴보자

### 인수 확인

```jsx
function add(x, y) {
	return x + y;
}

console.log(add(2)); // NaN
console.log(add('a', 'b')); // 'ab'
```

- 위 코드는 자바스크립트 문법상 어떠한 문제도 없다. 이러한 상황이 발생한 이유는 다음과 같다.
    - 자바스크립트 함수는 매개변수와 인수의 개수가 일치하는지 확인하지 않는다.
    - 자바스크립트는 동적 타입 언어다. 따라서 자바스크립트 함수는 매개변수의 타입을 사전에 지정할 수 없다.
- 따라서 자바스크립트의 경우 함수를 정의할 때 적절한 인수가 전달되었는지 확인할 필요가 있다.
- 타입스크립트와 같은 정적 타입을 선언할 수 있는 상위 확장을 도입하거나, typeof, arguments, 단축평가 등을 활용하여 매개변수를 확인하는 방법이 있는데 ES6에서 부터 도입된 **매개변수 기본값**을 사용하면 함수 내에서 수행하던 인수 체크 및 초기화를 간소화할 수 있다. 단, 매개변수 기본값은 매개변수에 인수를 전달하지 않았을 경우와 undefined를 전달한 경우에만 유효하다.

```jsx
// ES6 매개변수 기본값 사용법
function add(a = 0, b = 0, c = 0) {
	return a + b + c;
}

console.log(add(1, 2, 3)); // 6
console.log(add(1, 2)); // 3
console.log(add(1)); // 1
consoel.log(add()); // 0
```

### 매개변수의 최대 개수