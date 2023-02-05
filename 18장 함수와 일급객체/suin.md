# 18장 함수와 일급객체

## 18.1 일급객체

일급객체 : 다른객체들에 일반적으로 적용가능한 연산을 모두 지원하는객체
                  → 사용할때 다른 요소 들과 아무런 차별이 없다.

1. 무명의 리터럴로 생성할수 있다. → 런타임에 생성이 가능하다
2. 변수나 자료구조(객체, 배열 등)에 저장할 수있다.
3. 함수의 매개변수에 전달할수있다
4. 함수의 반환값으로 사용할수있다.

→ 자바스크립트의 함수는 위 조건을 모두 만족!  = 일급객체이다.

```jsx
//1. 무명의 리터럴로 생성할수 잇다. → 런타임에 생성이 가능하다
const increase = function (num){
	return ++num;
}

//2. 변수나 자료구조(객체, 배열 등)에 저장할 수있다.
const auxs = {increase};

//3. 함수의 매개변수에 전달할수있다.
//4. 함수의 반환값으로 사용할수있다.
function makeCounter(aux){
	let num = 0;
	return function(){
		num = aux(num);
		return num;
	}
}
const increaser = makeCounter(auxs.increase);
console.log(increaser);//1
```

이해가 잘안되서 찾아 봤는데 2 3 4번은 다 적혀있는데

“런타임 함수 생성 가능 여부” 이부분에서 저자마다 말이 달라진다고 한다.

→ C의 함수는 일급객체인가 이급객체인가?

일단 참고하고 넘어가자.

## 18.2 함수객체의 프로퍼티

함수는 객체 → 프로퍼티를 가질수있다.

 arguments, caller, length 등.. 함수객체의 데이터 프로퍼티이다.

__proto__는 접근자 프로퍼티 → 함수객체 고유의 프로퍼티가 아니다.

Object.prototype는 모든 객체가 상속받아 사용할수있다. → 함수도 Object.prototype객체로 부터 접근자 프로퍼티를 상속받는다.

```jsx
function square(num){
	return num*num;
}
console.dir(square);
//ƒ square(num)
console.log(Object.getOwnPropertyDescriptors(square));
//{length: {…}, name: {…}, arguments: {…}, caller: {…}, prototype: {…}}

//__proto__는 함수의 프로퍼티가 아니다.
console.log(Object.getOwnPropertyDescriptor(square, "__proto__"));
//undefined

//함수는 Object.prototype객체로 부터 __proto__접근자 프로퍼티를 상속받는다.
console.log(Object.getOwnPropertyDescriptor(Object.prototype, "__proto__"));
//{enumerable: false, configurable: true, get: ƒ, set: ƒ}
```

### 18.2.1 arguments 프로퍼티

- arguments객체는 함수호출시 전달된 인수들의 정보를 담고있는 **순회 가능한 유사 배열 객체**
함수내부에서 지역변수처럼 사용된다 → 함수 외부에서는 참조 X
- arguments프로퍼티는 ES3부터 표준에서 폐지되었다.
→ Function.arguments 같은 사용법은 권장 X → arguments 객체를 참조하자
- arguments 객체는 인수를 프로퍼티 값으로 소유 프로퍼티 키는 인수의 순서를 나타낸다.
arguments 객체의 callee프로퍼티는 호출되어 arguments를 생성한 함수 → 함수자신을 가리킨다.
arguments 객체의 length프로퍼티는 인수의 개수를 가르킨다.
- arguments 객체는 매개변수 개수를 확정할수 없는 가변인자 함수를 구현할때 유용하다.
    
    ```jsx
    function sum(){
    	let res = 0;
    	for(let i = 0; i < arguments){
    		res += arguments[i];
    	}
    	return res;
    }
    
    console.log(sum()); //0
    console.log(sum(1,2,3,4));//10
    ```
    
