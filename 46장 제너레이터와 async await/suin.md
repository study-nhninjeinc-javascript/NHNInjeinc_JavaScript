## 46.1 제너레이터란?

- 코드 블록의 실행을 일시중지 했다가 필요한 시점에 재개할 수 있는 특수한 함수다.
- 제너레이터와 일반함수의 차이
    1. 제너레이터 함수는 함수 호출자에게 함수 실행의 제어권을 양도 할 수 있다.
        - 일반 함수를 호출하면 제어권이 함수에게 넘어가고 함수코드를 일괄 실행한다.
        → 즉, 함수 호출자는 함수를 호출한 이후 함수실행을 제어할수 없다.
        - 제너레이터 함수는 함수 실행을 함수 호출자가 제어할 수 있다.
        - 다시말해 함수호출자가 함수 실행을 일시중지시키거나 재개시킬 수 있다.
        - 이는 **함수 제어권을 함수가 독점하는 것이 아니라 함수 호출자에게 양도 할수 있다**는 것을 의미한다.
    2. 제너레이터 함수는 함수 호출자와 함수의 상태를 주고 받을 수 있다.
        - 일반함수를 호출하면 매개변수를 통해 함수 외부에서 값을 주입받고 함수 코드를 일괄 실행하여 결과값을 함수 외부로 반환한다.
        → 즉, 함수가 실행되고 있는 동안에는 함수 외부에서 함수 내부로 값을 전달하여 함수의 상태를 변경할수 없다.
        - 제너레이터 함수는 함수 호출자와 양방향 함수의 상태를 주고 받을수 잇다.
        - 다시말해 **제너레이터 함수는 함수 호출자에게 상태를 전달할 수 있고 함수 호출자로부터 상태를 전달받을 수도 있다.**
    3. 제너레이터 함수를 호출하면 제너레이터 객체를 반환한다.
        - 일반 함수를 호출하면 함수코드를 일괄 실행하고 값을 반환한다.
        - **제너레이터 함수를 호출하면 함수코드를 실행하는 것이아니라 이터러블이면서 동시에 이터레이터인 제너레이터 객체를 반환한다.**

이터러블 : 반복 가능한(iterable, 이터러블) 객체는 배열을 일반화한 객체

이터레이터 : 객체를 next 메서드로 순환 할 수 있는 객체

```jsx
var iterator = '12'[Symbol.iterator]();
iterator.next(); // {value: "1", done: false}
iterator.next(); // {value: "2", done: false}
iterator.next(); // {value: undefined, done: true}
```

## 46.2 제너레이터 함수의 정의

- 제너레이터 함수는 function* 키워드로 선언한다. 그러나 하나이상의 yield 표현식을 포함한다.
- 이것을 제외하면 일반함수를 정의하는 방법과 같다

```jsx
//제너레이터 함수 선언문
function* genDecFunc(){
	yield 1;
}

//제너레이터 함수 표현식
const gneExpFunc = function*(){
	yield 1;
};

//제너레이터 메서드
const obj = {
	* gneExpMethod(){
		yield 1;
	}
};

//제너레이터 클래스 메서드
class MyClass{
	* gneClsMethod(){
		yield 1;
	}
}
```

- 에스터리스크(*)의 위치는 function 키워드와 함수이름 사이라면 어디든 상관없다.
- 일관성을 유지하기 위해 function 키워드 바로 뒤에 붙이는 것을 권장한다.
- 제너레이터 함수는 화살표함수로 정의할수 없다.
- 제너레이터 함수는 new 연산자와 함께 생성자 함수로 호출할 수 없다.

```jsx
const genArrowFunc = * () => {
	yield 1;
}
//caught SyntaxError: Unexpected token '*'

functon* genFunc(){
	yield 1;
}
new genFunc();
//Uncaught SyntaxError: Unexpected token '{'
```

## 46.3 제너레이터 객체

