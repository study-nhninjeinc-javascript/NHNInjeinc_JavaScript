## 20.1 strict mode란?

```jsx
function foo(){
	x = 10;
}
foo();//undefined
console.log(x); //10
//전역 스코프에도 x변수의 선언이 존재하지 않아 ReferenceError를 발생시킬 것 같지만
//자바스크립트 엔진은 암묵적으로 전역객체에 x프로퍼티를 동적 생성한다.
//전역객체의 x프로퍼티는 마치 전역변수처럼 사용할 수 있다.
```

- 암묵적전역 : 암묵적으로 전역객체에 프로퍼티를 동적생성 하는 것
- 개발자의 의도와는 상관없이 발생한 암묵적 전역은 오류를 발생 시키는 원인이 될 가능성이 크다. → var let const 키워드를 사용하여 변수를 선언한 다음 사용해야한다
- 이를 지원하려고 ES5부터 strict mode(엄격모드) 추가
→ 자바스크립트 문법을 좀 더 엄격히 적용하여 오류를 발생시킬 가능성이 높거나 자바스크립트 엔진의 최적화 작업에 문제를 일으킬수 있는 코드에 대해 명시적인 에러를 발생시킨다.
- ESLint 같은 린트 도구를 사용해도 strict mode와 유사한 효과를 얻을수있다.
정적 분석 기능을 통해 소스를 실행하기 전에  소스코드를 스캔하여 문법적 오류만이 아니라 잠재적 오류까지 찾아내고 오류의 원인을 리포팅해주는 유용한 도구

## 20.2 strict mode의 적용

- 적용하려면 함수 몸체의 선두에 ‘use strict’;를 추가.
적역 선두에 추가하면 strict mode가 적용

```jsx
'use strict';
function foo(){
	x = 10;
}
foo();//Uncaught ReferenceError: x is not defined
```

- 함수몸체의 선두에 추가하면 중첩 함수에 strict mode가적용

```jsx
function foo(){
	'use strict';
	x = 10;
}
foo();////Uncaught ReferenceError: x is not defined
```

- 코드의 선두에 ‘use strict’;를 위치시키지 않으면 strict mode가 동작하지 않는다.

```jsx
function foo(){
	x = 10;
	'use strict';
}
foo(); //undefined
```

## 20.3 전역에 strict mode를 적용하는 것은 피하자

- 전역에 적용한 strict mode는 스크립트 단위로 적용된다.
- 스크립트 단위로 적용된 strict mode 다른 스크립트에 영향을 주지 않고 한정되어 적용된다.
- strict mode, non-strict mode 혼용하는 것은 오류를 발생시킬수있다.
~~특히 외부 서드파티 라이브러리를 사용하는 경우 라이브러리 non-strict mode인 경우도 있기 때문에 strict mode를 적용하는 것은 바람직하지 않다.~~ 그냥 다른곳에서는 안쓰는 경우가 있으니까 쓰는곳만 쓰자..
→ 이경우 즉시 실행 함수 선두에 strict mode를 적용한다.

```jsx
<script>
	'use strict';
</script>
<script>
	x = 1; //에러 발생 x
	console.log(x);//1
</script>
<script>
	'use strict';
	y = 1; //Uncaught ReferenceError: y is not defined
	console.log(y);
</script>

<script>
	(function(){
		'use strict';
	}());
</script>
```

## 20.4 함수 단위로 strict mode를 적용하는 것도 피하자

- 어떤 함수는 strict mode를 적용하고 어떤함수는 strict mode를 적용하지 않는 것은 바람직하지 않다. 또한, 모든 함수에 일일이 strict mode를 적용하는것도 번거로운일이다.
참조할 함수 외부의 컨텍스트에 strict mode를 적용하지 않는다면 또한 문제가 될수있다.
→ 즉시실행함수로 감싼 스크립트 단위로 적용하는 것이 바람직하다

```jsx
(function(){
	var let = 10;
	function foo(){
		'use strict';
		let = 20; //Uncaught SyntaxError: Unexpected strict mode reserved word
	}
	foo();
}());
```

## 20.5 strict mode가 발생시키는 에러

### 20.5.1 암묵적 전역

- 선언하지 않은 변수를 참조하면 ReferenceError가 발생한다.

```jsx
(function(){
	'use strict';
	x = 1;
	console.log(x);//Uncaught ReferenceError: x is not defined
}());
```

### 20.5.2 변수, 함수, 매개변수의 삭제

- delete연산자로 변수, 함수 매개변수를 삭제하면 SyntaxError가 발생한다.

```jsx
(function(){
	'use strict';
	var x = 1;
	delete x;//Uncaught SyntaxError: Delete of an unqualified identifier in strict mode.
	
	function foo(a){
		delete a;//Uncaught SyntaxError: Delete of an unqualified identifier in strict mode.
	}
	delete foo;//Uncaught SyntaxError: Delete of an unqualified identifier in strict mode.
}());
```

### 20.5.3 매개변수 이름의 중복

- 중복된 매개변수 이름을 사용하면 SyntaxError가 발생한다.

```jsx
(function(){
	'use strict';
	//Uncaught SyntaxError: Duplicate parameter name not allowed in this context
	function foo(x,x){
		return x+x;
	}
}());
```

### 20.5.4 with문의 사용

- with문을 사용하면 SyntaxError가 발생
- with문은 전달된 객체를 스코프 체인에 추가한다.
with문은 동일한 객체의 프로퍼티를 반복해서 사용할때 이름을 생략할 수 있어서 코드가 간단해지는 효과가 있지만 성등과 가독성이 나빠지는 문제가 있다. → with문은 사용하지 않는 것이 좋다.

```jsx
(function(){
	'use strict';
	//Uncaught SyntaxError: Strict mode code may not include a with statement
	with({x:1}){
		console.log(x);
	};
}());
```

## 20.6 strict mode 적용에 의한 변화

### 20.6.1 일반함수의 this

- strict mode에서 일반함수로서 this에 undefined가 바인딩된다.
- → 일반 함수 내부에서 this를 사용할 필요가 없기 때문 → 에러는 발생하지 않는다.

```jsx
(function(){
	'use strict';
	
	function foo(){
		console.log(this);//undefined
	}
	foo();
	
	function Foo(){
		console.log(this);//Foo	
	}
	new Foo();
}());
```

### 20.6.2 arguments 객체

- strict mode에서는 매개변수에 전달된 인수를 재할당하여 변경해도 arguments 객체에 반영되지 않는다.

```jsx
(function(a){
	'use strict';
	//매배변수에 전달된 인수를 재할당하여 변경
	a =2;
	//변경된 인수가 arguments 객체에 반영되지 않는다
	console.log(arguments);//Arguments [1, callee: (...), Symbol(Symbol.iterator): ƒ]
}(1));
```