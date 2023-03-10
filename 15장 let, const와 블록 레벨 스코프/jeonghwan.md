- 15.1 var 키워드로 선언한 변수의 문제점
    
    ES5까지 변수를 선언할 수 있는 유일한 방법은 var 키워드 사용뿐이었다. 
    
    - 15.1.1 변수 중복 선언 허용
        
        var 키워드로 선언한 변수는 중복 선언이 가능하다.
        
        ```jsx
        var x = 1;
        var y = 1;
        
        var x = 100;
        //이건 무시되며 에러를 뱉지 않는다.
        var y;
        
        console.log(x); // 100
        console.log(y); // 1
        ```
        
        동일한 이름의 변수가 선언되어 있는 것을 모르고 변수를 중복 선언하면서 값까지 할당했다면 의도치 않게 먼저 선언된 변수 값이 변경되는 일이 발생한다.
        
    - 15.1.2 함수 레벨 스코프
        
        var 키워드로 선언한 변수는 오로지 함수의 코드 블록만을 지역 스코프로 인정한다. 따라서 함수 외에서 var키워드로 선언한 변수는 코드블록 내에서 선언해도 전역 변수가 된다.
        
        ```jsx
        var x=1;
        
        if(true){
        	//x는 이미 전역변수
        	var x=10;
        }
        
        console.log(x); // 10
        ```
        
        ```jsx
        var i = 10;
        
        for( var i = 0; i< 5; i++){
        	console.log(i);
        }
        
        console.log(i); //5
        ```
        
        함수 레벨 스코프는 전역 변수를 남발할 가능성이 높고 이로 인해 중복 선언되는 경우가 발생한다.
        
    - 15.1.3 변수 호이스팅
        
        var 키워드로 변수를 선언하면 변수 호이스팅에 의해 변수 선언문이 스코프의 선두로 끌어올려진것 처럼 동작한다.  변수 호이스팅에 의해 var 키워드로 선언한 변수는 변수 선언문 이전에 참조할 수 있다. 하지만 할당문 이전에 변수를 참조하면 undefined를 반환한다.
        
        ```jsx
        console.log(foo); // undefined
        
        foo = 123;
        
        console.log(foo); //123
        
        var foo;
        ```
        
        에러가 나진 않지만 가독성이 떨어지고 오류발생 여지를 남김
        
- 15.2 let 키워드
    
     var 키워드의 단점을 보완하기 위해 ES6에서는 새로운 변수 선언 키워드인 let , const를 도입했다
    
    - 15.2.1  변수 중복 선언 금지
        
        var 키워드로 동일한 이름의 변수를 중복 선언해도 아무런 에러가 발생하지 않는다. 
        
        하지만 let 키워드로 이름이 같은 변수를 중복선언하면 문법 에러가 발생한다.
        
        ```jsx
        var foo = 123;
        
        var foo = 456;
        
        let bar = 123;
        
        let bar = 456;
        
        ```
        
    - 15.2.2 블록 레벨 스코프
        
        var 키워드로 선언한 변수는 오로지 함수의 코드 블록만들 지역 스코프로 인정하는 함수 레벨 스코프를 따른다. 하지만 let 키워드로 선언한 변수는 모든 코드 블록을 지역 스코프로 인정하는 블록 레벨 스코프를 따른다.
        
        ```jsx
        let foo = 1;
        
        {
        	let foo = 2;
        	let bar = 3;
        }
        
        console.log(foo); //1
        console.log(bar); //못찾음 
        
        ```
        
        전역 변수에 선언된 foo 와 블록 안에 선언된 foo 와 bar 는 다른 변수이다.
        
        ```jsx
        let i = 10;
        
        function foo(){
        	let i = 100;
        
        	for (let i = 1; i < 3; i +=){
        		console.log(i); // 1 , 2
        	}
        	console.log(i); // 100
        }
        
        console.log(i); //10
        ```
        
    - 15.2.3 변수 호이스팅
        
        var 키워드로 선언한 변수와 달리 let 키워드로 선언한 변수는 변수 호이스팅이 발생하지 않는 것처럼 동작한다.
        
        ```jsx
        console.log(bar);//모름 
        
        let bar;
        ```
        
        변수 선언문 이전에 변수를 참조하면 참조 에러가 발생함
        
        var 키워드는 암묵적으로 자바스크립트 엔진에 의해 암묵적으로 ‘선언단계’와 ‘초기화 단계’가 한번에 진행된다
        
        선언 단계에서 스코프에 변수 식별자를 등록해 자바스크립트 엔진에 변수의 존재를 알린다.그리고 즉시 초기화 단계에서 undefined로 변수를 초기화 한다. 따라서 변수 선언문 이전에 변수에 접근해도 스코프에 변수가 존재하기 때문에 에러가 발생하진 않는다. 하지만 undefined를 반환하고 이후 할당문에 도달하면 비로소 값이 할당된다.
        
        ```jsx
        console.log(foo); //선언 초기화
        
        var foo; 
        console.log(foo); undefined
        
        foo = 1; //할당단계
        ```
        
        let키워드로 선언한 변수는 ‘선언 단계’와 ‘초기화 단계’가 분리되어 진행된다.
        
        런타임 전에 자바스크립트 엔진에 의해 암묵적으로 선언 단계가 먼저 실행되지만 초기화 단계는 변수 선언문에 도달했을때 실행된다. 만약 초기화 단계가 실행되기 전에 접근하려하면 참조 에러가 발생한다.
        
        스코프의 시작 지점부터 초기화 지점까지 변수를 참조할 수 없는 구간을 **일시적 사각지대** 라고 한다.
        
        ```jsx
        console.log(foo); //참조에러
        
        let foo; 
        console.log(foo); //undefined 초기화단계
        
        foo = 1;
        console.log(foo); //1   할당단계 실행
        ```
        
        ```jsx
        let foo = 1;
        
        {
        	console.log(foo);  // 선언 단계 전이므로 참조에러 발생 
        	let foo = 2;
        }
        ```
        
    - 15.2.4 전역 객체와 let
        
        var 키워드로 선언한 전역 변수와 전역 함수,   그리고 선언하지 않은 변수에 값을 할당한 암묵적 전역은 전역 객체window의 프로퍼티가 된다. 전역 객체의 프로퍼티를 참조할때 window를 생략 할 수 있다. (참조할때 앞에 window.으로 시작 )
        
        let 키워드로 선언한 전역 변수는 전역 객체의 프로퍼티가 아니다. 
        
        let 전역 변수는 보이지 않는 개념적인 블록(정적 스코프) 내에 존재하게 된다.
        