- **제너레이터 함수를 호출하면 일반함수처럼 함수 코드 블록을 실행하는 것이 아니라 제너레이터 객체를 생성해 반환한다.**
- **제너레이터 함수가 반환한 제너레이터 객체는 이터러블 이면서 이터레이터다.**
- 다시 말해, 제너레이터 객체는 Symbol.iterator 메서드를 상속받는 이터러블이면서 value, done 프로퍼티를 갖는 이터레이터 이절트 객체를 반환하는 next메서드를 소유하는이터레이터다.
- 제너레이터 객체는 next메서드를 가지는 이터레이터이므로 Symbol.iterator 메서드를 호출해서 별도로 이터레이터를 생성할 필요가 없다.

```jsx
//제너레이터 함수
function* genFunc(){
	yield 1;
	yield 2;
	yield 3;
}

//제너레이터 함수를 호출하면 제너레이터 객체를 반환한다.
const generator = genFunc();

//제너레이터 객체는 이터러블이면서 동시에 이터레이터다
//이터러블은 Symbol.iterator 메서드를 직접 구현하거나 프로토타입 체인을 통해 상속받은 객체다.
console.log(Symbol.iterator in generator);//true
//이터레이터는 next메서드를 갖는다
console.log('next' in generator);//true

```

- 제너레이터 객체는 next 메서드를 갖는 이터레이터지만, 이터레이터에 없는 return throw 메서드를 갖는다.
- 제너레이터 객체의 세개의 메서드를 호출하면 다음과 같이 동작한다.
    1. next 메서드를 호출하면 제너레이터 함수의 yield 표현식까지 코드블록을 실행하고 yield된 값을 value 프로퍼티 값으로 false를 done 프로퍼티 값으로 이터레이터 리절트 객체를 반환한다.
    2. return 메서드를 호출하면 인수로 전달받은 값을 value프로퍼티 값으로, true를 done 프로퍼티 값으로 갖는 이터레이터 리절트 객체를 반환한다.
    3. throw 매서드를 호출하면 인수로 전달받은 에러를 발행시키고 undefined를 value프로퍼티 값으로, true를 done프로퍼티 값으로 갖는 이터레이터 리절트 객체를 반환한다.
        
        ```jsx
        //제너레이터 함수
        function* genFunc(){
        	try{
        		yield 1;
        		yield 2;
        		yield 3;
        	}catch(e){
        		console.error(e);
        	}
        }
        
        //제너레이터 함수를 호출하면 제너레이터 객체를 반환한다.
        const generator = genFunc();
        
        //next 메서드를 호출
        console.log(generator.next());//{value: 1, done: false}
        //return 메서드를 호출
        //console.log(generator.return('End!'));//{value: 'End!', done: true}
        //throw 매서드를 호출
        console.log(generator.throw('Error!'));//Error!
        //{value: undefined, done: true}
        
        //return 메서드를 호출하고 throw메서드를 호출하면 그냥 caught Error!만 나온다
        //왜일까?
        //next를 통해서 done : true 값 이후에 throw메서드를 호출하면 caught Error!만 나온다
        //throw 매서드를 호출하면 done프로퍼티의 값을 true로 하여 할 필요가 없어서인가???
        ```
        

## 46.4 제너레이터의 일시중지와 재개

- 일반함수는 호출 이후 제어권을 함수가 독점, 제너레이터는 함수 호출자에게 yield 키워드와 next메서드를 통해  제어권을 양도(yield)하여 필요한 시점에서 함수 실행을 재개할수 있다.
- 제너레이터 함수를 호출하면 제너레이터 함수의 코드블록이 실행되는 것이 아니라 제너레이터 객체를 반환한다.
- 이터러블이면서 동시에 이터레이터인 제너레이터 객체는 next 메서드를 갖고, next 메서드를 호출하면 제너레이터 함수의 코드블록을 실행한다.
- 단, 일반함수처럼 한번에 코드 블록의 모든코드를 일괄 실행하는 것이 아니라 yield 표현식까지만 실행한다.
- **yield 키워드는 제너레이터 함수의 실행을 일시중지시키거나 yield 키워드 뒤에 오는 표현식의 평가 결과를 제너레이터 함수 호출자에게 반환한다.**

