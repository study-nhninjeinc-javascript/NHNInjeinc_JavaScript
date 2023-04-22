# 제너레이터와 async/await

> ES6에서 도입된 제네레이터는 코드 블록의 실행을 일시 중지했다가 필요한 시점에 재개할 수 있는 특수한 함수다.
> 

### 제네레이터와 일반 함수의 차이

- 제네레이터 함수는 함수 호출자에게 함수 실행의 제어권을 양도할 수 있다.
    - 일반 함수 호출 시 제어권이 함수에게 넘어가고 함수 코드를 실행한다. → 함수 호출자는 함수를 호출한 이후 함수 실행을 제어할 수 없다.
    - 제네레이터는 함수는 함수 실행을 함수 호출자가 제어할 수 있다. → 함수 호출자가 함수 실행을 일시 중지시키거나 재개할 수 있다.
        - 함수의 제어권을 함수가 독점하는 것이 아니라 함수 호출자에게 양도할 수 있다는 것이다.
- 제네레이터 함수는 함수 호출자와 함수의 상태를 주고받을 수 있다.
    - 일반 함수를 호출하면 매개변수를 통해 함수 외부에서 값을 받아 함수 코드를 일괄 실행하여 결과값을 함수 외부로 반환한다.
        - 함수가 실행되고 있는 동안에는 함수 외부에서 함수 내부로 값을 전달하여 함수의 상태를 변경할 수 없다.
    - 제네레이터 함수는 함수 호출자와 양방향으로 함수의 상태를 주고받을 수 있다.
        - 제네레이터 함수는 함수 호출자에게 상태를 전달할 수 있고 호출자로부터 상태를 전달받을 수도 있다.
- 제네레이터 함수를 호출하면 제네레이터 객체를 반환한다.
    - 일반 함수를 호출하면 함수 코드를 일괄 실행하고 값을 반환한다.
    - 제네레이터 함수를 호출하면 함수 코드를 실행하는 것이 아니라 이터러블이면서 동시에 이터레이터인 제네레이터 객체를 반환한다.

## 제너레이터 함수의 정의

> 제네레이터 함수는 function* 키워드로 선언한다. 또한 하나 이상의 yield 표현식을 포함한다.
> 

```jsx
// 제너레이터 함수 선언문
function* genDecFunc() {
	yield 1;
}

// 제너레이터 함수 표현식
const genExpFunc = function* () {
	yield 1;
};

// 제너레이터 메서드
const obj = {
	* genObjMethod() {
		yield 1;
	}
};

// 애스터리스크(*)의 위치는 function 키워드와 함수 이름 사이라면 어디든 상관 없다고 한다.
function * genFunc_1() { yield 1; )
function *genFunc_2() { yield 2; }
function*genFunc_3() { yield 3; }
// 보통 일관성 유지를 위해 function 키워드 바로 뒤에 붙이는 것을 권장한다.
function* genFunc_4() { yield 4; }

// 제너레이터 함수는 화살표 함수로 정의할 수 없다.
const arrowGen = * () => {
	yield 1;
}; // error

// 제너레이터 함수는 new 연산자와 함께 생성자 함수로 호출할 수 없다.
const newGen = new genDecFunc(); // error 
```

## 제너레이터 객체

> 제너레이터 함수를 호출하면 일반 함수처럼 함수 코드 블록을 실행하는 것이 아니라 제너레이터 객체를 생성해 반환한다. → 이때 제너레이터 객체는 이터러블이면서 동시에 이터레이터다.
> 
- 이터러블 장에서 배운것처럼 이터러블은 Symbol.iterator 메서드를 상속받는 객체이고 이터레이터는 value와 done 프로퍼티를 갖는 이터레이터 리절트 객체를 반환하는 next 메서드를 소유하는 객체를 말한다.
    - 즉 제너레이터 함수가 반환하는 객체는 Symbol.iterator 메서드를 상속받으면서 이터레이터 리절트 객체를 반환하는 next 메서드를 소유하고 있는 객체이다.
    
    ```jsx
    // 제너레이터 함수
    function* genFunc() {
    	yield 1;
    	yield 2;
    	yield 3;
    }
    
    // 제너레이터 함수를 호출하면 제너레이터 객체를 반환한다.
    const generator = genFunc();
    
    console.log(Symbol.iterator in generator); // true -> 이터러블이면서 동시에 이터레이터인 제너레이터 객체
    
    console.log('next' in generator); // true
    ```
    
