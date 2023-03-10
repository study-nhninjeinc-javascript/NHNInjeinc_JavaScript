## 10장 객체 리터럴

### 객체란?

* 자바스크립트는 객체 기반의 프로그래밍 언어이며, 원시 값을 제외한 나머지 값(함수, 배열, 정규 표현식 등)은 모두 객체이다.

* 하나의 값만 나타내는 원시 타입의 값과 다르게 객체는 다양한 타입의 값(원시 값 또는 다른 객체)를 하나의 단위로 구성한 복합적인 자료구조이다. 또한 원시 값은 변경 불가능한 값이지만, 객체는 변경 가능한 값이다.

* 객체의 구성은 프로퍼티와 메서드로 구성되어 있다.
``` 
var counter = {
    num : 0, //프로퍼티(key : value)
    increase: function() { //메서드
        this.num++;
    }
} 
```

### 객체 리터럴에 의한 객체 생성

* 자바같은 클래스 기반의 객체 지향 언어는 클래스를 정의하고 필요한 시점에 new 연산자와 함께 생성자를 호출하여 인스턴스를 생성하는 방식으로 객체를 생성한다.

* 자바스크립트는 프로토타입 기반의 객체지향 언어로서 다양한 객체 생성 방법을 지원한다.

객체 리터럴
```
var person = {
    name : 'Lee'
}
```
Object 생성자 함수
```
var person = new Object();
```
생성자 함수
```
function Person(name) {
    this.name = name;
}
//일반 함수인지 생성자 함수인지 구분하기 위해, 생성자 함수의 첫 문자는 대문자로 표기하는 것이 관례
var personTest = new Person("Lee"); 
```
Object.create 메서드

클래스
```
class Person {

}
let test = new Person();
```

### 프로퍼티
```
const test = {
    name: 'test'
}
```
프로퍼티 키 : 빈 문자열을 포함하는 모든 문자열 또는 심볼 값
프로퍼티 값 : 자바스크립트에서 사용할 수 있는 모든 값

프로퍼티 키는 식별자 네이밍 규칙을 따르지 않는 이름에는 반드시 따옴표를 사용해야한다.
```
{'na-me': 'test'}
```
### 프로퍼티 접근

프로퍼티에 접근하는 방법에는 두가지가 있다.
* 마침표 프로퍼티 접근 연산자(.)를 사용하는 마침표 표기법
* 대괄호 프로퍼티 접근 연산자([...])를 사용하는 대괄호 표기법
```
var person = {
    name : 'Lee'
}

console.log(person.name)
console.log(person['name']) <- 대괄호 프로퍼티 접근 연산자내부의 프로퍼티 키는 따옴표로 감싼 문자열이어야한다
```
> 번외

.은 프로퍼티 접근 연산자로 .앞에는 객체를 가리키는 식별자가 나와야하고 뒤에는 프로퍼티가 나와야한다.
```
var test='inje';
test.testProperty = 123;
console.log(test.testProperty); //undefined
test는 객체가 아니지만 error가 나지 않는다.
```
**string 자료형을 임시 래퍼 객체로 감싼다.**

래퍼 객체 변환 과정
- 원시타입에 해당하는 객체 생성 -> 객체의 메서드 호출 -> 메서드 처리 후 객체 소멸

그러므로 console.log(test.testProperty) -> undefined

원시 타입에 프로퍼티나 메서드를 추가하고 싶다면
new 키워드를 사용하여 명시적으로 래퍼 객체를 생성하여야 한다
```
var test= new String('inje');
test.testProperty = 123;
console.log(test.testProperty); //123
```
### 프로퍼티 동적 생성

존재하지 않는 프로퍼티에 값을 할당하면 프로퍼티가 동적으로 생성되어 추가되고 프로퍼티 값이 할당된다.
```
var person = {
    name : 'Lee'
}
person.age = 25;
console.log(person) // {name:'Lee', age:25}
```
### 프로퍼티 삭제
존재하지 않는 프로퍼티를 삭제해도 에러없이 무시된다
```
delete person.age //문법
```
### es6 객체 리터럴 확장 기능
프로퍼티 축약 표현
프로퍼티 값으로 변수를 사용하는 경우 변수 이름과 프로퍼티 키가 동일한 이름일때 프로퍼티 키를 생략할 수 있다. 이떄 프로퍼티 키는 변수 이름으로 자동 생성
```
let x = 1, x = 2;
const obj = {x,y};
console.log(obj) //{x:1,y:2}
```