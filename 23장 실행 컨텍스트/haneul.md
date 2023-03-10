# 23. 실행 컨텍스트

## 소스코드의 타입

 ECMAScript 사양은 소스코드를 4가지 타입으로 구분한다. 가지 타입의 소스코드는 실행 컨텍스트를 생성한다.

### 전역 코드 (Global code)

 전역에 존재하는 소스코드를 말한다. (전역에 정의된 함수, 클래스 등의 내부 코드는 포함하지 않는다!)

- 전역 변수를 관리하기 위해 최상위 스코프인 전역 스코프를 생성해야 한다.
- 선언된 변수와 정의된 함수를 전역 객체의 프로퍼티와 메서드로 바인딩하고 참조하기 위해 전역 객체와 연결되어야 한다.
- 이를 위해 전역 코드가 평가되면 전역 실행 컨텍스트가 생성된다.

### 함수 코드 (Function code)

 함수 내부에 존재하는 소스코드를 말한다. 함수 내부에 중첩된 함수, 클래스 등의 내부 코드는 포함되지 않는다.

- 지역 스코프를 생성하고 지역 변수, 매개변수, arguments 객체를 관리해야 한다.
- 생성한 지역 스코프를 전역 스코프에서 시작하는 스코프 체인의 일원으로 연결해야 한다.
- 이를 위해 함수 코드가 평가되면 함수 실행 컨텍스트가 생성된다.

### eval 코드

 빌트인 전역 함수인 eval 함수에 인수로 전달되어 실행되는 소스코드를 말한다.

- eval 코드는 자신만의 독자적인 스코프를 생성한다.
- 이를 위해 eval 코드가 평가되면 eval 실행 컨텍스트가 생성된다.

### 모듈 코드 (Module code)

 모듈 내부에 존재하는 소스코드를 말한다. 모듈 내부의 함수, 클래스 등의 내부 코드는 포함되지 않는다.

- 모듈 코드는 모듈별로 독자적인 모듈 스코프를 생성한다.
- 이를 위해 모듈 코드가 평가되면 모듈 실행 컨텍스트가 생성된다.

## 소스코드의 평가와 실행

 모든 소스코드는 실행에 앞서 평가 과정을 거친다. (코드를 실행하기 위한 준비)

- 자바스크립트는 소스코드를 평가 과정과 실행 과정으로 나누어 처리한다.
- 이때 평가 과정에서는 실행 컨텍스트를 생성하고 변수, 함수 등의 선언문만 먼저 실행하여 생성된 변수나 함수 식별자를 키로 실행 컨텍스트가 관리하는 스코프에 등록한다.
- 소스코드 실행 과정에서는 선언문을 제외한 소스코드가 순차적으로 실행된다.
    - 이때 실행에 필요한 정보(변수나 함수의 참조)를 실행 컨텍스트가 관리하는 스코프에서 검색해서 취득한다.
    - 소스코드의 실행 결과는 다시 실행 컨텍스트가 관리하는 스코프에 등록된다.

🔍 소스코드 평가 → 실행 컨텍스트 -(실행에 필요한 정보)→ 소스코드 실행 -(실행 결과)→ 실행 커텍스트

```jsx
var x; // 평가 과정에서 실행됨
x = 1; // 실행 과정에서 실행됨

// 자바스크립트 엔진은 해당 코드를 2개의 과정으로 나누어 처리한다.
// 소스코드 평가 과정에서 변수 선언문이 실행되며 생성된 식별자 x가 실행 컨텍스트의 스코프에 등록된다. (값은 undefined)
// 이후 소스코드 실행 과정에서 변수 할당문이 실행되며 이때 실행 컨텍스트가 관리하는 스코프에서 식별자 x가 있는지 검색한다.
// 검색 성공 시 x에 값을 할당하고 결과를 실행 컨텍스트의 스코프에 등록한다.
```

## 실행 컨텍스트의 역할

```jsx
// 전역 변수
const x = 1;
const y = 2;

// 함수 정의
function foo(a) {
	// 지역 변수
	const x = 10;
	const y = 20;

	console.log(a + x + y);
}

// 함수 호출
foo(100);

console.log(x + y); // 3
```

1. 전역 코드 평가
    - 전역 코드를 실행 하기 전 전역 코드 평가 과정을 거친다.
    - 선언문이 먼저 실행되며 그 결과 생성된 전역 변수와 전역 함수가 실행 컨텍스트가 관리하는 전역 스코프에 등록된다
        - 위 코드에서는 x, y, foo 식별자가 전역 스코프에 등록된다.
