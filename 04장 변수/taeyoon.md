# 4_변수

변수 = 하나의 값을 저장하기 위해 확보한 메모리공간 자체

⭕ 메모리는 데이터를 저장할 수 있는 **메모리 셀의 집합체**

메모리 셀의 하나의 크기는 **1바이트(8바이트)**이며

각 셀은 고유의 메모리 주소를 갖는다 

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5636af73-5cb1-4864-a141-2fe42a292123/Untitled.png)

```jsx
//하나의 값 저장
let userID = 1;
let UserName = 'kim';

//객체나 배열로 자료구조 사용해서 여러 값 저장(그륩화)
let user ={
	id : 1,
	name : 'kim'
}

let user = [
	{id:1, name:'kim'},
	{id:2, name:'lee'}
]
```

```jsx
var result = 10 + 20 ;
//result = 식별자 
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b4d3be67-bd2a-42a2-816e-5e822aee9a05/Untitled.png)

변수에 값을 저장하는 것을 **할당**

변수에 저장된 값을 읽어 들이는 것을 **참조**

### 식별자

식별자는 값이 아닌 **메모리 주소를 기억** !!

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/72465c1b-53a9-49a3-a453-7e923ce45b39/Untitled.png)

```jsx
var scroe; //변수 선언

//메모리 주소에는 빈값이 아닌 undefined가 암묵적 할당되어 초기화

//var키워드만!
```

⚠️**undefined는 원시 타입의 값**

### 변수 선언의 실행 시점과 변수 호이스팅

```jsx
console.log(score); // undefined

var score; // 변수 선언
```

변수 선언이 소스 코드가 한 줄씩 순차적으로 실행되는 시점,

즉 **런타임이 아니라 그 이전 단계에서 먼저 실행**되기 때문에 undefined 반환

//호이스팅참고

### 값의 할당

**변수 선언 ⇒ 런타임 이전** 

**값의 할당 ⇒ 런타임 시점**

```jsx
console.log(score); //undefined;

var score; //변수 선언
scroe = 20; //값의 할당

console.log(score); // 80
```

```jsx
console.log(score) // undefined

var score = 80; //할당은 런타임 시점

console.log(score); // 80
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/89437dcf-e754-4fed-a110-2086f3f79012/Untitled.png)

**새로운 메모리 공간에 값을 할당**

### 값의 재할당

```jsx
var score = 80; //선언 및 할당
score = 90; //재할당
```

**만약 값을 재할당할 수 없어서 변수에 저장된 값을 변경할수 없다면 변수가 아니라 상수 !!**