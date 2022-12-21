# 11장 원시값과 객체의 비교

## 데이터 타입 간단히 알아보기

- 자바스크립트가 제공하는 7가지 데이터 타입은
숫자, 문자열, 불리언, null, undefined, 심벌, 객체 타입
- 크게 원시타입과 객체타입으로 구분할 수 있다.
- 원시타입은 변경 불가능한 값이다.
- 참조 타입의 값은 변경 가능한 값이다.
- 원시값을 변수에 할당하면 변수(확보된 메모리 공간)에는 시제 저장 값이 저장된다.
- 객체의 경우엔 변수에 할당하면 변수(확보된 메모리 공간)에는 참조값이 저장된다.
- 원시값을 갖는 변수를 다른 변수에 할당하면 원본의 원시 값이 복사되어 전달 됨
- 객체를 가리키는 변수를 다른 변수에 할당하면 원본의 참조값이 복사되어 전달 됨

### 원시값

- 위에서 설명했듯 변경 불가능한 값이다.
- 변수는 하나의 값을 저장하기 위해 확보한 메모리 공간 자체 또는 그 메모리 공간을 식별하기 위해 붙힌 이름이고, 값은 변수에 저장된 데이터로서 표현식이 평가되어 생성된 결과를 말한다.
- 변경 불가능하다는 것은 변수가 아니라 값에 대한 진술이다.
- 원시 값은 변경 불가능하다는 말은 원시 값 자체를 변경할 수 없다는 뜻이고 변수값을 변경할 수 없다는 것이 아니다. (
- 변수는 언제든지 재할당을 통해 변수 값을 변경(엄밀히 말하면 교체)할 수 있다. 그렇기 때문에 이름이 변수다.
- 반대로 상수는 단 한 번만 할당이 허용되므로 변수 값을 변경(교체)할 수 없다.
- 원시값에 대한 할당과 재할당을 그림으로 표현하면 아래와 같다.

![https://velog.velcdn.com/images%2Fnomadhash%2Fpost%2F5e197937-108e-4f9c-8be8-0c8c04d3fe8e%2F%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-09-16%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%207.02.57.png](https://velog.velcdn.com/images%2Fnomadhash%2Fpost%2F5e197937-108e-4f9c-8be8-0c8c04d3fe8e%2F%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-09-16%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%207.02.57.png)

- 원시값은 읽기 전용 값이다. 어떤 일이 있어도 불변한다.(메모리에서) 이러한 원시 값의 특성은 데이터의 신뢰성을 보장한다.
- 쉽게 말해 원시값은 변수에 재할당 말고는 값을 변경할 수 없다는 뜻

## 문자열과 불변성

- 원시 타입별로 메모리 공간의 크리가 미리 정해져 있다.
    - 문자열 타입 (2바이트)
    - 숫자 타입 (8바이트)
    - 이외의 원시 타입은 크기를 명확히 규정하고 있지 않아서 브라우저 제조사의 구현에 따라 원시 타입의 크기는 다를 수 있다.
- 문자열은 다른 원시 값과 비교할 때 독특한 특징이 있다. 문자열은 0개 이상의 문자(character)로 이뤄진 집합을 말하며, 1개의 문자는 2바이트의 메모리 공간에 저장된다. 따라서 문자열은 몇 개의 문자로 이뤄졌느냐에 따라 필요한 메모리 공간의 크기가 결정된다.
- 반대로 숫자 값은 1도, 1000000도 동일한 8바이트가 필요하다.
- 문자열의 경우 (실제와는 다르지만 단순하게 계산했을 때) 1개의 문자로 이뤄진 문자열은 2바이트, 10개의 문자로 이뤄진 문자열은 20바이트가 필요한셈이다.

<aside>
💡 문자열은 유사 배열 객체이면서 이터러블 이므로 배열과 유사하게 각 문자에 접근할 수 있다.

</aside>

> 유사 배열 객체란 마치 배열처럼 인덱스로 프로퍼티 값에 접근할 수 있고 length 프로퍼티를 갖는 객체를 말한다. 문자열은 마치 배열처럼 인덱스를 통해 각 문자에 접근할 수 있으며, lenth 프로퍼티를 갖기 때문에 유사 배열 객체이고 for문으로 순회할 수 도 있다.
> 
- 실제 배열과 다른점은 str[0] = ‘S’처럼 이미 생성된 문자열의 일부 문자를 변경해도 반영되지 않는다. 문자열은 변경불가능한 값이기 때문이다.(원시값)

## 값에 의한 전달

- 값에 의한 전달은 아래 그림하나로 모든게 설명된다.

![https://s3.us-west-2.amazonaws.com/secure.notion-static.com/3bb4a658-ac22-4eb0-bc64-b0390e7e5b53/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221220%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221220T134232Z&X-Amz-Expires=86400&X-Amz-Signature=e03de820fa9d00d06315d9d042cc5c8e8e0296a43d0b875758f677811d86b5f5&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/3bb4a658-ac22-4eb0-bc64-b0390e7e5b53/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221220%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221220T134232Z&X-Amz-Expires=86400&X-Amz-Signature=e03de820fa9d00d06315d9d042cc5c8e8e0296a43d0b875758f677811d86b5f5&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

- 원시 타입의 변수를 다른 변수에 할당하면 같은 메모리 주소값을 할당 받는게 아니라 같은 값 자체가 복사되어 새롭게 생성된다는 뜻이고, 주의할 점은 원시 타입이라고 해서 변수는 값 자체를 갖고 있는게 아니라 메모리 주소를 가지고 있는 것이다.

<aside>
💡 원시 타입의 변수를 다른 변수에 할당하면 할당하는 시점에는 두 변수가 같은 원시 값을 참조하다가 어느 한쪽의 변수에 재할당이 이뤄졌을 때 비로소 새로운 메모리 공간에 재할당된 값을 저장하도록 동작할 수도 있다(자바스크립트 엔진 제조사 마다 차이가 있을 수 있다는 뜻). 
그리고 파이썬은 이처럼 동작한다.

</aside>

![https://s3.us-west-2.amazonaws.com/secure.notion-static.com/7734afdc-b60a-4dc9-a65e-4f3e74bd6711/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221220%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221220T134208Z&X-Amz-Expires=86400&X-Amz-Signature=0364d8e80e87dc29308339a848f72ff8193b38408d766e9da328a2af837cd98b&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/7734afdc-b60a-4dc9-a65e-4f3e74bd6711/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221220%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221220T134208Z&X-Amz-Expires=86400&X-Amz-Signature=0364d8e80e87dc29308339a848f72ff8193b38408d766e9da328a2af837cd98b&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

- 정리하면 원시 타입의 변수를 평가하는 두 가지 방식이 있다.
    - 새로운 값을 생성(복사)해서 메모리 주소를 전달하는 방식. 이 방식은 할당 시점에 두 변수가 기억하는 메모리 주소가 다름
    - 메모리 주소를 그대로 전달(공유)하는 방식. 이 방식은 할당 시점에 두 변수가 기억하는 메모리 주소가 같다.

---

## 객체

<aside>
💡 자바스크립트 객체의 관리방식!

</aside>

- 자바스크립트 객체는 프로퍼티 키를 인덱스로 사용하는 해시 테이블(hash table은 연관 배열, map, dictionary, lookup table이라 부르기도 한다.)이라고 생각할 수 있다. 대부분의 자바스크립트 엔진은 해시 테이블과 유사하지만 높은 성능을 위해 일반적인 해시 테이블 보다 나은 방법으로 객체를 구현

![https://s3.us-west-2.amazonaws.com/secure.notion-static.com/f09c0396-efa6-4700-a75d-08d1547475ae/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221220%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221220T134054Z&X-Amz-Expires=86400&X-Amz-Signature=a6a1a5d9013844e2509c73b07122fc8edac2c7cb6c0cb46b5d7dce2cea71b35f&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/f09c0396-efa6-4700-a75d-08d1547475ae/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221220%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221220T134054Z&X-Amz-Expires=86400&X-Amz-Signature=a6a1a5d9013844e2509c73b07122fc8edac2c7cb6c0cb46b5d7dce2cea71b35f&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

- 자바스크립트는 객체가 생성된 이후라도 동적으로 프로퍼티와 메서드를 추가할 수 있다. 이는 사용하기 매우 편리하지만 성능 면에서는 이론적으로 클래스 기반 객체지향 프로그래밍 언어의 객체보다 생성과 프로퍼티 접근에 비용이 더 많이 드는 비효율적인 방식이다.
- 따라서  V8 엔진에서는 프로퍼티에 접근하기 위해 동적 탐색 대신 히든 클래스라는 방식을 사용해 C++ 객체의 프로퍼티에 접근하는 정도의 성능을 보장한다.

### 변경 가능한 값, 참조에 의한 전달

- 객체(참조) 타입의 값, 즉 객체는 변경 가능한 값이다
- 아래 그림을 보면 객체를 할당한 변수에는 생성된 객체가 실제로 저장된 메모리 공간의 주소가 저장되어 있다. 이 값을 참조 값이라고 한다. 변수는 이 참조 값을 통해 객체에 접근할 수 있다.

![https://s3.us-west-2.amazonaws.com/secure.notion-static.com/98252cdc-cd2b-4eca-8da5-c716fe9aef90/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221220%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221220T133906Z&X-Amz-Expires=86400&X-Amz-Signature=f7f2ef04eced7cfa3ed81ef10d4e07e87efc47b20d78e3ba569c4019aa859034&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/98252cdc-cd2b-4eca-8da5-c716fe9aef90/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221220%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221220T133906Z&X-Amz-Expires=86400&X-Amz-Signature=f7f2ef04eced7cfa3ed81ef10d4e07e87efc47b20d78e3ba569c4019aa859034&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

- 위 와 같은 구조로  person 변수는 값으로 객체의 메모리 주소값을 갖고 있기 때문에 실제 객체의 값이 변경되어도 person의 값(메모리 주소값)은 바뀌지 않기 때문에 객체는 변경이 일어나도 const(상수)를 사용할 수 있는것이다.
- 객체는 이러한 구조적 단점에 따른 부작용이 있다. 그것은 원시 값과는 다르게 여러 개의 식별자가 하나의 객체를 공유할 수 있다는 것이다.

> 객체를 프로퍼티 값으로 갖는 객체의 경우 얕은 복사는 한 단계까지만 복사하는 것을 말하고 깊은 복사는 객체에 중첩되어 있는 객체까지 모두 복사하는 것을 말한다.

얕은 복사 : 객체에 중첩되어 있는 객체의 참조 값을 복사한다. (객체 타입일 경우에만 해당 됨) 
깊은 복사 : 객체에 중첩되어 있는 객체까지 모두 복사해서 원시 값처럼 완전한 복사본을 만든다. (원시 타입도 해당 됨)
> 
- 아래 그림처럼 원본 person을 사본 copy에 할당하면 원본 person의 참조 값을 복사해서 copy에 저장한다. (참조에 의한 전달, 다른말론 공유에 의한 전달) 이때 원본 person과 사본 copy 모두 동일한 객체를 가리킴

![https://s3.us-west-2.amazonaws.com/secure.notion-static.com/7a55f6d6-5146-4b50-bda8-8afb993e322b/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221220%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221220T133835Z&X-Amz-Expires=86400&X-Amz-Signature=80a2d2502998757441a40516f25ad453db41f04617fa49a4681cc4b8c9c304cb&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/7a55f6d6-5146-4b50-bda8-8afb993e322b/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221220%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221220T133835Z&X-Amz-Expires=86400&X-Amz-Signature=80a2d2502998757441a40516f25ad453db41f04617fa49a4681cc4b8c9c304cb&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

<aside>
💡 메모리 구조 좀 정확히 알아야겠다

</aside>