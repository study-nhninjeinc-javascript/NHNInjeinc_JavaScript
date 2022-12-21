testtest✨

test2

# 객체 리터럴, 원시 값과 객체의 비교

구분: Modern JavaScript
날짜: 2022/12/20 → 2022/12/22
단원: 10, 11 Chapter

# 10.객체 리터럴

---

### 1. **객체**

---

- `원시타입`(변경불가능한 값)과 `객체타입`(변경 가능한 값)으로 구성
- 키와 값으로 구성된 프로퍼티를 0개 이상 가진 집합
※프로퍼티 값이 함수인 경우 일반 함수와 구분하기 위해 메서드라 부름
- 프로퍼티와 메서드로 구성된 집합

 

### 2. **객체생성**

---

1. 객체 리터럴 사용: 중괄호 내에 프로퍼티를 정의하는 가장 일반적인 객체 생성 방법 
    
    ```jsx
    var person = {
    	name : 'Lee',
    	sayHello : function(){
    		console.log('hello! My name is ${this.name}.');
    	}
    }
    
    console.log(typeof person); //object
    console.log(person); //{name: "Lee", sayHello: f}
    ```
    
2. 함수 사용 : object생성자함수, 생성자함수, object.create메서드, 클래스(ES6)

### 3. **************프로퍼티**************

---

식별자 역할을 하는 `키`와 `값`으로 구성

**키 :** 빈문자열 포함 모든 문자열 또는 심벌 값, 식별자 역할

1. 식별자 네이밍 규칙을 따르지 않는 키 이름은 따옴표를 사용해 묶음
    
    ```jsx
    var person = {
    	firstName: 'Ung-mo', //네이밍규칙 준수
    	'last-name': 'Lee' //네이밍규칙 안준수, SyntaxError: Unexpected token -
    	}
    ```
    
2. 키로 문자열이나 심벌 값 외의 값 사용시 문자열로 암묵적 타입변환
-키로 숫자 사용 시 내부적으로 문자열로 변환
3. 이미 존재하는 프로퍼티 키 중복 선언시 나중에 선언한 프로퍼티가 먼저 선언한 프로퍼티 덮어 씀(에러 발생x)
    
    ```jsx
    var foo = {
    	name : 'Lee',
    	name : 'Kim'
    };
    
    console.log(foo); //{name: "Kim"};
    ```
    

**값** : 모든 값 가능, 프로퍼티 값이 함수일 경우 일반 함수와 구분하기 위해 메서드라 부름

```jsx
var circle = {
	radius : 5,
	getDiameter : function(){ //<-메서드
		return 2 * this.radius; 
	}
}

console.log(circle.getDiameter()); //10
```

**프로퍼티 접근**

- 마침표 표기법(.)과 대괄표 표기법([ … ])이 있다.
    
    ```jsx
    var person = {
    	name : 'Lee'
    };
    
    //마침표 표기법
    console.log(person.name); // Lee
    //대괄호 표기법
    console.log(person['name']); //Lee
    ```
    
- **키가 식별자 네이밍 규칙을 준수하지 않는 이름이라면 반드시 대괄호표기법 사용하며, 대괄호 내의 키는 반드시 따옴표로 감싼 문자열이어야 함**
단, 키가 숫자인 경우 따옴표 생략 가능
    
    ```jsx
    var person = {
    	name : 'Lee'
    };
    
    console.log(person[name]); //ReferenceError : name is not defined
    ```
    
    → 대괄호 사용시 따옴표가 없는 경우 식별자로 해석되어 변수로 선언된 name의 값을 넣으려고 하는데 선언된 name이 없기 때문에 not defined에러 발생
    
- 선언되지 않은 키 접근시 undefined 반환

**프로퍼티 값 갱신**

이미 존재하는 프로퍼티에 값을 할당 시 값이 갱신 됨

```jsx
var person = {
	name : 'Lee'
};

person.name = 'Kim';

console.log(person); //{name: "Kim"}
```

**프로퍼티 동적 생성**

존재하지 않는 프로퍼티에 값을 할당 시 프로퍼티가 동적으로 생성되어 추가되고 값이 할당 됨

```jsx
var person = {
	name : 'Lee'
};

person.age = 20;
console.log(person); //{name: "Lee", age: 20}
```

**프로퍼티 삭제**

delete 연산자 사용, 존재하지 않는 프로퍼티 삭제 시 무시됨

```jsx
var person = {
	name : 'Lee'
};
person.age = 20;

delete person.age;
delete person.address; //무시

console.log(person); //{name: "Lee"}
```

**ES6에서 추가된 객체리터럴의 확장 기능**

1. 프로퍼티 축약 표현