2. 전역 코드 실행
    - 전역 코드 평가 과정 이후 런타임이 시작되어 전역 코드가 순차적으로 실행된다.
    - 이때 전역 변수에 값이 할당되고 함수가 호출된다.
        - 함수 호출 시 순차적으로 실행되던 전역 코드의 실행을 일시 중단하고 코드 실행 순서를 변경하여 함수 내부로 진입한다.
3. 함수 코드 평가
    - 함수 호출에 의해 코드 실행 순서가 변경되어 함수 내부로 진입하면 함수 내부의 코드를 실행하기에 앞서 함수 코드 평가 과정을 거친다.
    - 이때 매개변수와 지역 변수가 실행 컨텍스트가 관리하는 지역 스코프에 등록된다.
        - 위 코드의 foo 함수 내부에서는 매개변수 a와 x, y 변수가 스코프에 등록된다.
    - 또한 함수 내부에서 지역 변수처럼 사용할 수 있는 arguments 객체가 생성되어 지역 스코프에 등록되고 this 바인딩도 결정된다.
4. 함수 코드 실행
    - 함수 코드 평가 과정 이후 런타임이 시작되어 함수 코드가 순차적으로 실행된다.
    - 이때 짜여진 코드에 맞게 x와 y 변수에 값이 할당되고 console.log 메서드가 호출된다.
        - 이때 console.log 메서드를 호출하기 위해 식별자인 console을 스코프 체인을 통해 검색한다.
            - console 식별자는 전역 객체에 프로퍼티로 존재한다 → 전역 객체의 프로퍼티가 마치 전역 변수처럼 전역 스코프를 통해 검색 가능하다.

🔍 코드가 실행되려면 스코프, 식별자, 코드 실행 순서 등의 관리가 필요하다!

1. 선언에 의해 생성된 모든 식별자를 스코프를 구분하여 등록하고 상태 변화를 지속적으로 관리할 수 있어야 한다.
2. 스포크는 중첩 관계에 의해 스코프 체인을 형성해야 한다.
    1. 스코프 체인을 통해 상위 스코프로 이동하여 식별자를 검색할 수 있어야 한다.
3. 현재 식행 중이 코드의 실행 순서를 변경할 수 있어야 하며 다시 되돌아갈 수도 있어야 한다. (함수 호출에 의한 실행 순서 변경)

> 이 모든 것을 관리하는 것이 실행 컨텍스트이다.
실행 컨텍스트는 소스코드를 실행하는 데 필요한 환경을 제공하고 코드의 실행 결과를 실제로 관리하는 영역이다.
> 
- 식별자를 등록하고 관리하는 스코프와 코드 실행 순서 관리를 구현한 내부 메커니즘으로 ,모든 코드는 실행 컨텍스트를 통해 실행되고 관리된다.
    - 식별자와 스코프는 실행 컨텍스트의 렉시컬 환경으로 관리
    - 실행 순서는 실행 컨텍스트의 스택으로 관리

## 실행 컨텍스트 스택

```jsx
const x = 1;

function foo() {
	const y = 2;
	
	function bar() {
		const z = 3;
		console.log(x + y + z);
	}
	bar();
}

foo();
```

<aside>
💡 생선된 실행 컨텍스트는 스택 자료구조로 관리된다. 이를 실행 컨텍스트 스택이라고 부른다.

</aside>

### 스택

![stack.PNG](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e9c5d428-ffe0-4a57-9ac8-af4cdf8aa168/stack.png)

- 코드 평가 시 실행 컨텍스트가 스택에 푸시(PUSH) 된다.
- 함수의 경우 함수가 종료되면 해당 함수의 실행 컨텍스트를 실행 컨텍스트 스택에서 팝(POP) 되어 제거된다.
- 최종적으로 모든 코드가 종료된 경우 전역 실행 컨텍스트도 팝 되어 제거된다.

🔍 실행 컨텍스트 스택의 최상위 실행 컨텍스트는 현재 실행 중인 코드의 실행 컨텍스트이다. 이를 실행 중인 실행 컨텍스트라고 한다.

## 렉시컬 환경

```jsx
// 전역
const x = 1;

function foo() { // 지역(함수)
	const y = 2;

	console.log(x + y);
}
```

- 전역 렉시컬 환경 (Global Lexical Environment)
    - x : 1
    - foo : function obj
