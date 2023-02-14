# 20. strict mode

```jsx
function foo() {
	x = 10;
}
foo();

console.log(x); // 10
```

> 개발자의 의도와는 상관없이 발생한 암묵적 전역은 오류를 발생시키는 원인이 될 수 있다.
> 

<aside>
💡 오류를 줄여 안정적인 코드를 생산하기 위해 ES5 부터 strict mode가 추가되었다.

</aside>

strict mode 는 **자바스크립트 언어의 문법을 좀 더 엄격히 적용하여 오류를 발생시킬 가능성이 높거나 자바스크립트 엔진의 최적화 작업에 문제를 일으킬 수 있는 코드에 대해 명시적인 에러를 발생시킨다.**

🔎 Lint 도구라는 것을 사용하여 strict mode와 유사한 효과를 얻을 수 있다.

---

## strict mode 적용

```jsx
'use strict';

x = 10; // error
```

```jsx
function strictFunc() {
	'use strict';
	x = 10;
}

strictFunc(); // error
```

```jsx
<script>
	'use strict';
</script>
<script>
	x = 1; // -> 에러가 발생하지 않는다.
</script>
```

### 전역에 엄격 모드를 적용하는 것은 바람직 하지 않다.

- 여러 라이브러리를 사용하는 경우 각 라이브러리의 엄격 모드 적용 여부가 다르다면 엄격 모드가 혼용되어 오류를 발생시킬 수 있다.

### 함수 단위로 엄격 모드를 적용하는 것 또한 바람직하지 않다.

- 모든 함수에 일일이 엄격 모드를 적용하는 것은 상당히 번거롭다.

```jsx
(function() {
	let = 10;
	
	function strictFunc() {
		'use strict';

		let = 20; // strict mode reserved word
	}

	strictFunc();
}());
```

<aside>
💡 엄격 모드는 즉시 실행 함수로 감싼 스크립트 단위로 적용하는 것이 바람직하다.

</aside>

## 엄격 모드가 발생시키는 대표적인 에러

```jsx
(function() {
	'use strict';

	x = 1; // is not defined
}());
```

```jsx
(function() {
	'use strict';

	// error : Duplicate parameter
	function dupParam(x, x) {};
}());

// non-strict
function dupPram2(y, y) {
	console.log(y);
}
dupParam2(1, 2); // 2
```

```jsx
(function() {
	'use strict';

	var x = 1;
	delete x; // error

	function deleteFunc(y) {
		delete y; // error
	}
	delete deleteFunc; // error
}());
```

```jsx
(function() {
	'use strict';

	with({ x: 1 }) { // SyntaxError
		console.log(x);
	}
}());
```

```jsx
with({ x: 10, y: 20}) {
	console.log(x + y); // 30
}
```

## 엄격 모드 적용에 의한 변화

```jsx
(function() {
	'use strict';

	function findThis() {
		console.log(this);
	}
	findThis(); // undefined

	function CreateFuncThis() {
		console.log(this);
	}
	new CreateFuncThis(); // CreateFuncThis
}());
```

```jsx
(function(a) {
	'use strict';

	a = 2;
	
	console.log(a); // 2

	console.log(arguments); // { 0: 1, length: 1 }
}(1));
```