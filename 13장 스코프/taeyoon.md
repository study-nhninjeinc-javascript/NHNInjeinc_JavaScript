# 13_scope

//아직 다 정리 안된 내용입니다!

자바스크립트 코드를 실행시에  엔진은 콜 스택이라는 곳에 전역 실행컨텍스트를 담음

call stack 에선 최근에 추가된 실행 컨텍스트만 활성화 

호출스택(callstack) 

⇒함수를 실행하면 해당 함수의 기록을 스택 맨 위에 push합니다 

 결과 값을 반환하면 스택에 쌓여있던 함수는 pop

// callstack을 이해하기 위해 비동기 콜백 등 다양한 내용을 이해해야하므로 더 자세한 내용을 알아야합니다/////////

**Hositing**

선언문이 마치 최상단에 끌어올려진 듯한 현상

//Environement Record

식별자와 식별자에 바인딩된 값을 기록하는 객체

 

```jsx
console.log(hoTe); // undefined햐

var hoTe = 'value';

console.log(hoTe) // value
```

Variavle Hositing vs Function Hositing

Variavle Hositing

### 생성단계

var = 실행컨텍스트의 생성단계에서 선언문만 실행해서 Record에 기록 (미리 기록)

          **hote : undefined; 상태**

### 실행단계

선언문 외 나머지 코드 순차적 실행 (생성단계를 참조 및 업데이트)

          hote : value라고 업데이트

var 특징

선언 = 메모리 공간을 확보하고 식별자와 연결

초기화 = 식별자에 암묵적으로 undefined 값 바인딩

**선언과 초기화가 동시에 이루어짐**

const

hote를 기록은 하지만 undefined로 초기화 X

⇒선언문 이전에 hote를 참조하려면 Reference Error 발생

let, const  특징

선언 = 메모리 공간을 확보하고 식별자와 연결

초기화는 실행 X

선언문 전까지는 참조시에 Reference Error 

Function Hositing

```jsx
/* Global */
f(); // Type Error  // f는 undefined이기 때문에 호출 X

var f = () =>{
	//something..
}

========================

/* Global */
ff(); // Reference Error  // 아무 값도 없기 때문에 참조에러

const ff = () =>{
	//something..
}

//함수 선언문은 이전에 다뤘던 내용이기에 간단히
//선언과 동시에 함수가 생성되어 실행 가능
```

외부환경참조

outer Environment Reference //바깥 렉시컬환경을 가리킴 

record outer 합쳐서 lexical Environment라고 함 정적 환경 또는 렉시컬 환경

### **식별자 결정**

//

변수섀도잉 가장 가까운 변수를 참조하기에 상위 스코프의 변수값은 무시

스코프 체인 식별자를 결정할 때 활용하는 스코프들의 연결리스트 타고타고타고타고

실행컨텍스

⇒코드를 실행하는데 필요한 환경을 제공하는 객체