# 객체

- 객체의 원시타입은 단 하나의 값을 나타낸다
- 객체의 객체타입은 다양한 타입의 값을 하나의 단위로 구성한 복합적인 구조이다. 
- 객체는 0개 이상의 프로퍼티로 구성된 집합이고 프로퍼티는 키와 값으로 구성된다.

```jsx
 var person = {
    
    name : ‘Lee’,  // 프로퍼티
    age : 20       // 프로퍼티
   
    };
```

 - 프로퍼티란 자바스크립트에서 사용할수 있는 모든(객체의 상태를 나타내는) 값을 말한다.
 - 메서드란 프로퍼티값이 함수일경우 구분하거나 프로퍼티를 참조하고 조작할 수 있는 동작하는것을 의미한다.
 
 ```jsx
 var counter = {
    num : 0,                // 프로퍼티
    increase: function(){   // 메서드
      this.num++;
    } 
  };
```

# 객체 리터럴에 의한 객체 생성

 - 객체 리터럴은 중괄호 내에 0개 이상의 프로퍼티를 정의한다.
 
```jsx
    var person = {
    
    name : ‘Lee’,
    
    sayHello : function() {
    
    console.log(’Hello! ${this.name}’);
    
    }
    
    };   // 값이 있는 개체      
    
    console.log(typeof person); //object
    
    console.log(person); //{name: 'Lee', sayHello: ƒ}
    
    var empty = {};  //빈 객체
```
    
    

# 프로퍼티

- 객체는 프로퍼티의 집합이며, 프로퍼티는 키와 값으로 구성된다.
```jsx
   var person = {
   name : 'Lee', // 프로퍼티 키는 name , 프로퍼티 값은 'Lee'
   age : 20 // 프로퍼티 키는 age , 프로퍼티 값은 '20'
   }
```
    

# 메서드

- 메서드 : 프로퍼티 값이 함수일 경우 일반함수와 구분하기 위한것 

```jsx
var circle = {
   radius : 5, // 값인 프로퍼티
  
  
   getDiameter : function(){ //함수 인 메서드
       return 2*this.radius; 
   }
};
   console.log(circle.getDiameter());
```

# 프로퍼티 접근

- 프로퍼티 접근법으론 연산자를 사용하는 마침표 표기법 , 댈괄호를 사용하는 대괄호 표기법으로 나뉜다.
```jsx
var person = {
   name : 'Lee'

};
   console.log(person.name); // 마침표 표기법
  consolse.log(person['name']) // 대괄호 표기법
```

# 프로퍼티 값 갱신 , 동적 생성, 삭제

- 프로퍼티에 값을 할당하면 프로퍼티 값이 갱신된다.

```jsx
var person = {
   name : 'Lee'

};
  person.name = 'Kim';
  
  consolse.log(person) // [name : 'Kim']
```

- 존재하지 않는 프로퍼티에 값을 할당하면 프로퍼티가 동적으로 생석되 값을 할당 할 수 있다.

```jsx
var person = {
   name : 'Lee'

};
  person.age = '20';
  
  consolse.log(person) // [name : '20']
```
- delete 연산자를 통해 프로퍼티를 삭제한다. 이떄 프로퍼티 값에 접근할수 있는 표현식이여야 한다.

```jsx
var person = {
   name : 'Lee'

};
  person.age = '20';
  
  delete person.age; // person 객체에 age 프로퍼티가 존재해 삭제가 된다.
  
  delete person.address; // person 객체에 address 프로퍼티가 존재하지 않아 삭제할수 없다.
  
  consolse.log(person) // [name : 'Lee']
```

# ES6에서 추가된 객체 리터럴의 확장 기능

-프로퍼티값으로 변수를 사용하는 경우 변수 이름과 프로퍼티 키가 동일한 이름일때 프로퍼티 키를 생략 할 수 있다.
   
```jsx
    let x = 1, y =2;
    
    const obj = {
        x  : x,
        y : y
    
    };  
    
    //const obj = {x,y};
    
    console.log(obj);//{x: 1, y: 2}
 ```   

- 프로퍼티이름으로 프로퍼티 키를 동적 생성할수 있고 생성시 객체 리터럴 외부에 대괄호 표기법을 사용해야한다.
 
```jsx
    const prefix = 'prop';
    
    let i = 0;
    
    //객체 리터럴 내부에서 생성된 프로퍼티 이름으로 프로퍼티 키를 동적 생성
    
    const obj = {
    
    ['${prefix}-${++i}'] : i,
    
    ['${prefix}-${++i}'] : i,
    ['${prefix}-${++i}'] : i,
    
    };
```    

- 메서드를 정의할때 function 키워드를 생략한 축약 표현을 사용할 수 있다.
```jsx
   const obj = {
    name : 'Lee',
    sayHi(){  //메서드 축약 표현
      console.log('Hi!' +this.name);
    }
   };
   obj.sayHi() // Hi! Lee
```    