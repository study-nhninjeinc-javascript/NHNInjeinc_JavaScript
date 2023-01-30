## 생성자 함수에 의한 객체 생성

### Object 생성자 함수
```
// 빈 객체의 생성
const person = new Object();

// 프로퍼티 추가
person.name = 'lee';
person.sayHello = function () {
    console.log('name ' + this.name);
}

console.log(person); // {name : 'lee', sayHello : f}
person.sayHello();  // name lee
```
* 생성자 함수란 new 연산자와 함께 호출하여 객체를 생성하는 함수를 말한다. 생성자 함수에 의해 생성된 객체를 인스턴스라고 한다.
* 자바스크립트는 Object 생성자 함수 외에도 String, Number, Boolean, Array, Date 등의 빌트인 함수를 제공한다.
> 빌트인 객체 -> ECMAScript 사양에 정의된 객체를 뜻하며, 애플리케이션 전역의 공통 기능을 제공한다.

### 생성자 함수
* 객체 리터럴에 의한 객체 생성 방식의 문제점
> 객체 리터럴에 의한 객체 생성 방식은 간편하다. 하지만 객체 리터럴에 의한 객체 생성 방식은 하나의 객체만 생성한다. 그러므로 동일한 프로퍼티를 갖는 객체를 여러 개 생성해야 하는 경우 매번 같은 프로퍼티를 기술해야 하기 때문에 비효율적이다.
```
const circle1 = {
    radius: 5,
    getDiameter() {
        return 2 * this.radius;
    }
}

const circle2 = {
    radius: 10,
    getDiameter() {
        return 2 * this.radius;
    }
}
```
> 객체는 프로퍼티를 통해 객체 고유의 상태를 표현한다. 그리고 메서드를 통해 상태 데이터인 프로퍼티를 참조하고 조작하는 동작을 표현한다. 따라서 프로퍼티는 객체마다 프로퍼티 값이 다를 수 있지만 메서드는 동일한 경우가 일반적이다.

* 생성자 함수에 의한 객체 생성 방식의 장점
> 생성자 함수에 의한 객체 생성 방식은 객체를 생성하기 위한 템플릿처럼 생성자 함수를 사용하여 프로퍼티 구조가 동일한 객체 여러 개를 간편하게 생성할 수 있다.
```
function Circle(radius) {
    this.radius = radius;
    this.getDiameter = function () {
        return 2 * this.radius;
    }
}

const c1 = new Circle(5);
const c2 = new Circle(10);

console.log(c1.getDiameter()) //10
console.log(c2.getDiameter()) //20
```

* this는 객체 자신의 프로퍼티나 메서드를 참조하기 위한 자기 참조 변수다. this가 가리키는 값, this 바인딩을 함수 호출 방식에 따라 동적으로 결정된다.

|함수 호출 방식|this가 가리키는 값(this 바인딩)|
|:--|:--|
|일반 함수로서 호출|전역 객체|
|메서드로서 호출|메서드를 호출한 객체(마침표 앞의 객체)|
|생성자 함수로서 호출|생성자 함수가(미래에) 생성할 인스턴스|