- 제너레이터 객체는 이터레이터에는 없는 return, throw 메서드를 갖는다.
    - return 메서드는 인수로 전달받은 값을 value 프로퍼티 값으로, true를 done 프로퍼티 값으로 갖는 이터레이터 리절트 객체를 반환한다.
    - throw 메서드를 호출하면 인수로 전달받은 에러르 발생시키고 undefined를 value 프로퍼티 값으로, true를 done 프로퍼티 값으로 갖는 이터레이터 리절트 객체를 반환한다.
    
    ```jsx
    function* genFunc() {
    	try {
    		yield 1;
    		yield 2;
    		yield 3;
    	} catch(e) {
    		console.error(e);
    	}
    }
    
    const generator = genFunc();
    
    console.log(generator.next()); // { value: 1, done: false }
    console.log(generator.return('DONE!')); // { value: 'DONE!', done: true }
    console.log(generator.throw('Error!')); // Uncaught Error! { value: undefined, done: true }
    ```
    

## 제너레이터의 일시 중지와 재개

> yield 키워드는 제너레이터 함수의 실행을 일시 중지시키거나 yield 키워드 뒤에 오는 표현식의 결과를 제너레이터 함수 호출자에게 반환한다.
> 

```jsx
function* genFunc() {
	yield 1;
	yield 2;
	yield 3;
}

// 제너레이터 객체를 반환한다.
const generator = getFunc();

// 처음 next 메서드를 호출하면 첫 번째 yield 표현식까지 실행되고 일시 중지된다.
console.log(generator.next()); // { value: 1, done: false } -> next 메서드는 이터레이터 리절트 객체를 반환한다.
// 여기서 value 프로퍼티의 값은 첫 번째 yield 표현식에서 yield 된 값이 할당된다.

// 두 번째 next 메서드를 호출하면 두 번째 yield 표현식까지 실행되고 일시 중지된다.
console.log(generator.next()); // { value: 2, done: false } 

console.log(generator.next()); // { value: 3, done: false }

console.log(generator.next()); // { value: undefined, done: true}
```

```jsx
// 제너레이터 객체의 next 메서드에는 인수를 전달 할 수 있다. 이때 전달한 인수는 제너레이터 함수의 yield 표현식을 할당받는 변수에 할당된다.
// yield 표현식의 평가 결과가 할당되지 않는다! -> 헷갈릴 수 있으니 주의하자
function* genFunc() {
	const x = yield 1; // 첫 next 메서드가 실행됬을때 yield 표현식이 x에 할당되는 것이 아니다!

	const y = yield x + 10;

	// 이때 반환하는 x + y 값은 세번째 next 메서드가 호출되었을 때 이터레이터 리절트 객체의 value 프로퍼티에 할당된다.
	// 즉 제너레이터에서는 값을 반환할 필요가 없고 return 은 종료의 의미로만 사용해야 한다.
	yield x + y;
}

const generator = genFunc();

let res = generator.next(); // 처음 호출하는 next 메서드에 인수를 전달하면 무시된다.
console.log(res); // {value: 1, done: false}

res = generator.next(10); // 이때 next 메서드에 전달한 10이 제너레이터 객체의 x 값에 할당된다. 값이 할당된 x 를 사용해서 yield 표현식이 평가된다.
console.log(res); // {value: 20, done: false}

res = generator.next(20); // 이때 next 메서드에 전달한 20이 제너레이터 객체의 y 값에 할당되며 이때는 함수 끝까지 실행된다.
console.log(res); // {value: 30, done: true}
```

