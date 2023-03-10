# 12장 함수

## 12.1 함수란?

- 자바스크립트에서 가장 중요한 핵심개념.
- 또 다른 핵심개념인 스코프, 실행컨텍스트 클로저 생성자 함수에 의한 객체 생성, 메서드, this, 프로토타입, 모듈화 등이 모두 함수와 깊은 관련이 있다.
- 수학의 함수 - 입력을 받아 출력을 내보내는 일련의 과정
프로그래밍 언어의 함수도 수학의 함수과 같은 개념이다.
- **프로그래밍언어의 함수는 일련의 과정을 문(statement)으로 구현하고 코드블록으로 감싸서 하나의 실행단위로 정의한 것이다.**
- **매개변수**(parameter) : 함수내부로 입력을 전달받는 변수
**인수**(argument) : 입력
**반환값**(return value) : 출력
- 함수는 값이며, 여러개 존재 할 수 있다. 특정함수를구별하기 위하여 식별자인 **함수이름**을 사용할수있다.
- 함수는 **함수정의**를 통해 생성한다.
- 인수를 매개변수를 통해 함수에 전달하면서 함수의 실행을 명시적으로 지시 - **함수호출**이라고 한다.
- 함수를 호출하면 코드 블록에 담긴 문들을 실행하고 반환값을 반환한다.

```jsx
//함수정의
function add(x, y){ //함수이름 : add 매개변수 : x, y
	return x + y;//반환값 : x + y
}

//함수호출
add(2, 5);//인수 2, 5
//return 값은 7
```

---

## 12.2 함수를 사용하는 이유

- 함수는 필요할때 여러번 호출할 수 있다. - 실행 시점 결정 가능, 재사용가능
- 동일한 작업을 반복적으로 수행해야 한다면 미리 정의된 함수를 재사용하는것이 효율적 → **코드의 재사용** 측면에서 매우 유용
- 코드의 중복을 억제 재사용성을 높이는 함수는 유지보수의 편의성 ↑ 
실수를 줄여 **코드의 신뢰성** ↑
- 함수는 객체타입의 값 - 이름(식별자)을 붙일 수 있다.
적절한 함수이름은 함수의 내부코드를 이해하지 않고 함수의 역할을 파악할수 있게 돕는다. - **코드의 가독성**을 향상
- 코드는 동작하는것만이 목적은 X - 코드는 개발자를 위한 문서 - **가독성**이 좋은 코드가 좋은코드이다

---

## 12.3 함수 리터럴

- 자바스크립트의 함수는 객체타입의 값 - 합수도 리터럴로 생성 할 수 있다
- 리터럴의 구성요소
    
    
    | 구성요소 | 설명 |
    | --- | --- |
    | 함수이름 | - 함수 이름은 식별자 - 식별자 네이밍 규칙을 준수해야 한다.
    - 함수 이름은 함수몸체 내에서만 참조할수있는 식별자 
    - 함수 이름은 생략할 수 있다. - 이름 O : 기명함수, 이름 X : 무명함수 |
    | 매개변수 목록 | - 0개이상의 매개변수를 소괄호로 감싸고 쉼표로 구분
    - 각 매개변수에는 함수를 호출할때 지정한 인수가 순서대로 할당
    - 매개변수는 함수 몸체내에서 변수와 동일하게 취급 - 식별자 네이밍 규칙을 준수해야한다. |
    | 함수 몸체 | - 함수가 호출되었을때 실행될 문들을 하나의 실행단위로 정의한 코드불록
    - 함수 몸체는 함수 호출에 의해 실행 |
- 함수 리터럴도 평가되어 값을 생성 - 이 값은 객체 → 함수는 객체
- 함수는 객체지만 일반객체와는 다르다 - 일반객체는 호출할 수 있지만 함수는 호출할 수 있다 & 일반객체에는 없는 함수 객체만의 고유한 프로퍼티를 같는다

---

## 12.4 함수 정의

