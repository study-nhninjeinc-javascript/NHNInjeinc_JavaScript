# 11. 원시 값과 객체의 비교

---

자바스크립트가 제공하는 숫자, 문자열, 불리언, null, undefined, 심벌, 객체타입의 7가지 데이터타입은 원시타입과 객체타입으로 나뉜다.

`원시 타입(primitive)`

- 변경 불가능한 값
- 변수에 값 할당 시 메모리공간에 실제 값 저장
- 원시 값 가지는 변수를 다른 변수에 할당 시 원본의 원시 값이 복사 되어 전달(=값에 의한 전달)

`객체 타입(object/reference type)`

- 변경 가능한 값
- 변수에 값 할당시 메모리공간에는 참조 값이 저장
- 객체를 가리키는 변수를 다른 변수에 할당 시 원본의 참조 값이 복사되어 전달(=참조에 의한 전달)

### 1. 원시 값

---

**변경 불가능한 값(불변성)**

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9b597d85-4153-46f8-b64d-f5c70abfafe8/Untitled.png)

- 원시 값은 변경 불가능한 값으로 직접 변경 불가. 따라서 변수 값 변경을 위해 원시 값을 **재 할당**하면 **새로운 메모리 공간**을 확보하고 재 할당한 값을 저장한 후, 변수가 참조하던 메모리 공간의 **주소를 변경**한다.
- 변수와 반대 개념인 상수의 경우 값 저장을 위해 메모리가 필요하다는 점에 있어 변수와 유사하지만 한번만 할당이 가능하다는 점에 있어 차이를 가진다.

**문자열과 불변성**

- 문자열은 0개 이상의 문자로 이루어진 집합으로, 크기가 미리 정해진 다른 원시 타입과 달리 몇 개의 문자로 구성되어 있느냐에 따라 필요한 메모리 공간의 크기가 결정됨
- 자바스크립트는 원시 타입인 문자열 타입을 제공함.
- `유사배열객체`
원시 타입인 문자열을 객체 타입 중 하나인 배열과 유사하게 사용 할 수 있는 자바스크립트의 특징이다.

```jsx
var str = "string";

//문자열은 유사 배열이므로 배열과 유사하게 인덱스를 사용해 각 문자에 접근할 수 있다.
console.log(str[0]); //s

//원시 값인 문자열이 객체처럼 동작한다.
console.log(str.length); //6
console.log(str.toUpperCase()); //STRING

//하지만 원시 타입 이기 때문에 값 변경 불가.(에러는 발생 안됨)
str[0] = 'S';
console.log(str); //string
```

**값에 의한 전달**

변수에 원시 값을 갖는 변수 할당 시, 두 변수의 원시 값은 서로 다른 메모리 공간 주소 값을 가지고 있기 때문에 **어느 한쪽에서 재 할당을 통해 값을 변경하더라도 서로 간섭할 수 없다.**

```jsx
var score = 80;
var copy = score;

console.log(score); //80
console.log(copy); //80

score = 100;

console.log(score); //100
console.log(copy); //80
```

재할당 시 동작 방식 1 - 애초부터 다른 메모리 주소 값으로 저장

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d06df09e-763f-4306-94aa-63b83280d972/Untitled.png)

재할당 시 동작 방식 2 - 같은 메모리 주소 참조하다가 재 할당 시 다른 주소 값으로 변경

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b8dff49b-fd5e-4827-8a32-ea1145d7cba1/Untitled.png)

### 2. 객체

---

**변경 가능한 값**

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/74974bf3-efbe-45ec-bc39-19c41e07d530/Untitled.png)

- 메모리에 저장된 값에 직접 접근하는 원시 타입과 달리 객체는 메모리에 저장된 참조 값을 통해 실제 객체에 접근함.
- 값 변경을 위해 재 할당하는 원시 타입과 달리 객체는 메모리에 저장된 객체를 직접 수정할 수 있다. 또한 재 할당을 하지 않고 변경하기 때문에 객체를 할당한 변수의 참조 값도 변경되지 않는다.

**얕은 복사와 깊은 복사**

`얕은 복사`

- 한 단계만 복사
- 원본과 복사 값이 메모리에 같은 참조 값(=주소)을 가지게 복사한 경우로, 원본에서 프로퍼티 수정 시 복사 값에서도 값이 수정 값으로 변경되어 출력

`깊은 복사`

- 객체에 중첩되어 있는 객체 까지 모두 복사
- 원본과 복사 값이 메모리에 다른 참조 값을 가진 경우로, 원본에서 프로퍼티 수정 시 복사 값은 값 변동 없이 원래 값으로 출력

```jsx
const o = {x : {y : 1}};

//얕은 복사
const c1 = { ... o} ; //스프레드 문법 참고
console.log(c1 === o); //false
console.log(c1.x === o.x); //true //중첩된 객체까지는 다른 주소값 가지게 복사 안됨

//깊은 복사
const _ = require('lodash'); //lodash 의 cloneDeep을 사용한 깊은 복사
console c2 = _.cloneDeep(o); 
console.log(c2 === o); //false
console.log(c2.x === o.x); //false //중첩된 객체도 다른 주소값 가지게 복사됨 !
```

**참조에 의한 전달**

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b9428235-14da-45a0-a06e-470e57ad70df/Untitled.png)

person과 copy는 저장된 메모리 주소는 …F2 ….524로 다르지만 메모리 안에 저장된 값은 동일한 참조값을 가진다. 이는 두 식별자가 하나의 객체를 공유하는 것으로 원본 또는 사본 중 어느 한쪽에서 객체를 변경하면 서로 영향을 주고 받는다.

```jsx
var person = {
	name : 'Lee'
};

var copy = person; //얕은 복사=동일한 참조값 가짐
console.log(copy === person); //true

copy.name = 'Kim'; //객체 변경
person.address = 'Seoul'; //객체 변경

//copy와 person은 동일한 참조값을 가지므로 서로 영향을 주고 받음
console.log(person); //{name:"kim", address:"Seoul"}
console.log(copy);   //{name:"kim", address:"Seoul"}
```

정리 참고