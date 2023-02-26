# 5_표현식과 문

### 표현식

표현식이 평가되면 **새로운 값을 생성하거나 기존 값을 참조**

```jsx
//리터럴 표현식
10
'hello'

//식별자 표현식(선언 가정)
sum
person.name
arr[1]

//연산자 표현식
10+20
sum = 10

//호출 표현식

sqaure();
person.getName();
```

## 문

프로그램을 구성하는 기본 단위이자 최소 실행 단위

```jsx
//변수 선언문
var x;

//할당문
x=1;  // 자체가 표현식이지만 완전문한 문이기도 하다 

//if문 
if (x>1){console.log(x)}

```

표현식과 문을 구별하는 법은 변수에 할당해보는 것 

```jsx
var foo = var x ; // StntaxError
```