- 함수를 호출하기 이전에 인수를 전달받을 매개변수와 실행할 문들, 그리고 반환할 값을 지정하는것
- 자바스크립트 엔진에 의해 평가되어 함수 객체가 됨
- 함수를 정의하는 방법
    
    
    | 함수 정의 방식 | 예시 |
    | --- | --- |
    | 함수 선언문 | function add(x, y){
       return x + y;
    } |
    | 함수표현식 | var add = function (x, y){
       return x + y;
    } |
    | function 생성자 함수 | var add = new Funcion(’x’, ’y’, ‘return x + y’) |
    | 화살표함수 | var add = (x, y) ⇒ x+y |

---

- 모든 함수 정의 방식은 함수를 정의한다는 면에서 동일. 단, 미묘하지만 중요한차이가 있다.
    - 변수선언과 함수 정의
        
        변수는 선언(declaration)한다고 했지만 함수는 정의(definition)
        함수선언문이 평가되면 식별자가 암묵적으로 생성되고 객체가 할당된다.
        

### 12.4.1 **함수선언문**

- 함수 선언문은 함수 리터럴과 형태가 동일. 그러나 함수 선언문은 함수이름을 생략할수 없다.
- 함수 선언문은 표현식이 아닌 문이다 - 콘솔에서 함수 선언문을 실행하면 undefined가 출력
표현식인 문이라면 undefined 표현식이 평가되어 생성된 함수가 출력되어야한다.

```jsx
function add(x, y){
  return x + y;
}

console.dir(add); //ƒ add(x, y)
console.log(add(2,5)); // 7

function (x, y){
  return x + y;
}
//Uncaught SyntaxError: Function statements require a function name

```

- 문은 변수에 할당할수 없다 - 함수 선언문도 표현식이 아닌 문이므로 변수에 할당 할 수 없다.
- 그러나 함수 선언문이 변수에 할당되는것 처럼 보일수있다.
→ 함수선언문으로 해석하는 경우와 표현식인 문인 함수 리터럴 표현식으로 해석하는경우가 있기때문이다.
- {}은 중의적 표현이다 - 코드문맥에 따라 해석이 달라진다.
{}이 단독으로 존재 - 블록문, 값으로평가되어야할 문맥에서 피연산자로 사용 - 객체리터럴
- 기명함수 리터럴도 중의적인 코드이다 - 코드의 문맥에 따라 달라진다
함수 이름이 있는 함수 리터럴을 단독으로 사용(값으로 평가되어야하는 문맥에서 함수리터럴을 사용하지 않는경우) - 함수선언문
함수리터럴이 값으로 평가되어야하는 문맥 - 함수 리터럴 표현식
    
    ```jsx
    //기명함수 리터럴을 단독으로 사용하면 함수 선언문으로 해석된다.
    //함수선언문에서는 함수이름을 생략할수없다.
    function foo(x, y){console.log('foo');}
    foo();//foo
    
    //함수 리터럴을 피연산자로 사용하면 함수 선언문이아니라 함수 표현식으로 해석
    //()그룹연산자 내에 있는 함수
    (function bar(){console.log('bar');});
    bar();//Uncaught ReferenceError: bar is not defined
    ```
    

![bar는 함수 몸체내에서만 참조할수 있는 식별자이므로 함수를 호출 할 수 없다.](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/67d285bc-c17f-43f8-aefc-b54f05ff6e56/Untitled.png)

bar는 함수 몸체내에서만 참조할수 있는 식별자이므로 함수를 호출 할 수 없다.

![foo는 자바스크립트 엔진이 암묵적으로 생성한 식별자이다.](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ef027fb8-a817-4744-a1a4-a1a608462559/Untitled.png)

foo는 자바스크립트 엔진이 암묵적으로 생성한 식별자이다.

- 따라서 자바스크립트 엔진은 생성된 함수를 호출하기 위해 이름과 동일한 이름의 식별자를 암묵적으로 생성하고 거기에 함수 객체를 할당한다.
- 함수는 함수의 이름으로 호출하는 것이 아니라 함수 객체를 가리키는 식별자로 호출한다.
    
    ```jsx
    // 식별자          함수이름
    var add = function add(x, y){
      return x + y;
    }
    
    //         식별자
    console.log(add(2,5)); // 7
    ```
    