- foo 렉시컬 환경 (지역[함수] 렉시컬 환경)
    - y : 2

🔍 두 렉시컬 환경이 스코프 체인으로 연결되어있다.

- 렉시컬 환경은 키와 값을 갖는 객체 형태의 스코프를 생성하여 식별자를 키로 등록하고 식별자에 바인딩된 값을 관리한다.

<aside>
💡 렉시컬 환경은 스코프를 구분하여 식별자를 등록하고 관리하는 저장소 역할을 하는 렉시컬 스코프의 실체다.

</aside>

🔍 실행 컨텍스트는 LexicalEnvironment 컴포넌트와 VariableEnvironment 컴포넌트로 구성된다.

실행 컨텍스트 생성 이후 두 컴포넌트는 초기에 하나의 동일한 렉시컬 환경을 참조하지만 이후 몇 가지 상황을 만나면 VariableEnvironment 컴포넌트를 위한 새로운 렉시컬 환경을 생성하여 두 컴포넌트의 내용이 달리질 수도 있다.

### 렉시컬 환경의 구성 컴포넌트

- EnvironmentRecord (환경 레코드)
    - 스코프에 포함된 식별자를 등록하고 등록된 식별자에 바인딩된 값을 관리하는 저장소
- Outer Lexical Environment Reference (외부 렉시컬 환경에 대한 참조)
    - 상위 스코프를 가리키는 포인터
    - 외부 렉키설 환경에 대한 참조를 통해 단방향 링크드 리스트인 스코프 체인을 구현한다.

## 실행 컨텍스트의 생성과 식별자 검색 과정

```jsx
var x = 1;
const y = 2;

function foo (a) { 
	var x = 3;
	const y = 4;

	function bar (b) {
		const z = 5;
		console.log(a + b + x + y + z);
	}

	bar(10);
}

foo(20);
```

### 1. 전역 객체 생성

 전역 객체는 전역 코드가 평가되기 이전에 생성된다.

<aside>
💡 전역 객체에는 빌트인 전역 프로퍼티, 빌트인 전역 함수, 표준 빌트인 객체가 추가되며 동작 환경에 따라 클라이언트 사이드 Web API 또는 특정 환경을 위한 호스트 객체를 포함한다.
전역 객체도 프로토타입 체인의 일원이다.

클라이언트 사이트 Web API는 브라우저에 내장되어 있는 API를 말한다. 주로 웹 동작에 도움을 줄 수 있는 API가 있다. (DOM, BOM, XMLHttpRequest 등)

빌트인 전역 객체는 ECMAScript 사양에 정의된 객체를 말한다. (별도 선언 없이 전역 변수처럼 언제 어디서나 참조 할 수 있다.)

</aside>

### 2. 전역 코드 평가

 소스코드가 로드 된 후 자바스크립트 엔진이 전역 코드를 평가한다.

### 전역 코드 평가 절차

> 전역 실행 컨텍스트 생성 → 전역 렉시컬 환경 생성 → 전역 환경 레코드 생성 → 객체 환경 레코드 생성 → 선언적 환경 레코드 생성 → this 바인딩 → 외부 렉시컬 환경에 대한 참조 결정
> 
1. 전역 실행 컨텍스트 생성
    - 비어있는 전역 실행 컨텍스트 생성 후 실행 컨텍스트 스택에 푸시 (첫 실행 컨텍스트이기 때문에 실행 중인 실행 컨텍스트가 된다.)
2. 전역 렉시컬 환경 생성
    - 전역 렉시컬 환경을 생성하고 전역 실행 컨텍스트에 바인딩한다.
    1. 전역 환경 레코드 생성
        1. 객체 환경 레코드 생성
            - var 키워드로 선언된 전역 변수와 함수 선언문으로 정의된 전역 함수가 전역 환경 레코드의 객체 환경 레코드에 연결된 BindingObject를 통해 전역 객체의 프로퍼티와 메서드가 된다.
        2. 선언적 환경 레코드 생성
            - let, const 키워드로 선언한 전역 변수는 선언적 환경 레코드에 등록된다.
                - 해당 변수는 초기화 단계(런타임에 실행 흐름이 변수 선언문에 도달) 전까지 일시적 사각지대에 빠지게 된다.
    2. this 바인딩
        - 전역 환경 레코드의 [GlobalThisValue] 내부 슬롯에 this가 바인딩 됨 (전역 객체가 바인딩 됨)
    3. 외부 렉시컬 환경에 대한 참조 결정
        - 현재 평가 중인 소스코드가 전역 코드라면 외부 렉시컬 환경에 대한 참조에 null이 할당된다.