## 제너레이터의 활용

### 이터러블의 구현

> 무한 이터러블을 생성하는 제너레이터 함수를 구현해보자
> 

```jsx
const infiniteFibonacci = (function* () {
	let [pre, cur] = [0, 1];

	while(true) {
		[pre, cur] = [cur, pre + cur];
		yield cur;
	}
}());
// 제너레이터 객체를 즉시실행함수로 감싸 실행한다. -> 제너레이터 객체를 생성한다.
// 제너레이터 객체는 호출 시 제너레이터 객체를 반환한다. -> 내부 코드가 실행되지 않는다!

// for of 문을 통해 제너레이터 객체의 next 메서드를 반복해서 호출
for (const num of infiniteFibonacci) {
	// for of 문에서 변수 num 은 next 메서드 호출 시 반환하는 이터레이터 리절트 객체의 value 값을 가져온다.
	// 제너레이터 함수에서 next 호출 시 반환하는 이터레이터 리절트 객체의 value 값은 yield 표현식의 평가 결과이다.
	// next 메서드를 호출할 때마다 while 문이 반복되어 yield 표현식이 평가되고 결과를 가져온다.
	if (num > 10000) break;
	console.log(num);
}
```

### 비동기 처리

> next 메서드와 yield 표현식을 통해 비동기 처리를 동기 처리처럼 구현할 수 있다.
> 

```jsx
const async = generatorFunc => {
	// 2. async 함수가 제너레이터 함수를 인수로 받으면서 호출되면 코드가 실행되며 제너레이터 객체를 생성한다.
	const generator = generatorFunc();

    // 3. 정의한 onResolved 메서드를 반환한다.
	const onResolved = arg => {
		// 6. async 함수가 인수로 전달받은 제너레이터 객체의 next 메서드를 호출하고 이를 결과로 받는다.
		// 이때 첫 번째 next 호출이 아니라면 arg 인수가 존재할 것이고 이를 next 메서드 호출 시 인수로 전달한다.
		const result = generator.next(arg);
		
		// next 메서드가 반환한 객체의 done 프로퍼티가 true 인 경우 result.value를 반환
		// next 메서드가 반환한 객체의 done 프로퍼티가 false 경우 결과값을 인수로 자기 자신(onResolved)을 재귀 호출
		return result.done ? result.value : result.value.then(res => onResolved(res));
	};

	// 4. 정의한 onResolved 메서드를 반환한다.
	return onResolved;
};

// 1. async 함수에 제너레이터 함수를 인수로 전달하면서 호출한다.
// 5. async 함수가 반환하는 onResolved 메서드를 실행한다.
async(function* fetchTodo() {
	const url = 'https://jsonplaceholder.typicode.com/todos/1';

    // 첫 번째 next 호출 시 fetch 문까지 실행한 뒤 해당 결과를 반환한다.
	const response = yield fetch(url);

    // 두 번째 next 호출 시 response를 역직렬화 한 값을 반환한다.
    // 이때 response 는 두 번째 next 호출 시 전달받은 인수가 되는데 이는 fetch 의 결과이다.
    // fetch의 결과는 Response 객체를 래핑한 Promise 객체이다.
	const todo = yield response.json();

    // 세 번째 next 호출 시 todo 변수에 next 메서드의 인수가 할당된다. -> 역직렬화한 fetch의 결과
    // 마지막 호출(더이상 yield가 없다.)이므로 next 메서드는 value: undefined, done: false 가 반환된다.
    // 남은 코드도 모두 실행된다.
	console.log(todo);
	// {userId: 1, id: 1, title: 'delectus aut autem', complete: false}
})();

// async 함수가 반환하는 함수를 바로 호출하도록 하지 않고 따로 뺄 수도 있다.
const excute = async(function* fetchTodo() {
	const url = 'https://jsonplaceholder.typicode.com/todos/1';

	const response = yield fetch(url);
	const todo = yield response.json();
	console.log(todo);
	// {userId: 1, id: 1, title: 'delectus aut autem', complete: false}
});

excute();
// 위 처럼 실행해도 동일하게 동작한다는 의미이다.
```