### 12.4.2 함수 표현식

- 자바스크립트 함수는 객체타입의 값 → 값처럼 변수에 할당할수 있고 프로퍼티 값이 될수 있으며, 배열의 요소가 될수있다 - 이처럼 값의 성질을 갖는 객체를 **일급객체**라고한다
- 함수는 일급객체 이르모 함수 리터럴 로 생성한 함수 객체를 변수에 할당할수있다.
→ 이러한 함수 정의 방식을 함수 표현식이라한다.
- 함수 리터럴의 함수이름은 생략할수있다 → 익명함수라한다. 함수 표현식의 함수리터럴은 함수리름을 생략하는것이 일반적이다

```jsx
//함수표현식
var add = function (x, y){
  return x + y;
}
console.log(add(2,5)); // 7

//기명 함수표현식
var add2 = function foo(x, y){
  return x + y;
}
//함수 객체를 가리키는 식별자로 호출
console.log(add2(2,5)); // 7
//함수이름으로 호출하면 ReferenceError 발생
//함수이름은 함수 몸체내부에서만 유효한 식별자
console.log(foo(2,5)); // Uncaught ReferenceError: foo is not defined
```

### 12.4.3 함수 생성 시점과 호이스팅

```jsx
//함수 참조
console.dir(add); //ƒ add(x, y)
console.dir(sub); //undefined

//함수 호출
console.log(add(2,5)); //7
console.log(sub(2,5)); //Uncaught TypeError: sub is not a function

//함수 선언문
function add(x, y){
  return x + y;
}

//함수 표현식
var sub = function (x, y){
  return x - y;
}
```

- 함수선언문으로 정의한 함수는 함수선언문 이전 호출 가능
함수표현식으로 정의한 함수는 함수표현식 이전에 호출 불가능
→ **함수 선언문 정의 함수와 함수 표현식 정의한 함수의 생성 시점이 다르기 때문**
- 함수 선언문도 코드가 한줄씩 순차적으로 실행되는 시점인 런타임 이전에 자바스크립트 엔진에 의해 먼저 실행
→ 함수 선언문으로 함수를 정의하면 런타임 이전에 함수 객체 생성
    → 함수 이름과 동일한 이름의 식별자를 암묵적으로 생성 → 생성된 함수 객체 할당
- **함수 선언문이 코드의 선두로 끌어올려진것처럼 동작하는 자바스크립트의 고유의 특징을 함수 호이스팅이라한다**
- 함수 호이스팅과 변수 호이스팅은 미묘한 차이가 있으므로 주의
- var키워드를 사용한 변수는 undefined로 초기화
함수선언문을 통해 암묵적으로 생성된 식별자는 함수 객체로 초기화
→ var 키워드를 사용한 변수 선언문 이전에 변수 참조시 호이스팅으로 인해 undefined로 평가
    함수 선언문으로 정의한 함수를 함수 선언문 이전에 호출 시 호이스팅에 의해 호출 가능
- 함수 표현식은 변수에 할당되는 값이 함수 리터럴인 문
→ 함수표현식은 변수 선언문과 변수 할당문을 한번에 기술한 축약 표현과 동일하게 작동
- 변수선언은 런타임이전에 undefined로 초기화 되지만..
**변수 할당문의 값은 할당문이 실행되는 시점, 즉 런타임에 평가 되므로 함수 표현식의 함수리터럴도 할당문이 싱행되는 시점에 평가되어 함수 객체가 된다.**
- **함수 표현식으로 함수를 정의하면 변수 호이스팅이 발생
함수선언문으로 함수를 정의하면 함수 호이스팅이 발생**
- 함수 호이스팅은 함수를 호출하기 전에 반드시 함수를 선언해야 한다는 당연한 규칙을 무시한다.
히같은 문제때문에 함수선언문 대신 함수 표현식을 사용할것을 권장한다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3cf5f168-d42f-429e-b66d-8c240c8adcac/Untitled.png)