- 15.3 const 키워드
    
    const 키워드는 상수를 선언하기 위해 사용한다. 하지만 반드시 상수만을 위해 사용하지는 않는다.
    
    - 15.3.1 선언과 초기화
        
        const 키워드로 선언한 변수는 반드시 선언과 동시에 초기화해야 한다.
        
        ```jsx
        const foo = 1;
        ```
        
        ```jsx
        const foo; //문법 에러
        ```
        
        const키워드로 선언한 변수는 let 키워드와 선언한 변수와 마찬가지로 블록 레벨 스코프를 가지며 호이스팅이 발생하지 않는것 처럼 동작한다.
        
    - 15.3.2 재할당 금지
        
        var 또는 let 키워드로 선언한 변수는 재할당이 자유로우나 const 키워드로 선언한 변수는 재할당이 금지된다.
        
        ```jsx
        const foo = 1;
        foo = 2; // 안됌 
        ```
        
    - 15.3.3 상수
        
        const 키워드로 선언된 변수에 원시 값을 할당한 경우 원시 값은 변경할 수 없는 값이고 const 키워드에 의해 재할당이 금지되므로 할당된 값을 변경할 수 있는 방법은 없다. 상수는 프로그램 전체에서 공통적으로 사용하므로 나중에 상수만 변경하면 되기때문에 유지보수성이 대폭 상향 된다.
        
    - 15.3.4 const 키워드와 객체
        
        const 키워드로 선언된 변수에 원시 값을 할당한 경우 값을 변경할 수 없다. 하지만 const 키워드로 선언된 변수에 객체를 할당한 경우 값을 변경할 수 있다.
        
        ```jsx
        const person = {
        	name : 'Lee'
        }
        
        person.name = 'Kim';
        
        console.log(person); // {name:Kim}
        ```
        
        const 키워드는 재할당을 금지할 뿐 ‘불변’을 의미하진 않는다. 새로운 값을 재할당 하는것은 불가능 하지만 프로퍼티 동적 생성, 프로퍼티 값의 변경을 통해 객체를 변경하는 것은 가능하다.
        
- 15.4 var vs let vs const
    
    ES6을 사용한다면 var 키워드는 사용하지 않는다.
    
    재할당이 필요한 경우에 한정으로  let 키워드를 사용한다.
    
    변경이 발생하지 않고 읽기전용으로 사용하는 원시값과 객체에는 const키워드를 사용한다.