# 06장 데이터 타입

데이터 타입 : 값의 종류

| 구분 | 데이터타입 | 설명 |
| --- | --- | --- |
| 원시타입 | 숫자 타입 | 숫자, 정수와 실수 구분 없이 하나의 숫자타입만 존재 |
|  | 문자열 타입 | 문자열 |
|  | 불리언타입 | 논리적 참과 거짓 |
|  | undefined 타입 | var 키워드로 선언된 변수에 암묵적으로 할당되는 값 |
|  | null타입 | 값이 없다는 것을 의도적으로 명시할 때 사용하는 값 |
|  | 심벌타입 | ES6에서 추가된 7번째 타임 |
| 객체타입 |  | 객체 함수 배열 등 |

## 6.1 숫자 타입

- C나 자바는 정수, 실수를 구분해서 int, long, float, double등 다양한 숫자 타입을 제공
그러나 자바스크립트는 하나의 숫자타입만 존재
- ECMAScript 사양에 따르면 숫자 타입의 값은 배정밀도 64비트 부동소수점 형식을 따른다.
→ 모든 수를 실수로 처리한다, 정수만 표현하기 위한 데이터 타입이 별도로 존재하지 않는다.
- 자바스크립트는 2진수, 8진수, 16진수를 표현하기 위한 데이터 타입을 제공하지 않기 때문에 이들 값을 참조하면 모두 10진수로 해석된다.
- 숫자 타입은 추가적으로 세가지 특별한 값도 표현할 수 있다.
Infinity, -Infinity, NaN

```jsx
var integer = 10;//정수
var double = 10.12;//실수
var negative = -20//음의 정수

var binary = 0b01000001;//2진수
console.log(binary);//65
var octal = 0o101;//8진수
console.log(octal);//65
var hex = 0x41;//16진수
console.log(hex);//65

console.log(binary === octal);//true
console.log(octal=== hex);//true

//숫자타입은 모두 실수로 처리된다.
console.log(1.0 === 1);//true
console.log(4/2);//2
console.log(3/2);//1.5

//세가지 특별한 값
console.log(10/0);//Infinity
console.log(-10/0);//-Infinity
console.log(1*'String');//NaN
```

배정밀도 64비트 부동 소수점 형식이 뭔데??
부동소수점 : 컴퓨터가 실수를 표현하는 방법
단정도 부동소수점 : 32bit, 부호 1bit 지수부 8bit 가수부 23bit
배정도 부동소수점 : 64bit, 부호 1bit 지수부 11bit 가수부 52bit 

```jsx
console.log(0.1 + 0.2 === 0.3); // false
console.log(0.1+0.2); //0.30000000000000004
//0.1을 2진법으로 바꾸면
//0.00011001100...로 순환 소수가 나온다.
0.1.toString(2)//0.0001100110011001100110011001100110011001100110011001101
//이진수로 변환하고 소수점을 1이 나올 때까지 오른쪽 또는 왼쪽으로 옮긴 다음,
//소수점 오른쪽에 해당하는 수를 가수부분에 넣게 되는데
//이 수가 표현 자리수보다 넘어서게 되면 나머지 부분에서 반올림 처리를 하므로
//근사값이 저장되면서 부정확하게 된다
```

## 6.2 문자열 타입

- 문자열은 텍스트 데이터를 나타내는데 사용한다.
문자열은 0개 이상의 16비트 유니코드 문자의 집합으로 전세계 대부분의 문자를 표현할 수 있다.
- 문자열은 작은따옴표(’’), 큰따옴표(””), 백틱(``)으로 텍스트를 감싼다
일반적인 표기법은 작은따옴표이다.
- 다른 타입 값과 같이 문자열을 따옴표로 감싸는 이유는 키워드나 식별자 같은 토큰과 구분하기 위해서 → 만약 문자열을 따옴표로 감싸지 않으면 자바스크립트 엔지는 키워드나 식별자 같은 토큰으로 인식한다.
- C : 문자열 타입X 문자의 배열로 문자열을 표현, 자바: 문자열을 객체로 표현
자바스크립트의 문자열은 원시 타입, 변경 불가능 값이다. → 문자열 생성시 문자열을 변경할수 없다는 것을 의미한다.

