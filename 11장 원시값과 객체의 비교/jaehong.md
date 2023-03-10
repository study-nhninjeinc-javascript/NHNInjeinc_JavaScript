## 11장 원시 값과 객체의 비교

### 원시값?

-   원시 타입의 값은 변경 불가능한 값이다.
-   변수는 메모리 공간을 확보하기 메모리 공간을 식별하기 위해 사용되며, 재할당을 통해 원본의 값을 복사해온후 새로운 메모리 공간에 값을 넣고 그 변수가 참조하고 있는 값을 변경 시킬 수 있다.
-   즉, 원시 값 자체를 변경할 수 없을 뿐, 변수의 값은 변경할 수 있다.

```
const o = {};
//할당된 객체는 변경 가능
o.a = 1;
console.log(o)
```

### 객체

-   객체는 프로퍼티의 개수가 정해져 있지 않으며, 동적으로 추가되고 삭제할 수 있다. 따라서 객체는 원시 값과 같이 확보해야 할 메모리 공간의 크기를 사전에 정해 둘 수 없다.

*   자바스크립트 객체는 프로퍼티 키를 인덱스로 사용하는 해시 테이블이라고 생각 할 수 있다. 자바 같은 경우 사전에 정의된 클래스를 기반으로 객체를 생성하여 프로퍼티와 메서드가 정해져 있는데로 객체를 생성한다.

*   이러한 자바스크립트 객체의 특징은 편리하지만, 선언할때의 프로퍼티의 타입 등이 실제로 프로퍼티 값을 접근할 때에 달라질 수 있으므로, 프로퍼티를 접근할 때마다 프로퍼티를 찾아야 합니다.
    이러한 방식을 동적 탐색이라고 합니다.

구글의 V8엔진에서는 히든 클래스라는 방식을 사용하여 프로퍼티에 접근을 한다.

<img src="https://miro.medium.com/max/700/1*0-zCgnZLrFia_zO-V2Btvg.jpeg">

> source : https://ui.toast.com/weekly-pick/ko_20210909
