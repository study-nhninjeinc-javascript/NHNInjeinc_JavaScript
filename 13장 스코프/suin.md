# 13장 스코프

## 13.1 스코프란?

- **모든 식별자는 자신이 선언된 위치에 의해 다른 코드가 식별자 자신을 참조할수있는 유효범위가 결정된다**. → 이를 스코프라고 한다.
사전 - **scope** : 명) 기회[여지/능력], **범위**

```jsx
function add(x,y){
		//매개변수는 함수 몸체 내부에서만 참조 할수 있다.
		//-> 매개변수의 스코프(유효범위)는 함수 몸체 내부다.
    console.log(x, y);
		return x+y;
}
add(2, 5);//2 5
console(x, y); //Uncaught ReferenceError: x is not defined

var var1 = 1;
if(true){
		var var2 = 2;
		if(true){
			var2 = 3;
		}
}

function foo(){
		var var4 = 4;
		function bar(){
			var var5 = 5;
		}
}
console.log(var1);//1
console.log(var2);//3
console.log(var3);//Uncaught ReferenceError: var3 is not defined
console.log(var4);//Uncaught ReferenceError: var4 is not defined
console.log(var5);//Uncaught ReferenceError: var5 is not defined
```

- **식별자 결정** : 자사스크립트 엔진이 같은 두개의 변수 중에서 어떤 변수를 참조해야할것인지 결정 하는 것

```jsx
var x = 'global';
funcntion foo(){
		var x = 'local';
		console.log(x);//local
}
foo();
console.log(x);//global
//왜 1번은 local 2번은 global일까
//foo안의 x는 함수내부에서 선언되어 내부에서만 참조가능
//밖의 x는 전체에서 사용가능
//그렇다면 foo안에서 x는 왜? 내부것으로 참조가 될까?
//var - 함수스코프를 따름, 자신이 선언된 곳과 가장 가까운 함수를 유효범위로 가짐
//let const - 블록 스코프를 따르는데 자신이 선언된 곳과 가장 가까운 블록을 유효범위로 가짐
//일단 이부분 스코프 체인에서 나중에 할 것 같다. -일단 패스

```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b0fdb596-a1d4-485b-8da6-8ca45093959f/Untitled.png)

- 따라서 스코프란 **자바스크립트 엔진이 식별자를 검색할때 사용하는규칙**이라고도 할수있다.
- 자바스크립트 엔진은 코드를 실행할때 코드의 문맥을 고려
    - 코드 문맥과 환경
        
        코드가 어디서 실행되며 주변에 어떤코드가 있는지를 렉시컬환경이라고 부른다.
        → 코드의 문맥은 렉시컬 환경으로 이뤄진다. 이를 구현한것이 실행컨텍스트 이며 모든 코드는 실행 컨텍스트에서 평가되고 실행된다. - 23장에서 곧 살펴볼 예정
        
- 식별자는 어떤 값을 구별할 수있어야 하므로 유일해야한다. → 식별자인 변수이름은 중복될수없다. → 하나의 값은 유일한 식별자에 연결되어야한다.
- 그런데 위 소스는 변수명이 같으나 스코프가 다르기 때문에 별개의 변수임 - 헷갈리지 말것
- 프로그래밍 언어에서는 스코프를 통해 식별자인 변수 이름의 충돌을 방지
→ 스코프내에서 식별자는 유일해야하지만, 다른 스코프에서는 같은 이름의 식별자를 사용할수 있게한다.
→ 스코프는 네임스페이스다.

var 키워드로 선언된 변수는 같은 스코프내에서 중복선언이 허용이 된다. - 이는 의도치않게 변수값이 재할당 되어 변경되는 부작용을 발생시킨다.

→ let이나 const는 중복선언 허용하지 않는다.

```jsx
function foo(){
		var x = 1;
		var x = 2;
		console.log(x);//2
}
foo();
function bar(){
		let x = 1;
		let x = 2;//Uncaught SyntaxError: Identifier 'x' has already been declared
}
bar();
```

식별자? : 어떤 값을 구별하여 식별해 낼수있는 고유한 이름

네임 스페이스 : 

## 13.2 스코프의 종류

## 13.3 스코프 체인

## 13.4 함수레벨 스코프

## 13.5 렉시컬 스코프