```jsx
//제너레이터 함수
function* genFunc(){
	yield 1;
	yield 2;
	yield 3;
}

//제너레이터 함수를 호출하면 제너레이터 객체를 반환한다.
//이터러블이면서 동시에 이터레이터인 제너레이터 객체는 next메서드를 갖는다
const generator = genFunc();

//처음 next메서드를 호출하면 첫 번째 yield 표현식까지 실행되고 일시중지된다.
//next메서드는이터레이터 리절트 객체({value,done})를 반환한다.
//value프로퍼티에는 첫번째 yield 표현식에서 yield된 값 1이 할당된다.
//done 프로퍼티에는 제너레이터 함수가 끝까지 실행되었는지를 나타내는 false가 할당된다.
console.log(generator.next());

//다시 next메서드를 호출하면 두번째 yield 표현식까지 실행되고 일시중지된다.
//next메서드는 이터레이터 리절트 객체 ({value,done})를 반환한다.
//value프로퍼티에는 두번째 yield 표현식에서 yield된 값 2이 할당된다.
//done 프로퍼티에는 제너레이터 함수가 끝까지 실행되었는지를 나타내는 false가 할당된다.
console.log(generator.next());

//다시 next메서드를 호출하면 세번째 yield 표현식까지 실행되고 일시중지된다.
//next메서드는 이터레이터 리절트 객체 ({value,done})를 반환한다.
//value프로퍼티에는 세번째 yield 표현식에서 yield된 값 3이 할당된다.
//done 프로퍼티에는 제너레이터 함수가 끝까지 실행되었는지를 나타내는 false가 할당된다.
console.log(generator.next());

//다시 next메서드를 호출하면 남은 yield 표현식이 없으므로 제너레이터 함수의 마지막까지 실행한다.
//next메서드는 이터레이터 리절트 객체 ({value,done})를 반환한다.
//value프로퍼티에는 제너레이터 함수의 반환값 undefined가 할당된다.
//done 프로퍼티에는 제너레이터 함수가 끝까지 실행되었는지를 나타내는 true가 할당된다.
console.log(generator.next());
```

- **제너레이터 객체의 next 메서드를 호출 → yield 표현식까지 실행 → 일시 중지된다.
이때 함수의 제어권이 호출자로 양도 된다.**
- 이후 필요한 시점에 호출자가 또다시 next 메서드를 호출 위가 반복
- 이때 제너레이터 객체의 next메서드는 value, done 프로퍼티를 갖는 이터레이터 리절트 객체를 반환한다.
value 프로퍼티에는 yield표현식에서 yield된 값 할당
done 프로퍼티에는 제너레이터 함수가 끝까지 실행되었는지 나타내는 불리언값 할당
- 끝까지 실행되면 value 프로퍼티에는 제너레이터 함수의 반환 값이 할당, done 프로퍼티에는 true 할당된다.
- 이터레이터의 next메서드와 달리 제너레이터 객체의 next 메서드에는 인수를 전달할수 있다
- **제너레이터 객체의 next 메서드에 전달한 인수는 제너레이터 함수의 yield 표현식을 할당 받는 변수에 할당된다.**
- yield 표현식을 할당받는 변수에 yield 표현식의 평가결과가 할당되지 않는것에 주의해야한다.