- arguments 객체는 유사배열객체, 배열이 아니므로 배열메서드를 사용할경우 에러가 발생한다.
→ 배열메서드를 사용하려면 function.prototype.call, function.prototype.apply를 사용해 간접 호출해야하는 번거로움이 있다. - 22장, 27장에서 살펴볼 예정 일단 다음에!
→ 이런 번거로움을 해결하기 위해 ES6에선 Rest파라미터를 도입
Rest 파라미터 : 매개변수 이름 앞에 세개의 점 …을 붙여서 정의한 매개변수를 의미 - 인수목록을 배열로 전달받는다.
    
    ```jsx
    function sum(...args){
    	//arr.reduce() : arr.reduce(callback[,initVal])
    	//배열의 각 요소에 대해 주어진 reducer함수를 실행하고, 하나의 결과값을 반환
    	//  callback함수의 인수 : 1: 콜백의 반환값 누적 2: 처리할 요소 3: 처리할 요소의 인덱스 4: 호출한 배열
      //  initVal : 초기값 제공 - 선택
    	return args.reduce((pre, cur)=>pre+cur,0);
    }
    console.log(sum(1,2,3,4));//10
    ```
    

### 18.2.2 caller 프로퍼티

- caller 프로퍼티는 ECMAScript 사양에 포함되지 않은 비표준 프로퍼티,,
- 함수 자신을 호출한 함수를 가르킨다.
→ 어디서 너를 불렀니? 뭐 이런 느낌?
- 함수를 호출한적 없으면 null을 가르킴

### 18.2.3 length 프로퍼티

- 함수를 정희할때 선언한 매개변수의 개수를 가르킨다.
- arguments 객체 length와 함수객체의 length 프로퍼티 값은 다를수있다.
왜냐? 자바스크립트는 매개변수와 인수의 개수가 같지 않아도 오류가 나지 않기 때문

### 18.2.4 name 프로퍼티

- 함수이름을 나타낸다.
- name 프로퍼티는 ES5와 ES6에서 동작을 달리한다.
ES5 : 빈문자열, ES6 : 함수객체를 가리키는 식별자를 값으로 갖는다.

### 18.2.5 __proto__ 접근자 프로퍼티

- 모든 객체는 [[Prototype]]이라는 내부슬롯을 갖는다.
[[Prototype]] 내부 슬롯은 객체지향 프로그램의 상속을 구현하는 프로토 타입 객체를 가리킨다.
- [[Prototype]]은 직접 접근 X __proto__ 접근자를 통해 간접적으로 객체에 접근 가능

```jsx
const obj = {a : 1};
//객체리터럴 방식으로 생성한 객체의 프로토타입 객체는 Object.prototype이다. 
console.log(obj.__proto__ === Object.prototype);
console.log(obj.hasOwnProperty("a"));//true
console.log(obj.hasOwnProperty("__proto__"));//false

let square = function (num){
	return num*num;
}

console.log(square.__proto__ === Function.prototype); //true
console.log(square.__proto__ === Object.prototype); //false
```

그럼 위에 함수는 Object.prototype객체로 부터 __proto__접근자 프로퍼티를 상속받는다. 말은 뭐가 되는거지..? 틀린건가?

__proto__ getter 함수는 객체의 내부 [[Prototype]] 값을 노출합니다. 객체 리터럴을 사용하여 생성된 객체의 경우 이 값은 Object.prototype
배열 리터럴을 사용하여 생성된 객체의 경우 이 값은 Array.prototype
함수의 경우 이 값은 Function.prototype입니다.
참조 : [https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/proto](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/proto)

### 18.2.6 prototype 접근자 프로퍼티

- protype 프로퍼티는 생성자 함수로 호출할 수 있는 함수객체 즉 constructor만이 소유하는 프로퍼티
- protype 프로퍼티는 함수가 객체를 생성하는 생성자 함수로 호출될때 생성자 함수가 생성할 인스턴스의 프로토타입 객체를 가르킨다.