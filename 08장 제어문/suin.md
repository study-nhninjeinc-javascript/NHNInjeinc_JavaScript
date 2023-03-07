# 08장 제어문

- 제어문은 조건에따라 코드블록을 실행하거나 반복실행할때 사용한다.
- 제어문을 사용하면 코드의 실행 흐름을 인위적으로 제어할수있다.
- 하지만 코드의 실행 순서가 변경된다는 것은 단순히 위에서 하래로 순차적으로 진행하는 직관적인 코드의 흐름을 혼란스럽게 만든다.
→ 따라서 제어문은코드의 흐름을 이해하기 어렵게 만들어 가독성을 해치는 단점이있다.
- 나중에 살펴볼 forEach, map, filter, reduce 같은 고차함수를 함수형 프로그래밍 기법에서는 제어문의 사용을 억제하여 복잡성을 해결하려고 노력한다.

## 8.1 블록문

- 블록문 : 0개 이상의 문을 중괄호로 묶은것
- 자바스크립트는 블록문을 하나의 실행단위로 취급한다.
- 블록문은 단독으로 사용할 수도 있으나 일반적으로 제어문이나 함수를 정의할 때 사용하는 것이 일반적이다.
- 블록문은 언제나 문의 종료를 의미하는 자체 종결성을 갖기 때문에 블록문의 끝에는 세미콜론이 붙지 않는다.

```jsx
{
	var foo = 10;
}
```

## 8.2 조건문

- 조건문은 주어진 조건식의 평가 결과에 따라 코드 블록의 실행을 결정한다.
- 자바스크립트는 if…else문과 switch문으로 두가지 조건문을 제공한다.

### if…else문

- if…else문은 주어진 조건식의 평가 결과(논리적 참 또는 거짓)에 따라 실행할 코드 블록을 결정한다.
- 조건식의 결과가 true일경우 if 문의 코드블록이 실행되고 false의 경우 else문의 코드 블록이 실행된다.
- if문의 조건식은 불리언값으로 평가되어야하지만, 불리언이 아닌경우 암묵적으로 불리언 값으로 강제 변환된다.

```jsx
if (조건식) {
    // 조건식이 참이면 이 코드 블록이 실행된다
}else{
	// 조건식이 거짓이면 이 코드 블록이 실행된다
}

//조건식을 추가하여 조건에 따라 실행될 코드 블록을 늘리고 싶으면 else if문을 사용
if (조건식1) {
    // 조건식1이 참이면 이 코드 블록이 실행된다
}else if(조건식2){
	// 조건식1이 거짓이면서 조건식2이 참이면 이 코드 블록이 실행된다
}else{
	// 조건식1, 조건식2이 거짓이면 이 코드 블록이 실행된다
}

//코드 블록내의 문이 하나라면 중괄호를 생략할 수 있다.
var num = 0;
var kind;
if(num==0)
	kind = '영';
console.log(kind);//영

//불리언이 아닌경우 암묵적으로 불리언 값으로 강제 변환
var x = 3;
var result;
if(x % 2)
	result = '홀';
else
	result = '짝';

console.log(result);//홀

//삼항조건 연산자로 바꿔 쓸 수 있다
x++;
result = x % 2 ? '홀' : '짝';
console.log(result);//짝
```

### switch문

- switch문은 주어진 표현식을 평가하여 그 값과 일치하는 표현식을 갖는 case문으로 실행 흐름을 옮긴다.
- case문은 상황을 의미하는 표현식을 지정하고 콜론으로 마친다. 그 뒤에 실행할 문들을 위치시킨다.
- 일치하는 case문이 없다면 default문으로 이동한다. default 문은 선택사항이다.
- break문으로 코드 블록에서 탈출할수 있도록 넣어 주어야한다.

