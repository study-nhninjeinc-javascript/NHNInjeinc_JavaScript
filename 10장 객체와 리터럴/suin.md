10장객체와 리터럴

10.1 객체란?
- 자바스크립트는 객체기반의 프로그래밍언어
- 자바스크립트를 구성하는 거의 "모든 것"이 객체, 원시값을 재외한 나머지(함수,배열, 정규표현식 등) 값은 모두 객체
- 객체타입은 다향한 타입의 값을 하나의 단위로 구성한 "복합적인 자료구조"
- 원시 값 : 변경 불가능 / 객체 값 : 변경 가능
- 0개 이상의 프로퍼티로 구성된 집합, 프로퍼티는 키와 값으로 구성
- 자바스크립트의 함수는 일급객체이므로 값으로 취급할 수 있다. - 프로퍼티 값으로 사용 가능
  프로퍼티의 값이 함수일경우 메서드라고 부른다 - 일반함수와 구분을 위해

var person = {
    name : 'Lee',           //프로퍼티
    age : 27,               //프로퍼티
    newYear : fucntion() {  // 메서드
        this.age++;
    }
};

프로퍼티 키 - name, age
프로퍼티 값 - 'Lee', 27
메서드 - newYear

- 프로퍼티 역할 : 객체의 상태를 나타내는 값(data)
- 메서드 역할 : 프로퍼티를 참조하고 조작할수 있는 동작

10.2 객체리터럴에 의한 객체생성
- 자바스크립트는 "프로토타입" 기반 객체지향언어로서 다양한 객체 생성 방법을 지원
  객체리터럴 / Object 생성자 함수 / 생성자 함수 / Object.create 메서드 / 클래스(ES6)..
- 객체 생성 방법 중 가장 일반적이고 간단한 방법
- 리터럴 : 사람이 이해할 수있는 문자 또는 약속된 기호를 사용하여  값을 생성하는 표기법 / 객체리터럴 : 객체를 생성하기 위한 표기법
- 중괄호 내에 0개 이상의 프로퍼티를 정의
- "변수에 할당되는 시점"에 자바스크립트 엔진은 "객체리터럴을 해석"해 객체 생성
- 객체리터럴 외의 객체 생성 방식은 모두 함수를 통해 생성

var person = {
    name : "Lee",
    sayHi : function(){
        console.log("Hello my name is ${this.name}");
    } //세미콜론 X
}; // 세미콜론 O

- 객체리터럴의 중괄호는 코드블록을 의미하지 X
  코드블록 - 중괄호 뒤에 세미콜론 X, but 객체리터럴은 값으로 평가 - 중괄호 뒤 세미콜론 O

var empty = {};
console.log(typeof person); //object
console.log(person);        //{name: 'Lee', sayHi: ƒ}
console.log(typeof empty);  //object

10.3 프로퍼티
- 객체는 프로퍼티의 집합, 프로퍼티는 키와 값으로 구성
- 프로퍼티를 나열할때는 쉼표로 구분
- 키와 값으로 사용할 수 있는 값
  프로퍼티 키 : 빈문자열을 포함하는 모든 문자열 또는 심벌 값
  프로퍼티 값 : js에서 사용할 수 있는 모든 값
- 프로퍼티 키 - 프로퍼티 값에 접근할수있는 이름, 식별자 역할
  식별자 네이밍 규칙을 따르지 않는 이름에는 반드시 따옴표가 필요

var person = {
    firstName : "Suin",
    'last-name' : "Lee"
};
console.log(person); //{firstName: 'Suin', last-name: 'Lee'}

var person = {
    firstName : "Suin",
    last-name : "Lee"
};                   //Uncaught SyntaxError: Unexpected token '-'

-- 쭉 예제 돌려 봄

10.6 프로퍼티 값 갱신
이미 존재하는 프로퍼티에 값을 할당하면 프로퍼티 값 갱신
var person = {name : "Lee"};
person.name = "Choi";
console.log(person); //{name: 'Choi'}

10.8 프로퍼티 삭제
delete - 연산자 삭제
존재하지 않는 프로퍼티 - 에러없이 무시
 var person = {name : "Lee"};
 delete person.name;
 delete person.test;
 console.log(person); //{}

 10.9 ES6에서 추가된 객체 리터럴의 확장기능
 - 프로퍼티 축약 표현
   var x = 1, y = 2;
   var obj = {x : x, y : y}
   console.log(obj); //{x: 1, y: 2}
   //축약표현
   var obj2 = {x,y}
   console.log(obj2); //{x: 1, y: 2}
 - 계산된 프로퍼티이름
   리터럴 내부에서도 계산된 프로퍼티 이름으로 프로퍼티키를 동적 생성할 수 있다.
   const prefix = 'prop';
   let i = 0;
   const obj = {
        '${prefix}-${++i}' : i,
        '${prefix}-${++i}' : i,
        '${prefix}-${++i}' : i,
   };
   console.log(obj);
 - 메서드 축약 표현
   function 키워드 생략 가능
   const obj = {
     name : 'Lee',
     sayHello(){
        console.log("hello "+this.name);
     }
   }
   obj.sayHello(); //hello Lee


-- 개인적인 궁금사항 
원시값?
"기본타입"의 값
Number, String, Boolean, null, undefined 다섯가지 타입을 말함
반대로 객체는 "참조타입"이라고 말할 수 있음

프로퍼티?
객체의 속성을 정의하는 변수

일급객체?
다른 객체들이 일반적으로 적용 가능한 연산을 모두 지원하는 객체
18장에서 다룬다고 써있음

인스턴스?
클래스에 의해 생성되어 메모리에 저장된 실체

ES6