### 12.4.4 Function 생성자 함수

- 자바스크립트가 기본 제공하는 빌트인 함수인 Function 생성자 함수에 배개변수 목록과 함수 몸체를 문자열로 전달하면서 new 연산자와 함께 호출하면 함수 객체를 생성해서 반환한다.
사실 new 연산자 없이 호출해도 결과는동일..
- Function 생성자 함수로 함수를 생성하는 방식은 일반적이지 않으며 바람직하지도 않다.
- Function 생성자 함수로 생성한 함수는 클로저를 생성하지 않는 등 함수 선언문이나 함수 표현식으로 생성한 함수와 다르게 동작

```jsx
var add = new Function('x','y','return x+y');
console.log(add(2,5));//7

var add1 = (function(){
  var a = 10;
  return function(x, y){
     return x+y+a;
  }
}());
console.log(add1(1,2));//13

var add1 = (function(){
  var a = 10;
  return new Function('x','y','return x+y+a');
}());
console.log(add2(1,2));//Uncaught ReferenceError: add2 is not defined
```

### 12.4.5 화살표 함수

- function 키워드 대신 화살표 >를 사용해 더 간략한 방법으로 함수를 선언할수 있다.
- 항상 익명함수로 정의한다
- 기존의 함수 선언문 또는 함수 표현식을 완전히 대체하기 위해 디자인 된것은 아니다.
- 기존의 함수보다 표현만 간략한 것이 아니라 내부 동작 또한 간략화 되어있다.
- 화살표 함수는 생성자함수로 사용 X, 기존 함수와 this 바인딩 방식이 다르고, prototype 프로퍼티가 없으면 argument 객체를 생성 X

```jsx
const add = (x,y) => x + y;
console.log(add(2,5)); //7
```

---

## 12.5 함수 호출

- 함수는 함수를 가리키는 식별자와 함쌍의 소과로인 함수 호출연산자로 호출한다.
- 함수호출연산자 내에는 0개이상의 인수를 쉼표로 구분해서 나열
- 함수를 호출하면 현재의 실행으름을 중단하고 호출된함수로 실행흐름을 옮긴다.
- 매개변수에 인수가 순서대로 할당, 함수 몸체의 문들이 싱행되기 시작한다.

### 12.5.1 매개변수와 인수

- 함수를 실행하기 위해 필요한 값을 함수 외부에서 함수 내부로 전달할 필요가 있는 경우, 매개변수(parameter)를 통해 인수(argument)를 전달한다.
- 인수는 값으로 평가될 수 있는 표현식이여야한다.
- 인수는 함수를 호출할때 지정, 개수와 타입에 제한이 없다.
- 매개변수는 함수를 정의할때 선언, 함수 몸체 내부에서 변수와 동일하게 취급
- 함수가 호출되면 함수 몸체내에서 암묵적으로 매개변수가 생성되고 일반 변수와 마찬가지로 undefined로 초기화된 이후 인수가 순서대로 할당
- 매개변수는 함수 몸체 내부에서만 참조할 수 있다.
→ 매개변수의 스코프는 함수 내부이다.
- 함수는 매개변수의 개수와 인수의 개수가 일치하는지 체크X
→ 매개변수만큼인수를 전달하는것이 일반적이지만 그렇지 않은경우에도 에러가 발생하지 않는다.
- 인수가 부족할 경우 항당되지 않은 매개변수 값은 undefined다.
인수가 더 많은 경우 초과된 인수는 무시된다. - 초과된 인수가 버려지는건아니다 arguments객체 프로퍼티러 보관된다.

```jsx
//함수선언문
function add(x, y){
   console.log(x, y);//2 5
   return x + y;
}

//함수 호출
//인수 1,2가 매개변수 x,y에 순서대로 할당 함수 몸체의 문들이 실행된다.
var result = add(1,2);
console.log(x, y);//Uncaught ReferenceError: x is not defined

add(1); //NaN
add(1,2,3); //3
```