```jsx
function* genFunc() {
	// 처음 next 메서드를 호출하면 첫 번째 yield 표현식까지 실행되고 일시 중지된다.
	// 이때 yield된 값 1은 next 메서드가 반환한 이터레이터 리절트 객체의 value 프로퍼티에 할당된다.
	// x 변수에는 아직 아무것도 할당되지 않았다. x 변수의 값은 next 메서드가 두 번째 호출될 때 결정된다.
	const x = yield 1;
	
	// 두 번째 next 메서드를 호출할 때 전달한 인수 10은 첫 번째 yield 표현식을 할당받는 x 변수에 할당된다.
	// 즉, const x = yield 1;은 두 번째 next 메서드를 호출했을 때 완료된다.
	// 두 번째 next 메서드를 호출하면 두 번째 yield 표현식까지 실행되고 일시 중지된다.
	// 이때 yield된 값 x + 10은 next 메서드가 반환한 이터레이터 리절트 객체의 value 프로퍼티에 할당된다.
	const y = yield (x + 10);
	
	// 세 번째 next 메서드를 호출할 때 전달한 인수 20은 두 번째 yield 표현식을 할당받는 y 변수에 할당된다.
	// 즉, const y = yield (x + 10);는 세 번째 next 메서드를 호출했을 때 완료된다.
	// 세 번째 next 메서드를 호출하면 함수 끝까지 실행된다.
	// 이때 제너레이터 함수의 반환값 x + y는 next 메서드가 반환한 이터레이터 리절트 객체의 value 프로퍼티에 할당된다.
	// 일반적으로 제너레이터의 반환값은 의미가 없다.
	// 따라서 제너레이터에서는 값을 반환할 필요가 없고 return은 종료의 의미로만 사용해야 한다.
	return x + y;
}

// 제너레이터 함수를 호출하면 제너레이터 객체를 반환한다.
// 이터러블이며 동시에 이터레이터인 제너레이터 객체는 next 메서드를 갖는다.
const generator = genFunc(0);

// 처음 호출하는 next 메서드에는 인수를 전달하지 않는다.
// 만약 처음 호출하는 next 메서드에 인수를 전달하면 무시된다.
// next 메서드가 반환한 이터레이터 리절트 객체의 value 프로퍼티에는 첫 번째 yield된 값 1이 할당된다.
let res = generator.next();
console.log(res); // {value: 1, done: false}

// next 메서드에 인수로 전달한 10은 genFunc 함수의 x 변수에 할당된다.
// next 메서드가 반환한 이터레이터 리절트 객체의 value 프로퍼티에는 두 번째 yield된 값 20이 할당된다.
res = generator.next(10);
console.log(res); // {value: 20, done: false}

// next 메서드에 인수로 전달한 20은 genFunc 함수의 y 변수에 할당된다.
// next 메서드가 반환한 이터레이터 리절트 객체의 value 프로퍼티에는 제너레이터 함수의 반환값 30이 할당된다.
res = generator.next(20);
console.log(res); // {value: 30, done: true}

//이해하기로는 첫번째에는 yield 표현식까지 실행되고 일시 중지
//다음부터는 **yield 표현식을 할당 받는 변수**에 **next 메서드**에 전달한 인수가 들어간다.
```

- 제너레이터 함수는 next 메서드와 yield 표현식을 통해 함수 호출자와 함수의 상태를 주고 받을수 있다.
- 함수 호출자는 next 메서드를 통해 yield 표현식까지 함수를 실행시켜
제너레이터 객체가 관리하는 상태를 꺼내올수 있고,
next 메서드에 인수를 전달해서 제너래이터 객체에 상태를 밀어넣을수 있다.

## 46.5 제너레이터의 활용

### 이터러블 구현

- 제너레이터 함수를 사용하면 이터레이션 프로토콜을 준수해 이터러블을 생성하는 방식보다 간단히 이터러블을 구현할 수 있다.

```jsx
//무한 이터러블을 생성하는 제너레이터 함수
const infiniteFibonacci = (function* () {
  let [pre, cur] = [0, 1];

  while (true) {
    [pre, cur] = [cur, pre + cur];
    yield cur;
  }
}());

//infiniteFibonacci는 무한 이터러블이다.
for (const num of infiniteFibonacci) {
  if (num > 10000) break;
  console.log(num); // 1 2 3 5 8...2584 4181 6765
}
```

### 비동기 처리

- 제너레이터 함수는 next메서드와 yield  표현식을 통해 함수 상태를 주고 받을 수 있다.
- 이러한 특성을 활용하면 프로미스를 사용한 비동기 처리를 동기 처리처럼 구현할수 있다.

```jsx
// node-fetch는 node.js 환경에서 window.fetch 함수를 사용하기 위한 패키지다.
// 브라우저 환경에서 이 예제를 실행한다면 아래 코드는 필요 없다.
//node.js 환경이 아니라서 테스트는 안돌려봤다..
const fetch = require('node-fetch');

// 제너레이터 실행기
const async = generatorFunc => {
	  const generator = generatorFunc(); // 2

	  const onResolved = arg => {
	    const result = generator.next(arg); // 5

	    return result.done
	      ? result.value // 9
	      : result.value.then(res => onResolved(res)); // 7
	  };

	  return onResolved; // 3
};

(async(function* fetchTodo() { // 1
	  const url = 'https://jsonplaceholder.typicode.com/todos/1';

	  const response = yield fetch(url); // 6
	  const todo = yield response.json(); // 8
	  console.log(todo);
	  // {userId: 1, id: 1, title: 'delectus aut autem', completed: false}
})()); // 4
```