```jsx
var string;
string = '문자열';
string = "문자열";
string = `문자열`;

string = ' 작은 따옴표로 감싼 문자열 내의 "큰따옴표"는 문자열로 인식';
string = ' 큰 따옴표로 감싼 문자열 내의 "작은따옴표"는 문자열로 인식';

console.log(string);//큰 따옴표로 감싼 문자열 내의 "작은따옴표"는 문자열로 인식

//따옴표로 감싸지 않으면 hello를 식별자로 인식
string = hello;//Uncaught ReferenceError: hello is not defined
```

## 6.3 템플릿 리터럴

- ES6부터 템플릿 리터럴이라고 하는 새로운 문자열 표기법이 도입되었다.
- 멀티라인 문자열, 표현식 삽입, 태그드 템플릿 등 편리한문자열 처리 기능을 제공
- 런타임에 일반 문자열로 변환되어 처리된다.
- 템플릿 리터럴은 백틱을 사용해 표현한다.

```jsx
var template = `Template literal`;
console.log(template );//Template litera
```

### 6.3.1 멀티라인 문자열

```jsx
var template = `<ul>\n\t<li><a href="#">HOME</a><li>\n</ul>`;
//<ul>
//	<li><a href="#">HOME</a><li>
//</ul>
console.log(template);

var template2 = `<ul>
	<li><a href="#">HOME</a><li>
</ul>`;
//<ul>
//	<li><a href="#">HOME</a><li>
//</ul>
console.log(template2);
```

### 6.3.2 표현식 삽입

표현식 삽입을 통해 간단히 문자열을 삽입할수 있다. → 문자열 연산자보다 가독성 좋고 간편히 문자열을 조합할 수 있다.

```jsx
var first = 'Suin';
var last = 'Lee';
console.log(`I'm ${first} ${last}`);//I'm Suin Lee
console.log(`1 + 2 = ${1 + 2}`);//1 + 2 = 3
```

## 6.4 불리언 타입

- 논리적 참, 거짓을 나타내는 true와 false뿐이다
- 조건문에서 자주사용한다.

```jsx
var foo = true;
console.log(foo);//true
foo = false;
console.log(foo);//false
```

## 6.5 undefined 타입

- 값은 undefined가 유일하다
- var 키워드로 선언한 변수는 암묵적으로 undefined로 초기화 된다.
변수 선언에 확보된 메모리 공간에 처음 할당이 이뤄 질 때 내버려 두지 않고 자바스크립트 엔진이 undefined로 초기화한다.
- 자바스크립트엔진이 변수를 초기화하는데 사용하는 undefined를 개발자가 의도적으로 변수에 할당한다면 undefined의 본래 취지와 어긋날뿐더러 혼란을 줄 수 있으므로 권장하지 않는다.
값이 없다는 걸 명시하고 싶다면 null을 할당한다.

## 6.6 null 타입

- 값은 null이 유일하다.
- 자바스트립트는 대소문자를 구별하므로 null은 Null, NULL 등과 다르다
- null 타입은 값이 없다는 것을 의도적으로 명시할 때 사용한다.
- 변수에 null을 할당하는 것은 변수가 이전에 참조하던 값을 더이상 참조하지 않겠다는 의미이다.
→ 이전에 할당되어 있던 값에 대한 참조를 명시적으로 제거하는것을 의미
- 함수가 유효한 값을 반환할 수 없는 경우 null을 반환하기도 한다.

## 6.7 심벌 타입

- ES6에서 추가된 7번째 타입, 원시타입이다.
- 다른값과 중복되지 않는 유일무이한 값이다.
→ 주로 이름이 충돌할 위험이 없는 객체의 유일한 프로퍼티 키를 만들기 위해 사용한다.
- 심벌 이외의 원시 값은 리터럴을 통해 생성, 심벌은 Symbol함수를 통해 호출해 생성한다.
이때 생성된 심벌 값은 외부에 노출되지 않으면 다른값과 절대 중복되지 않는 유일무이한 값이다.

```jsx
//심벌 값 생성
var key = Symbol('key');
console.log(typeof key);

