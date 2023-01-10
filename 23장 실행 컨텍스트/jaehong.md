## 실행 컨텍스트

```
var name = "test";  (1) 변수 선언 (6) 변수 대입
function wow(word) { (2) 변수 선언 (3) 변수 대입
    console.log(word + ' ' + name); (11)
}

function say() {  (4) 변수 선언 (5) 변수 대입
    var name = "test2"; (8)
    console.log(name);  (9)
    wow("hello");  (10)
}
say();  (7)
```

코드를 실행하는 순간 모든 것을 포함하는 전역 컨텍스트가 생긴다. 페이지가 종료될 때까지 유지된다.
함수를 호출할 때마다 함수 컨텍스트가 하나씩 생긴다
컨텍스트의 원칙 네가지
먼저 전역 컨텍스트 하나 생성 후, 함수 호출 시마다 컨텍스트가 생긴다
컨텍스트 생성시 컨텍스트 안에 변수객체(arguments, variable), scope chain, this 가 생성된다
컨텍스트 생성 후 함수가 실행되는데 사용되는 변수들은 변수 객체 안에서 값을 찾고, 없다면 스코프 체인을 따라 올라가며 찾는다.
함수 실행이 마무리되면 해당 컨텍스트는 사라진다.(클로저 제외)
페이지가 종료되면 전역 컨텍스트가 사라진다

전역 컨텍스트가 생성된 후 두번쨰 원칙에 따라 변수 객체, scope chain, this가 들어온다. 전역 컨텍스트에는 arguments(함수의 인자를 뜻함) 없고, variable은 해당 스코프의 변수들이다. name, wow, say가 있다.

scope chain(자신과 상위 스코프들의 변수객체)은 자기 자신인 전역 변수객체이다.
this는 따로 설정되어 있지 않으면 window이다.
this를 바꾸는 방법은 new를 호출하거나 함수에 다른 this 값을 bind해서 바꿀 수 있다.

객체 형식
순서

```
'전역 컨텍스트': {
    변수객체:{
        arguments: null,
        variable: ['name', 'wow', 'say'],
    },
    scopeChain: ['전역 변수객체'],
    this: window,
}
```

wow랑 say는 호이스팅 때문에 선언과 동시에 대입이 된다.
그후 variable의 name에 test가 대입이된다.

```
variable: [{name: 'test'}, {wow: Function}, {say: Function}]
```

그후 7번에서 say()를 하는 순간 새로운 say 함수 컨텍스트가 생기게 된다.

```
'say 컨텍스트':{
    변수객체:{
        arguments: null,
        variable: ['name'] //초기화 후 name : test2 가됨
    },
    scopeChain: ['say 변수객체', '전역 변수 객체'],
    this: window
}
```

say를 호출한후 위에서부터 차례대로 8 - 10을 실행한다.
variable의 name에 test2를 대입해준다
name 변수는 say 컨텍스트 안에서 찾으면 된다
variable에 있는 test2가 콘솔에 찍히게 된다
그 다음에 있는 wow("hello")가 있는데,
say 컨텍스트 안에서 wow 변수를 찾을 수 없기 때문에, scope chain을 따라 올라가
상위 변수객체에서 찾게된다.
그러므로 전역 변수 객체에서 찾게 되며, 전역 변수 객체의 variable에 wow라는 함수가 있으므로, 이것을 호출한다.

10번에서 wow함수가 호출되었으니 wow 컨텍스트가 생기게 된다.

```
'wow 컨텍스트':{
    변수객체:{
        arguments:[{word: 'hello'}],
        variable: null
    },
    scopeChain: ['wow 변수 객체', '전역 변수 객체'], //13장에 배운 렉시컬 스코핑에 따라 wow 함수의 스코프 체인은 선언시에 정해지게 된다.
    this: window
}
```

컨텍스트가 생긴 후 함수가 실행 되게 되며, 이때에 say 함수는 종료된게 아니다.
wow 함수 안의 console.log(word + ' ' + name)은 처음에 wow 컨텍스트에서 찾게 되며, word는 arguments에서 찾을 수 있고, name은 wow 변수객체에 값이 없으니, scope chain을 따라 전역 스코프에서 찾게 된다. 전역 변수객체에 variable의 name이 test라고 되어 있기 때문에 console에는 hello test 가 찍히게 된다. wow 컨텍스트에 따르면 wow 함수는 say 컨텍스트와 관련이 없는 것이다.
wow 함수 종료 후 wow 컨텍스트가 사라지고 say 함수가 마무리 되면, say 컨텍스트, 전역 컨텍스트 순으로 사라지게 된다.

## 호이스팅

호이스팅이란 변수를 선언하고 초기화 했을 떄 선언 부분이 최상단으로 끌어올려지는 현상을 의미한다.

```
console.log(test);   //undefined
say();  //정상적 say
function say() {
    console.log('say');
}
var test = 'test';
```

선언보다 호출을 먼저 하기에 에러가 날것 같지만 정상 동작하게 된다.
변수 선언과 함수 선언식이 최상단으로 끌어올려졌기 때문이다.

```
function say() {
    console.log('say');
}
var test;
console.log(test);   //undefined
say();  //정상적 say
test = 'test';
```

위의 코드는 다음과 같다.
하지만 같은 함수여도 함수 표현식으로 선언한 경우에는 에러가 발생하게 된다.

```
say(); // (3)
sayYeah(); // (5) 여기서 대입되기 전에 호출해서 에러
var sayYeah = function() { // (1) 선언 (6) 대입
  console.log('yeah');
}
function sayWow() { // (2) 선언과 동시에 초기화(호이스팅)
  console.log('wow'); // (4)
}
```

sayWow 함수는 함수 선언식이므로 컨텍스트 생성 후 바로 대입된다.

```
'전역 컨텍스트': {
  변수객체: {
    arguments: null,
    variable: [{ sayWow: Function }, 'sayYeah'],
  },
  scopeChain: ['전역 변수객체'],
  this: window,
}
```

sayYeah는 대입되기 전에 호출해서 에러가 발생하게 된다.
