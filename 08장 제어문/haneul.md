# 8. 제어문

## 블록문

```jsx
{ // 블록문
	let x = 1;
}

let y = 2;
if (y < 10) { // 제어문
	y++;
}

function sum(a, b) { // 함수 선언문
	return a + b;
}
```

## 조건문

### if … else

```jsx
if (/*조건식*/) {
	// 조건식1이 참인 경우 실행
} else if (/*조건식2*/ {
	// 조건식2이 참인 경우 실행
} else {
	// 모든 조건식이 거짓인 경우 실행
}
```

### switch

```jsx
switch(/*표현식*/) {
	case 표현식1:
		// switch 문의 표현식과 표현식1이 일치하면 실행됨
		break; // break 문이 없다면 해당 케이스가 실행되면 이후 케이스가 모두 실행된다.
	case 표현식2:
		// switch 문의 표현식과 표현식2가 일치하면 실행됨
		break;
	default:
		// switch 문의 표현식과 일치하는 case 문이 없을 때 실행
}
```

## 반복문

### for

```jsx
for (/* 변수 선언문 or 할당문 ; 조건식 ; 증감식 */) {
	// 조건식이 참인 경우 반복 실행될 문
}

for (;;) { ... } // 무한루프
```

### while

```jsx
while(/* 조건식 */) {
	// 조건식의 결과가 참이면 코드 블록을 실행
}

while(true) {
	// 무한 루프

	// 무한 루프를 방지하지 위해 탈출을 위한 조건을 만들어 두는것이 좋다.
	if (true) {
		break;
	}
}

do {
	// 조건식을 평가하기 전에 먼저 실행 한 뒤 조건식을 평가한다.
} while (/* 조건식 */)
```

## break문

> 레이블 문, 반복문 또는 switch 문의 코드 블록을 탈출한다. (그 외에 break 문을 사용하면 에러 발생한다.)
> 

```jsx
if (true) {
	break; // error -> Illegal break statement
}

// 식별자가 붙은 레이블 문
lab: console.log('lab');

// 식별자가 붙은 레이블 블록문
lab: {
	console.log(1);
	break lab; // lab 레이블 블록문을 탈출한다.
	console.log(2);
}

// 식별자가 붙은 레이블 for 문
outer: for (var i = 0; i < 3; i++) {
	for (var j = 0; j < 3; j++) {
		if (i + j === 3) {
			break outer; // 외부 for 문을 탈출한다..
		}
		console.log('inner');
	}
}

// 중첩된 for 문 외부로 탈출하는데 레이블을 사용할 순 있지만 그 밖의 경우에는 권장하지 않는다.
```

## continue문

> 반복문의 코드 블록 실행을 continue 문이 실행된 시점에서 중단하고 반복문의 증감식으로 실행 흐름을 이동시킨다. → 탈출 x
> 

```jsx
for (var i = 0; i < 3; i++ ) {
	if (i < 3) {
		continue;
	}

	console.log(i); // continue 문이 실행되면 이 문은 실행되지 않는다.
}
```