//객체생성
var obj = {};

//이름이 출돌할 위험이 없는 유일무이한 값인 심벌을 프로퍼티 키로 사용한다.
obj[key] = 'value';
console.log(obj[key]);//value
```

## 6.8 객체타입

- 원시타입과 객체타입은 근본적으로 다르다.
- 자바스크립트를 이루고 있는 거의 모든것이 객체이다.

## 6.9 데이터타입의 필요성

### 6.9.1 데이터타입에 의한 메모리 공간의 확보와 참조

- 값은 메모리에 저장하고 참조할 수 있어야한다.
메모리 값을 저장하려면 먼저 확보해야 할 메모리 공간의 크기를 결정해야한다.
→ 몇바이트의 메모리 공간을 사용해야 낭비와 손실 없이 값을 저장할수 있는지 알아야한다.
- 자바스크립트 엔진은 데이터타입에 따라 정해진 크기의 메모리 공간을 확보한다.
→ 변수에 할당되는 값의 데이터 타입에 따라 확보해야할 메모리 공간의 크기가 결정된다.
- 값을 참조 하는 경우에도 한번에 읽어 들여야할 메모리 공간의 크기(메모리 셀의 개수)를 알아야한다.

### 6.9.2 데이터 타입에 의한 값의 해석

- 메모리에 저장된 값은 데이터 타입에 따라 다르게 해석될 수 있다.
ex) 숫자 65, 문자 ‘A’

데이터 타입이 필요한 이유

- 값을 저장할때 확보해야하는 메모리 공간의 크기를 결정하기 위해
- 값을 참조할때 한번에 읽어 들여야 할 메모리 공간의 크기를 결정하기 위해
- 메모리에서 읽어 들인 2진수를 어떻게 해석할지 결정하기 위해

## 6.10 동적타이핑

### 6.10.1 동적 타입 언어와 정적 타입 언어

- C나 자바 같은 정적 타입 언어는 변수를 선언할때 변수에 할당할수있는 데이터 타입을 사전에 선언 → 명시적 타입선언 이라한다.
정적 타입 언어는 타입 변경 할 수 X, 컴파일 시점에 타입체크를 수행
타입의 일관성을 강제함으로써 안정적인 코드의 구현을 통해 런타임에 발생하는 에러를 줄인다.
    
    ```c
    char c;
    int num;
    ```
    
- 자바스크립트의 변수는 선언이 아닌 할당에 의해 타입이 결정된다.
또한, 재할당에 의해 변수 타입은 언제든지 동적으로 변할수 있다. _> 동적 타이핑이라고 한다.
- 자바스크립트는 동적타입 언어다.
- 변수는 타입을 가지지 않는다.
그러나 값은 데이터 값을 갖는다.

### 6.10.2 동적 타입 언어와 변수

- 동적 타입언어는 변수에 어떤 데이터 타입의 값이라도 자유롭게 할당할수 있다.
데이터타입에 대해 무감각해질 정도로 편리 → 편리함의 이면에는 위험도 도사리고 있다.
- 변수 값은 언제든지 변경될 수 있기 때문에 복잡한 프로그램에서는 변화하는 변수 값을 추적하기 어려울 수 있다.
동적 타입 언어의 변수는 값의 변경에 의해 타입도 언제든지 변경될 수 있다.
- 개발자의 의도와는 상관없이 암묵적으로 타입이 자동으로 변환되기도 한다.
잘못된 예측에 의해 작성된 프로그램은 당연히 오류를 뿜어낼 것이다.
→ 동적 타입 언어는 유연성은 높지만 신뢰성은 떨어진다.
- 코드는 오해하지 않도록 작성해야한다. 사람이 이해할 수 있는 코드 가독성이 좋은코드가 좋은코드다.