1. async함수가 호출(1)되면 인수로 전달받은 제너레이터 함수 fetchToDo를 호출하여 제너레이터 객체(2)를 생성 onResolved 함수를 반환한다.
onResolved 함수는 상위스코프의 generator변수를 기억하는 클로저다
async 함수가 반환한 onResolved  함수를 즉시 호출(4)하여 (2)에서 생성한 제너레이터 객체의 next 메서드를 호출(5)한다
2. next 메서드를 호출(5)되면 제너레이터 함수 fetchToDo의 첫번째 yield문(6)까지 실행된다.
이때 next 메서드가 반환한 이터레이터 리절트 객체의 done프로퍼티 값이 false, 즉 아직 제너레이터 함수가 끝까지 실행되지 않았다면 이터레이터 리절트 객체의 value 프로퍼티 값, 즉 첫번째 yield된 fetch 함수가 반환한 프로미스가 resolve한 Response 객체를 onResolved  함수에 인수로 전달하면서 재귀호출(7)한다.
3. onResolved  함수에 인수로 전달된 Response 객체를 next메서드에 인수로 전달하면서 메서드를 두번째로 호출한다.
이때 next메서드에 인수로 전달한 Response 객체는 제너레이터 함수 fetchToDo의 response 변수(6)에 할당되고 제너레이터 함수 fetchToDo의 두번째 yield문(8)까지 실행한다.
4. next메서드가 반환한 이터레이터 리절트 객체의 done 프로퍼티 값이 false, 즉 아직 제너레이터 함수 fetchToDo가 끝까지 실행되지 않았다면 이터레이터 리절트 객체의 value 프로퍼티 값, 즉 두번째 yield된 response.json 메서드가 반환한 프로미스가 resolve한 todo객체를 onResolved 함수에 인수로 전달하면서 재귀호출(7)한다.
5. onResolved 함수에 인수로 전달된 todo객체를 next 메서드에 인수로 전달하면서 next 메서드를 세번째 호출(5)한다.
이때 next 메서드에 인수로 전달한 doto 객체는 제너레이터 함수 fetchToDo의 todo변수(8)에 할당되고 제너레이터 함수에 fetchToDo가 끝까지 실행된다.
6. next 메서드가 반환한 이터레이터 리절트 객체의 done 프로퍼티 값이 true, 즉 제너레이터 함수가 fetchTodo가 끝까지 실행되었다면 이터레이터 리절트 객체의 value 프로퍼티 값, 즉 제너레이터 함수 fetchTodo 반환값인 undefined를 그대로 반환(9)하고 처리를 종료한다.
- 예제의 제너레이터 함수를 실행하는 제너레이터 싱행기인 async함수는 이해를 돕기위해 간략화한것 이므로 완전하지 않다.
- 혹시 async함수와 같은 제너레이터 실행기가 필요하다면 직접 구현하는 것 보다 co라이브러리를 사용하기를 바란다.

## 46.1 async/await

- ES8에는 제너레이터 보다 가독성 좋게 비동기 처리를 동기 처리처럼 동작하도록 구현할수 있는 async/await가 도입되었다.
- async/await는 프로미스를 기반으로 동작한다.
async/await를 사용하면 프로미스스의 then/catch/finally 후속 처리 메서드에 콜백 함수를 전달해서 비동기 처리 결과를 후속처리할 필요없이 마치 동기처럼 프로미스를 사용할수있다.

```jsx
const fetch = require('node-fetch');

async function fetchTodo(){
	const url = 'https://jsonplaceholder.typicode.com/todos/1';
	
	const response = await fetch(url);
	const todo = await response.json();
	
	console.log(todo);
}

fetchTodo();
```

### async 함수

