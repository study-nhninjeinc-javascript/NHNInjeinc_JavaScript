자바스크립트의 **모든 값은 데이터 타입이 존재** !

ES6부터 7개의 데이터 타입 제공

**원시타입 vs 객체타입**

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4e33e7ac-bef3-4ead-b86c-5cf123291b52/Untitled.png)

### 

- **숫자타입**
    
    ```jsx
    var interget = 10; //정수
    var double = 10.12; //실수
    var negative = -20; //음의 정수
    ```
    
    ```jsx
    // 숫자 타입의 세 가지 특별 값
    console.log(10 / 0); //Infinity
    console.log(10 / -10); //-Infinity
    console.log(1* 'String') // NaN
    ```
    
- **문자열 타입**
    
    자바스크립트 엔진의 문자열은 **원시타입**
    
    문자열은 0개 이상의 16비트 유니코드 문자의 집합 
    
    **백틱을 `` 사용**
    
    ```jsx
    var string;
    string = 't';
    string = "t";
    string = `t`; //es6
    ```
    
- **템플릿 리터럴**
    
    ```jsx
    var template = `Template literal`;
    console.log(template); // Template literal
    ```
    
    ### 멀티라인 문자열
    
    ```jsx
    var str = 'Hello
    word';
    //SynraxError
    
    var str2 = `Heelo
    word`;
    //'Heelo\nword'
    ```
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/06af91b8-094d-4eb3-ac45-9a9f8b2397d8/Untitled.png)
    
- **불리언 타입**
    
    ```jsx
    var foo = true;
    console.log(foo) // true
    
    foo = false;
    console.log(foo) // false
    ```
    
- **undefined 타입**
    
    ```jsx
    var foo;
    console.log(foo) // undefined  var만
    ```
    
    ⚠️변수가 없다는 걸 표현하고 싶을 땐 null로 할당
    
- **null 타입**
    
    ```jsx
    var foo ='kim';
    //이전 참조 제거 foo 변수는 더이상 kim참조 x
    //변수 소멸
    foo = null;
    ```
    
- **심벌 타입**
    
    이름이 충동할 위험이 없는 객체의 **유일한** 프로퍼티 키를 만들기 위해
    
    ```jsx
    //심벌 값 생성
    var key = Symbol('key');
    console.log(typeof key) // symbol
    
    var obj ={};
    
    //이름이 충동할 위험이 없는 유일무일한 값인 심벌을 프로퍼티 키로 사용
    obj[key] = 'value';
    console.log(obj[key]); //value
    
    console.log(obj)
    {Symbol(key): 'value'}
    ```
    
- 객체 타입
    
    11장 원시 값과 객체에서 다룬 내용이기에 참고
    
- **데이터 타입의 필요성**
    - 값을 저장할 때 확보해야 하는 **메모리 공간의 크기를 결정**
    - 값을 참조할 때 한 번에 읽어 들여야 할 **메모리 공간의 크기 결정**
    - 메모리에서 읽어 들인 **2진수를 어떻게 해석할지 결정**
- 동적 타이핑
    
    typeof 연산자로 변수를 연산하면 데이터 타입을 반환하는 것이 아니라 
    
    **할당된 값의 데이터 타입을 반환**
    
    재할당에 의해 변수의 타입은 언제든지 동적으로 변할 수 있다.(자바스크립트 특징)