```jsx
switch (표현식) {
    case 표현식1: 
        // 문의 표현식과 표현식1이 일치하면 실행될 문;
        break;
    case 표현식2:
        // 문의 표현식과 표현식2이 일치하면 실행될 문;
        break;
    default:
         // 문의 표현식과 일치하는 case문이 없을때 실행될 문;
}

//break문을 사용하지 않아 다음 case문으로 실행이 흘러서 
//str에는 default라는 값이 들어가게된다
var num = 1;
var str;
switch (num) {
    case 0: str = '0입니다';
    case 1: str = '1입니다';
    default: str = 'default';
}
console.log(str);//default

//break문을 사용해야 코드블록에서 탈출할수있다.
var year = 1996;
var animal;
switch (year % 12) {
    case 0 : animal = '원숭이 입니다'; break;
    case 1 : animal= '닭 입니다'; break;
    case 2 : animal= '개 입니다'; break;
    case 3 : animal= '돼지 입니다'; break;
    case 4 : animal = '쥐 입니다'; break;
    case 5 : animal= '소 입니다'; break;
    case 6 : animal= '호랑이 입니다'; break;
    case 7 : animal= '토끼 입니다'; break;
    case 8 : animal= '용 입니다'; break;
    case 9 : animal= '뱀 입니다'; break;
    case 10 : animal= '말 입니다'; break;
    case 11 : animal= '양 입니다'; break;
    default: animal= 'default'; break;
}
console.log(animal);//default
```

## 8.3 반복문

- 반복문은 조건식의 평가 결과가 참인 경우 코드블록을 실행한다.
그 후 조건식을 다시 평가하여 여전히 참인 경우 코드 블록을 다시 실행하여 조건식이 거짓일때까지 반복한다.

### for문

```jsx
for (변수 선언문 또는 할당문; 조건식; 증감식) {
    // 조건식이 참인경우 반복 실행될 문
}

for(var i = 0; i < 2; i++){
	console.log(i);
}
//1. i = 0 실행
//2. 조건식 실행 0 < 2므로 true
//3. 코드블록 실행 0 출력
//4. i++실행
//5. 조건식 실행 1 < 2므로 true
//6. 코드블록 실행 1 출력
//7. i++실행
//8. 조건식 실행 2 < 2므로 false -> 실행문 종료

//무한루프
for(;;){}
```

### while문

- for문은 반복횟수가 명확할때 주로사용, while문은 반복횟수가 불명확할 때 주로사용
- while문의 조건문의 평가결과가 거짓이 되면 코드 블록을 실행하지 않고 종료한다.
불리언 값이아니면 불리언 값으로 강제변환한다.

```jsx
while (조건식) {
    // 조건식이 참인경우 반복 실행될 문
}

var count = 0;
while (count < 3) {
  console.log(count);
	count++
}

//조건식의 평가결과가 언제나 참이되면 무한루프가 된다.
while(true){...}

//무한루프에서 탈출하기 위해서는
//코드 블록내에 if문으로 탈출조건을 만들어 break문으로 코드블록을 탈출한다.
var x = 0;
while(true){
	console.log(x);
	x++;
	if(x==3) break;
}
```

### do…while문

- 코드 블록을 먼저 실행하고 조건식을 평가한다.
→ 코드블록은 무조건 한번 이상 실행된다.

```jsx
var count = 0;
do{
	console.log(count);//0 1 2
}while(count < 3);
```

## 8.4 break문

- break문은 코드 블록을 탈출한다.
좀 더 정확하게 표현하면 레이블문, 반복문 또는 switch문의 코드 블록을 탈출한다.
- 레이블문, 반복문, switch문의 코드 블록 외에 break문을 사용하면 SyntaxError(문법에러)가 발tod한다
- 레이블문이란 식별자가 붙은 문을 말한다.
레이블 문은 중첩된 for문 외부로 탈출할때 유용하지만 그밖의 경우에는 일반적으로 권장하지 않는다. → 프로그램 흐름이 복잡해져서 가독성이 나빠진다.

```jsx
if(true){
	break;
}

//foo라는 레이블 식별자가 붙은 레이블 문
foo : console.log('foo');

foo : {
	console.log(1);
	break foo;//foo 레이블 블록문을 탈출한다.
	console.log(2);
}
console.log('Done!');

//outer라는 식별자가 붙은 레이블 for문
outer : for(var i = 0; i < 3; i++){
	for(var j = 0; j < 3; j++){
		if(i + j === 3) break outer;
		console.log(`inner [${i}, ${j}]`);
	}
}
//inner [0, 0]
//inner [0, 1]
//inner [0, 2]
//inner [1, 0]
//inner [1, 1]
```

## 8.5 continue문

- continue문은 반복문의 코드 블록 실행을 현지점에서 중단하고 반복문의 증감식으로 실행 흐름을 이동시킨다.