- await 키워드는 반드시 async 함수 내부에서 사용해야 한다.
- async 함수는 async키워드를 사용해 정의하며 언제나 프로미스를 반환한다.
- async함수가 명시적으로 프로미스를 반환하지 않더라도 async함수는 암묵적으로 반환값을 resolve하는 프로미스를 반환한다.

```jsx
// async 함수 선언문
async function foo(n){return n;}
foo(1).then(v => console.log(v)); //1

// async 함수 표현식
const bar = async function (n){return n;};
bar(2).then(v => console.log(v)); //2

// async 화살표 함수
const baz = async n => n;
baz(3).then(v => console.log(v)); //3

// async 메서드
const obj = {
  async foo(n){return n;}
};
obj.foo(4).then(v => console.log(v)); //4

// async 클래스 메서드
class MyClass {
	async bar(n){return n;}
}
const myClass = new MyClass();
myClass.bar(5).then(v => console.log(v)); //5
```

- 클래스의 constructor 메서드는 async메서드가 될수 없다 클래스의 constructor메서드는 인스턴스를 반환해야하지만 async함수는 언제나 프로미스를 반환해야한다.

```jsx
class MyClass {
	async constructor (){}
}
//caught SyntaxError: Class constructor may not be an async method
```

### await 키워드

- **await 키워드는 프로미스가 settled 상태(비동기 처리가 수행된 상태)가 될 때 까지 대기하다가 setteld 상태가 되면 프로미스가 resolve 한 처리 결과를 반환한다.**
- await 키워드는 반드시 프로미스 앖에서 사용해야한다.

```jsx
const fetch = require('node-fetch');

async fetchGithubUserName = async id => {
	const res = await fetch('https:/api.github.com/users/${id}')//1
	const {name} = await res.json();
	console.log(name);
}

fetchGithubUserName('IT-SSU');
```

- 1의 fetch 함수가 수행한 요청에 대한 서버의 응답이 도착해서 fetch 함수가 반환한 프로미스가 settled 상태가 될때까지 1은 대기 이후 프로미스다 settled 상태가 되면 프로미스가 resolve한 처리결과를 res 변수에 할당된다.

```jsx
async function foo() {
	const a = await new Promise(resolve => setTimeout(() => resolve(1), 3000));
	const b = await new Promise(resolve => setTimeout(() => resolve(2), 2000));
	const c = await new Promise(resolve => setTimeout(() => resolve(3), 1000));
	
	console.log([a,b,c]); // [1, 2, 3]
}

foo(); // 약 6초 소요된다.
```

- 모든 프로미스에 await 키워드를 사용하는것은 주의해야한다.
- foo 함수가 수행하는 3개의 비동기 처리는 서로 연관이 없어 개별적으로 수행되는 비동기 처리이므로 앞선 비동기 처리가 완료될때까지 대기해서 순차적으로 처리할 필요가 없다.

```jsx
async function foo() {
	const res = await Promise.all([
		new Promise(resolve => setTimeout(() => resolve(1), 3000)),
		new Promise(resolve => setTimeout(() => resolve(2), 2000)),
		new Promise(resolve => setTimeout(() => resolve(3), 1000))
	]);
	
	console.log(res); // [1, 2, 3]
}

foo(); // 약 6초 소요된다.
```

### 에러처리

- 비동기 처리를 위한 콜백 패턴의 단점중 심각한것은 에러처리가 곤란하다는것이다.
- 에러는 호출자 방향으로 전파된다. 즉, 콜 스택의 가장 아래방향(실행중인 실행컨택스트가 푸시되기 직전에 푸시된 실행컨텍스트 방향)으로 전파된다.
- 비동기 함수의 콜백 함수를 호출한 것은 비동기 함수가 아니기 때문에 try...catch 문으로 에러를 캐치할 수 없다.
- 하지만, async/await에서는 try catch 문으로 에러를 캐치할 수 있다. -> 프로미스를 반환하는 비동기 함수는 명시적으로 호출할 수 있기 때문에 호출자가 명확하다.
- async 함수 내에서 cath문을 사용해서 에러 처리를 하지 않으면 async 함수는 발생한 에러를 reject하는 프로미스를 반환한다.

```jsx
const fetch = require('node-fetch');

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