# 9. 타입변환과 단축평가

## 타입 변환

### 명시적 타입 변환

> 개발자의 의도에 따라 값의 타입을 변환하는 것을 명시적 타입 변환 또는 타입 캐스팅이라 한다.
> 

```jsx
var x = 10; // number

var s = x.toString();
console.log(typeof s, s); // string 10
```

### 암묵적 타입변환

> 개발자의 의도와는 상관없이 표현식을 평가하는 도중에 자바스크립트 엔진에 의해 암묵적으로 타입이 자동 변환되는 것을 암묵적 타입변환 또는 타입 강제 변환이라 한다.
> 

```jsx
var x = 10; // number

var s = x + '';
console.log(ltypeof s, s); // string 10
```

## 암묵적 타입 변환

### 문자열 타입으로

```jsx
1 + '2'; // -> 12
// + 연산자의 피연산자 중 하나 이상이 문자열이므로 연결 연산자로 동작한다.

`1 + 1 = ${1 + 1}`; // -> 1 + 1 = 2
// 템플릿 리터럴 표현식 삽임은 표현식 평가 결과를 문자열 타입으로 암묵적 타입 변환한다.

-0 + ''; // "0"
-1 + ''; // "-1"
NaN + ''; // "NaN
Infinity + ''; // "Infinity"
-Infinity + ''; // "-Infinity"
true + ''; // "true"
false + ''; // "false"
null + ''; // "null"
undefined + ''; // "undefined"
```

### 숫자 타입으로

```jsx
1 - '1'; // 0
1 * '10'; // 10
1 / 'one'; // NaN

'1' > 0; // true -> 비교 연산자는 피연산자의 크기를 비교하므로 문맥상 모두 숫자타입이어야 한다. 즉, 숫자 타입이 아니면 숫자 타입으로 암묵적 타입 변환한다.

+true; // 1
+false; // 0

+null; // 0
+undefined; // 0
```

### 불리언 타입으로

```jsx
if ('') console.log('TRUE'); else console.log('FALSE'); // FALSE
if (0) console.log('TRUE'); else console.log('FALSE'); // FALSE
if ('str') console.log('TRUE'); else console.log('FALSE'); // TRUE
if (null) console.log('TRUE'); else console.log('FALSE'); // FALSE
if (NaN) console.log('TRUE'); else console.log('FALSE'); // FALSE
```

<aside>
💡 불리언 타입으로 암묵적 타입 변환 시 자바스크립트 엔진은 불리언 타입이 아닌 값을 Truthy 값 또는 Falsy 값으로 구분한다.

</aside>

## 명시적 타입 변환

### 문자열 타입으로

```jsx
String(1); // "1"
String(NaN); // "NaN"
String(Infinity); // "Infinity"
String(true); // "true"
```

### 숫자 타입으로

```jsx
Number('0'); // 0
Number('-1'); // -1
Number('10.53'); // 10.53
Number(true); // 1
Number(false); // 0

// 문자열만 가능한 방법
parseInt('0'); // 0
parseInt('-1'); // -1
parseInt('0'); // 0

// 산술 연산자 이용
+'0'; // 0
+'-1'; // -1
+true; // 1

'0' * 1; // 0
```

### 불리언 타입으로

```jsx
Boolean('x'); // true
Boolean(''); // false
Boolean('false'); // true

Boolean(0); // false
Boolean(1); // true
Boolean(NaN); // false

Boolean(null); // false
Boolean(undefined); // false
```

## 단축 평가

### 논리 연산자

> 논리합(||) 또는 논리곱(&&) 연산자 표현식은 언제나 2개의 피연산자 중 어느 한쪽으로 평가된다.
> 

‘Cat’ && ‘Dog’ // → “Dog”

- 논리곱 연산자는 두 피연산자가 모두 true로 평가될 때 true를 반환한다.
- 이때 논리곱 연산자는 좌항에서 우항으로 평가가 진행되는데 논리곱 연산이기 때문에 ‘Cat’ 값이 true 가 되더라도 ‘Dog’ 값을 평가해야 한다.
- 즉 두 번째 피연산자가 논리곱 연산자 표현식의 평가 결과를 결정한다. → 논리 연산의 결과를 결정하는 두 번째 피연산자, 즉 문자열 ‘Dog’를 반환한다.

<aside>
💡 논리 연산의 결과를 결정하는 피연산지를 타입 변환하지 않고 그대로 반환하는 것을 단축 평가라 한다. → 표현식을 평가하는 도중 평가 결과가 확정된 경우 나머지 평가 과정을 생략하는 것을 말한다.

</aside>

```jsx
let obj = null;

// let value = obj.value -> error
let value = obj && obj.value; // -> null : 에러 발생을 방지할 수 있다.

function getStringLength(str) {
	str = str || ''; // 매개변수의 값이 false로 평가되면 빈 문자열 값이 들어간다.
	return str.length;
}

getSTringLength(); // 0
getStringLength('string'); // 6
```

### 옵셔널 체이닝 연산자

> ES11 에서 도입된 연산자 ‘?.’ 는 좌항의 피연산자가 null 또는 undefined인 경우 undefined를 반환하고 그렇지 않으면 우항의 프로퍼티 참조를 이어간다.
> 

```jsx
let obj = null;

let value = obj?.val;
console.log(value); // undefined

obj = { val: 'i am value' };

value = obj?.val;
console.log(value); // "i am value"

// 옵셔널 체이닝 연산자는 좌항 피연산자가 Falsy 값으로 평가되더라도 null 혹은 undefined가 아니라면 우항의 프로퍼티 참조를 이어간다.
obj = ''; // Falsy 값으로 평가됨
value = obj?.length;
console.log(value); // 0

```

### null 병합 연산자

> ES11에서 도입된 null 병합 연산자 ‘??’ 는 좌항의 피연산자가 null 또는 undefined인 경우 우항의 피연산자를 반환하고, 그렇지 않으면 좌항의 피연산자를 반환한다.
> 

```jsx
let x = null ?? 'default value'; // default value
```