### 

### 3. 전역 코드 실행

 변수 할당문이 선언된다

- 전역 변수 x, y에 값이 할당되고 foo 함수가 호출된다.
    - 변수 할당 및 함수 호출을 위해 선언된 식별자인지를 확인 → 식별자 결정
    - 식별자 결정은 실행 중인 실행 컨텍스트에서 식별자를 검색하기 시작.

### 4. foo 함수 코드 평가

 foo 함수 호출 시 전역 코드의 실행을 일시 중단하고 foo 함수 내부로 코드의 제어권이 이동한다. → 이후 함수 코드 평가 시작

### 함수 코드 평가 절차

1. 함수 실행 컨텍스트 생성
2. 함수 렉시컬 환경 생성
    1. 함수 환경 레코드 생성
        - 매개변수, arguments 객체, 함수 내부 선언한 지역 변수와 중첩 함수를 등록
    2. this 바인딩
    3. 외부 렉시컬 환경에 대한 참조 결정
        - 전역 렉시컬 환경의 참조가 할당됨

### 5. foo 함수 코드 실행

 foo 함수의 내부 소스코드가 순차적으로 실행된다.

- 식별자 결정을 위해 실행 중인 실행 컨텍스트의 렉시컬 환경에서 식별자를 검색

### 6. bar 함수 코드 평가

 foo 함수 내에서 bar 함수를 호출하게 되면 bar 함수 코드에 대한 평가가 시작된다. (foo 함수 코드 평가 참고)

### 7. bar 함수 코드 실행

 bar 함수의 내부 소스코드가 순차적으로 실행된다.

### bar 함수에서 ‘console.log()’; 문이 실행되는 과정

1. console 식별자 검색
    - ‘console’ 식별자를 스코프 체인에서 검색한다.
        - 현재 실행 중인 실행 컨텍스트는 bar 함수 실행 컨텍스트이기 때문에 bar 함수 실행 컨텍스트의 bar 함수 렉시컬 환경에서 먼저 검색한다.
        - console 식별자가 없으므로 상위 스코프, 즉 bar 함수의 외부 렉시컬 환경에 대한 참조가 가리키는 foo 함수 렉시컬 환경으로 이동하여 검색한다.
        - foo 함수에도 console 식별자가 없으므로 또 상위 스코프로 이동하여 검색한다.
        - console 식별자는 객체 환경 레코드의 BindingObject를 통해 전역 객체에서 찾을 수 있기 때문에 전역 렉시컬 환경에서 검색이 된다.
2. log 메서드 검색
    - 검색된 console 식별자에 바인딩된 log 메서드를 검색한다. (console 객체의 프로토타입 체인을 통해 검색)
3. 표현식 평가 
    - 표현식에 사용되는 각 식별자를 스코프 체인에서 검색한다.
4. console.log 메서드 호출
    - 평가되어 생성한 값을 메서드에 전달하여 호출한다.

### 8. bar 함수 코드 실행 종료

  모든 코드가 실행되어 bar 함수 실행 컨텍스트가 스택에서 pop 된다

<aside>
💡 실행 컨텍스트가 제거되었다고해서 렉시컬 환경까지 즉시 소멸되는 것은 아니다. 만약 누군가 렉시컬 환경을 참조하고 있다면 렉시컬 환경은 소멸하지 않는다.

</aside>

### 9. foo 함수 코드 실행 종료

 마찬가지로 모든 코드가 실행되어 foo 함수 실행 컨텍스트가 스택에서 pop 된다.

### 10. 전역 코드 실행 종료

 더 이상 실행할 전역 코드가 없어 실행이 종료되고 전역 실행 컨텍스트도 스택에서 pop 된다.

## 실행 컨텍스트와 블록 레벨 스코프

```jsx
let x = 1;

// if  문 실행 시 새로운 렉시컬 환경을 생성
// 실행 컨텍스트가 생성 되는 것이 아니기 때문에 전역 실행 컨텍스트의 렉시컬 환경이 if문 블록에 대한 렉시컬 환경을 가리키도록 함. (교체)
if (true) {
	let x = 10;
	console.log(x); // 10
}
// if 문이 종료 되었을 때 전역 실행 컨텍스트의 렉시컬 환경을 다시 전역 렉시컬 환경을 가리키도록 되돌린다.

console.log(x); // 1
```