### 12.5.2 인수 확인

- 자바스크립트 함수는 매개변수와  인수의 개수가 일치하는지 확인하지 않는다
- 자바스크립트는 동적 타입 언어다. 따라서 자바스크립트 함수는 매개변수 타입을 사전에 지정 할 수 없다.
- 함수를 정의할 때 적절한 인수가 전달되었는지 확인할 필요가있다.
- 부적절한 호출을 사전에 방지할수 없고 에러는 런타임에 발생하게 된다.
→ 타입스크립트와 같은 정적 타입을 선언할 수 있는 자바스크립트의 상위 확장을 도입해서 컴파일 시점에 부적절한 호출을 방지할수 있게 하는 것도 하나의 방법이다.

```jsx
function add(x, y){
   if(typeof x !== 'number' || typeof y !== 'number'){
     throw new TypeError('인수는 숫자여야합니다.');
   }
   return x + y;
}
console.log(add(2,3));//5
console.log(add(2));//Uncaught TypeError: 인수는 숫자여야합니다.
console.log(add('a','b'));//Uncaught TypeError: 인수는 숫자여야합니다.
```

- 인수의 개수는 확인하고 있지않지만 arguments객체를 통해 인수 개수를 확인할수도 있다.
또는 인수가 전달되지 않은경우 단축평가를 사용해 매개변수에 기본값을 할당하는 방법도 있다.

```jsx
function argumentChk(){
   console.log(arguments);
}
console.log(argumentChk(1,2,3));//Arguments(3) [1, 2, 3, callee:...

function add(a,b,c){
   a =  a || 0;
   b =  b || 0;
   c =  c || 0;
   return a + b + c;
}
console.log(add(1,2,3));//6
console.log(add(1,2));//3
console.log(add(1));//1
console.log(add(1));//0

function add2(a=0,b=0,c=0){
   return a + b + c;
}
console.log(add2(1,2,3));//6
console.log(add2(1,2));//3
console.log(add2(1));//1
console.log(add2(1));//0
```

### 12.5.3 매개변수의 최대개수

- ECMAScript 사양에서의 매개변수의 최대개수에 대해 명시적으로 제한하고 있지 않다.
- 물리적인 한계는 있으므로 엔진마다 매개변수의 최대개수에 대한 제한이 있겠지만 충분히 많은 매개변수를 지정할수있다.
- 매개변수는 순서에 의미가 있다. - 함수를 호출할때 전달해야 할 인수의 순서를 고려해야한다.
→ 유지보수성이 나빠진다.
- 함수의 매개변수는 코드를 이해하는데 방해되는 요소이므로 이상적인 매개변수 개수는 0개, 적을수록 좋다.
- **이상적인 함수는 한가지 일만 해야 하며 가급적 작게 만들어야한다.
→** 매개변수는 최대 3개 이상을 넘지 않는 것을 권장
- 그 이상의 매개변수가 필요하다면 매개변수를 선언하고 객체를 인수로 전달하는것이 유리
    - 프로퍼티 키만 정확히 지정하면서 매개변수의 순서를 신경쓰지 않아도 된다.
    - 명시적으로 인수의 의미를 설명하는 프로퍼티 키를 사용하게 되므로 코드의 가독성도 좋아진다
    - 주의할 것으로 함수 외부에서 함수 내부로 전달한 객체를 함수내부에서 변경하면 함수 외부객체가 변경되는 부수효과가 발생

### 12.5.4 반환문

- 함수는 return키워드와 표현식으로 이뤄진 반환문을 사용해 실행결과를 함수 외부로 반환 할수있다.
- **함수 호출은 표현식이다**
- 반환문은 함수의 실행을 중단하고 함수 몸체를 빠져나간다. - 반환문 이후에 다른 문이 존재하면 그 문은 무시된다.
- 반환문은 생략할수 있다. - 마지막 문까지 실행 후 undefined를 반환한다
- return 키워드와 반환값으로 사용할 표현식사이 줄바꿈이 있으면 세미콜론 자동삽입 기능에 의해 세미콜론이 추가되어 의도치 않은결과가 발생할수있다.
- 반환문은 함수 몸체 내부에서만 사용할수있다. 전역에서 사용하면 문법에러가 발생한다.
Node.js에서는 파일별로 독립적인 파일 스코프를 갖기 때문에 바깥영역에 사용해도 에러X