<aside>
💡 제너레이터 실행기가 필요하다면 직접 구현하지 말고 co 라이브러리를 활용해보자.

</aside>

## async/await

> 제너레이터를 사용해 비동기 처리를 동기 처리처럼 동작하도록 구현했으나 코드가 길어지고 가독성도 좋다고 보기는 힘들다.
> 

<aside>
💡 ES8에서는 제너레이터보다 간단하고 가독성 좋게 비동기 처리를 동기처럼 동작하도록 구현할 수 있는 async/await가 도입되었다.

</aside>

- async/await 는 프로미스를 기반으로 동작한다.

```jsx
async function fetchTodo() {
	const url = 'https://jsonplaceholder.typicode.com/todos/1';

	const response = await fetch(url);
	const todo = await response.json();
	console.log(todo);
}

fetchTodo();
```

<aside>
💡 async/await은 프로미스를 기반으로 동작한다. 프로미스를 정확하게 이해하는 것이 가장 중요할 듯.

</aside>

### async 함수

> async 함수는 async 키워드를 사용해 정의하며 언제나 프로미스를 반환한다. (명시적으로 프로미스를 반환하지 않더라도 암묵적으로 반환값을 resolve하는 프로미스를 반환한다.
> 

```jsx
// async 함수 선언
async function asyncFunc(n) { return n; }
asyncFunc(1).then(v => console.log(v)); // 1

// async 함수 표현식
const asyncExpFunc = async function(n) { return n; };
asyncExpFunc(2).then(v => console.log(v)); // 2

// async 화살표 함수
const asyncArrowFunc = async n => n;
asyncArrowFunc(3).then(v => console.log(v)); // 3

// async 메서드
const obj = {
	async asyncMethod(n) { return n; }
};
obj.asyncMethod(4).then(v => console.log(v)); // 4

// async 클래스 메서드
class MyClass {
	async asyncClassMethod(n) { return n }
}
const myClass = new MyClass();
myClass.asyncClassMethod(5).then(v => console.log(v)); // 5
```

### await 키워드

> await 키워드는 반드시 async 함수 내부에서 사용해야 한다. await 키워드는 프로미스가 settled 상태가 될 때까지 대기하다가 settled 상태가 되면 프로미스가 resolve한 처리 결과를 반환한다.
> 

```jsx
const getGithubUserName = async id => { // async 함수 정의
	const res = await fetch(`https://api.github.com/users/${id}`); // await 키워드 사용
	const { name } = await res.json(); // res 객체에서 name 값만 가져온다.
	console.log(name); // Lee Ha Neul
};

getGithubUserName('msrim2007');
```

- fetch 함수가 수행한 HTTP 요청에 대한 서버의 응답이 도착해 fetch 함수가 반환한 프로미스가 settled 상태가 될때까지 대기
    - 해당 프로미스가 settled 상태가 되면 프로미스가 resolve한 처리 결과가 res 변수에 담긴다

### 에러 처리

> async/await 에서 에러 처리는 try..catch 문을 사용할 수 있다.
> 

```jsx
const foo = async () => {
	try {
		const wrongUrl = 'https://wrong.url';

		const response = await fetch(wrongUrl);
		const data = await response.json();
		console.log(data);
	} catch (err) {
		console.error(err);
	}
};

foo();
```

- async 함수 내에서 catch 문을 사용해서 에러 처리를 하지 않으면 async 함수는 발생한 에러를 reject 하는 프로미스를 반환한다.
    - 프로미스의 후속처리 메서드를 사용해 에러를 캐치할 수도 있다.

```jsx
foo().then(console.log).catch(console.error);
```