```jsx
function returnAfterLog(x,y){
		return x+y;
}
console.log(returnAfterLog(1,2));//3

function foo(){
		return;
}
console.log(foo());//undefined

function multiply(){
		return
		x*y;
}
console.log(multiply(1,2));//undefined

return;//Uncaught SyntaxError: Illegal return statement
```

---

## 12.6 참조에 의한 전달과 외부 상태의 변경

- 원시값은 갑셍의한 전달 객체는 참조에 의한 전달 방식으로 동작
- 매개변수도 함수몸체 내부에서 변수와 동일하게 취급되므로 매개변수 또한 타입에 따라 값에 의한 전달, 참조에의한 전달 방식을 그대로 따른다.

```jsx
function changeVal(primitive, obj){
		primitive += 100;
		obj.name = 'PARK';
}

//외부 상태
var num = 100;
var person = {name : 'Lee'};

changeVal(num, person);

//원시 값은 값이 훼손X 참조값은 값이 훼손된다.
console.log(num);//100
console.log(person);//{name: 'PARK'}
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4370910b-1533-4f2d-808f-86baad0a9213/Untitled.png)

- 원시 타입 인수는 값 자체가 복사되어 매개변수에 전달되기 때문에 함수 몸체에서 그 값을 변경해도 원본은 훼손되지 않는다. → 함수 외부에서 함수 몸체 내부로 전달한 원시 값의 원본을 변경하는 부수효과도 발생하지 않는다
- 객체타입은 참조값이 복사되어 매개변수에 전달 → 함수 몸체에서 참조값을 통해 객체를 변경할 경우 원본이 웨손된다. → 함수외부에서 함수 몸체 내부로 전달한 참조값에 의해 원본 객체가 변경되는 부수효과가 발생
- 함수가 외부상태를 변경하면 상태변화를 추적하기 어려워진다 → 코드의 복잡성을 증가, 가독성을 해치는 원인
- 객체의 변경을 추적하려면 옵저버 패턴 등을 통해 객체를 참조를 공유하는 모든이들에게 변경 사실을 통지하고 대처하는 추가 대응이 필요
- 문제 해결 방법중 하나는 객체를 불변객체로 만들어 사용하는것
- 객체의 상태변경을 원천봉쇄하고 객체의 상태변경이 필요한 경우 깊은 복사를 통해 원본객체를 완전히 복제를 통해 새로운 객체를 생성하고 재할당을 통해 교체 → 부수효과를 없앨수있다.
- 외부 상태를 변경하지 않고 외부상태에 의존하지도 않는 함수를 순수 함수라 한다.

---

## 12.6 다양한 함수의 형태

---

???

스코프

실행컨텍스트

클로생성자에의한 객체셍성

메서드

this

프로토타입

모듈화

statement

성명, 진술, 서술

클린코드

리팩토링

리터럴

리터럴은 사람이 이해할수있는 문자 또는 약속된 기호를 사용해 생성하는 표기방식

프로퍼티

선언과 정의

console.dir과 console.log

표현식

표현식이란 어떤 값으로 이행하는 임의의 유효한 코드 단위

this, new, super, 그룹연산자…

[https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Expressions_and_Operators#표현식](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Expressions_and_Operators#%ED%91%9C%ED%98%84%EC%8B%9D)

의사코드

생성자 함수

생성자함수는 객체를 생성하는 함수, 객체를 생성하는 방식은 객체리터럴 이외에 다양한방법이있따. 17장에 있다.

클로저

화살표 함수의 this 바인딩

타입스크립트

단축평가

부수효과

불변객체

16.5.4에서 배운다.

옵저버패턴