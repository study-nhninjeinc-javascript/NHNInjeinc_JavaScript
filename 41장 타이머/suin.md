# 41장 타이머

## 41.1 호출스케줄링

- 함수를 명시적으로 호출하면 함수가 즉시 실행된다
- 함수를 명시적으로 호출하지 않고일정 시간이 경과된 이후에 호출되도록 예약하려면 타이머 함수를 사용한다. → 이를 호출 스케줄링 ****이라 한다.
- 자바스크립트는 타이머를 생성할 수 있는 타이머 함수 setTimeout과 setInterval,
타이머를 제거할 수 있는clearTimeout과 clearInterval을 제공한다.
- 타이머 함수는 ECMAScript 사양에 정의된 빌트인 함수가 아니다.브라우저 환경과 Node.js 환경에서 전역 객체의 메서드로 제공한다 → 타이머 함수는 호스트 객체다.
- 타이머 함수 setTimeout과 setInterval은 모두 일정 시간이 경과된 이후 콜백 함수가 호출
setTimeout - 단 한 번 동작, setInterval - 반복 동작
- 자바스크립트 엔진은 단 하나의 실행 컨텍스트 스택을 갖기 때문에 두 가지 이상의 태스크를 동시에 실행할 수 없다.
- 즉, 자바스크립트 엔진은 **싱글 스레드**로 동작한다.
이런 이유로 setTimeout과 setInterval은 **비동기 처리 방식**으로 동작한다.

## 41.2 타이머 함수

### setTimeout / clearTimeout

- setTimeout 함수는 두 번째 인수로 전달받은 시간으로 단 한 번 동작하는 타이머를 생성한다.
- 이후 타이머가 만료되면 첫 번째 인수로 전달받은 콜백 함수가 호출된다.
- 즉, setTimeout 함수의 콜백 함수는 두번째 인수로 전달받은 시간 이후 단 한번 실행되도록 스케줄링된다.

```jsx
const timeoutId = setTimeout(func | code [, delay, param1, param2, ...]);
```

- setTimeout 함수는 생성된 타이머를 식별할 수 있는 고유한 타이머 id를 반환한다. setTimeout 함수가 반환한 타이머 id는 브라우저 환경인 경우 숫자이며 Node.js 환경인 경우 객체다.
- setTimeout 함수가 반환한 id를 clearTimeout 함수의 인수로 전달하여 타이머를 취소할 수 있다.

```jsx
// 1초(1000ms) 후 타이머가 만료되면 콜백 함수가 호출된다.
// setTimeout 함수는 생성된 타이머를 식별할 수 있는 고유한 타이머 id를 반환한다.
const timerId = setTimeout(() => console.log('Hi!'), 1000);

// setTimeout 함수가 반환한 타이머 id를 clearTimeout 함수의 인수로 전달하여 타이머를
// 취소한다. 타이머가 취소되면 setTimeout 함수의 콜백 함수가 실행되지 않는다.
clearTimeout(timerId);
```

### setInterval / clearInterval

- setInterval 함수는 두 번째 인수로 전달받은 시간으로 동작하는 타이머를 생성한다.
- 이후 타이머가 만료될 때마다 첫 번째 인수로 전달받은 콜백 함수가 반복 호출된다.
- 이는 타이머가 최소될 때까지 계속된다.

```jsx
const timeoutId = setInterval(func | code [, delay, param1, param2, ...]); // 형식
```

- setInterval 함수는 생성된 타이머를 식별할수있는 고유한 타이머 id를 반환한다. setInterval 함수가 반환한 타이머 id는 브라우저 환경인 경우 숫자이며 Node.js 환경인 경우 객체다.
- setInterval 함수가 반환한 id를 clearInterval 함수의 인수로 전달하여 타이머를 취소할 수 있다.
→ setTimeout 똑같이 id값을 통해 타이머를 취소를 할 수 있다

```jsx
let count = 1;

// 1초(1000ms) 후 타이머가 만료될 때마다 콜백 함수가 호출된다.
// setInterval 함수는 생성된 타이머를 식별할 수 있는 고유한 타이머 id를 반환한다.
const timeoutId = setInterval(() => {
  console.log(count); // 1 2 3 4 5
  // count가 5이면 setInterval 함수가 반환한 타이머 id를 clearInterval 함수의
  // 인수로 전달하여 타이머를 취소한다. 타이머가 취소되면 setInterval 함수의 콜백 함수가
  // 실행되지 않는다.
  if (count++ === 5) clearInterval(timeoutId);
}, 1000);
```

## 41.3 디바운스와 스로틀

- scroll, resize, input, mousemove 같은 이벤트는 짧은 시간 간격으로 연속해서 발생한다
이러한 이벤트에 바인딩한 이벤트 핸들러는 과도하게 호출되어 성능에 문제를 일으킬 수 있다
- 디바운스와 스로틀은 짧은 시간 간격으로 연속해서 발생하는 이벤트를 그룹화해 과도한 이벤트 핸들러의 호출을 방지하는 기법이다

### 디바운스

- 짧은 시간 간격으로 이벤트가 연속해서 발생하면 이벤트 핸들러를 호출하지 않다가 일정 시간이 경과한 이후에 이벤트 핸들러가 한 번만 호출되도록 한다.
→ 짧은 시간 간격으로 발생하는 이벤트를 그룹화해서 마지막에 한 번만 호출되도록 한다.

```html
	<input type="text">
  <div class="msg"></div>
  <script>
    const $input = document.querySelector('input');
    const $msg = document.querySelector('.msg');

    const debounce = (callback, delay) => {
      let timerId;
      // debounce 함수는 timerId를 기억하는 클로저를 반환한다.
      return event => {
        // delay가 경과하기 이전에 이벤트가 발생하면 이전 타이머를 취소하고
        // 새로운 타이머를 재설정한다.
        // 따라서 delay보다 짧은 간격으로 이벤트가 발생하면 callback은 호출되지 않는다.
        if (timerId) clearTimeout(timerId);
        timerId = setTimeout(callback, delay, event);
      };
    };

    // debounce 함수가 반환하는 클로저가 이벤트 핸들러로 등록된다.
    // 300ms보다 짧은 간격으로 input 이벤트가 발생하면 debounce 함수의 콜백 함수는
    // 호출되지 않다가 300ms 동안 input 이벤트가 더 이상 발생하면 한 번만 호출된다.
    $input.oninput = debounce(e => {
      $msg.textContent = e.target.value;
    }, 300);
  </script>
```

- 사용자의 입력 완료 여부는 정확히 알 수 없으므로 일정 시간 동안 텍스트 입력 필드에 값을 입력하지 않으면 입력이 완료된 것으로 간주한다.
- 이를 위해 debounce 함수가 반환한 함수는 debounce함수에 두번째 인수로 전달한 시간보다 짧은 간격으로 이벤트가 발생하면 이전타이머를 취소하고 새로운 타이머를 재설정한다.
- 따라서 delay 동안 이벤트가 더 이상 발생하지 않으면 그때 호출 된다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/fc5b75db-fec8-4001-8b55-45e12393e03c/Untitled.png)

- 디바운스는 resize이벤트 처리나 input 요소에 입력된 값으로 ajax 요청하는 입력필드 자동완성 UI구현, 버튼 중복 클릭 방지 처리 등에 유용하게 사용된다.
- 실무에서는 underscore의 debounce 함수나 lodash의 debounce함수를 사용하는 것을 권장

### 스로틀

- 짧은 시간 간격으로 이벤트가 연속해서 발생하더라도 일정 시간 간격으로 이벤트 핸들러가 최대 한 번만 호출되도록 한다
→ 짧은 시간 간격으로 연속해서 발생하는 이벤트를 그룹화해서 일정 시간 단위로 이벤트 핸들러가 호출되도록 주기를 만든다.

```html
	<div class="container">
    <div class="content"></div>
  </div>
  <div>
    일반 이벤트 핸들러가 scroll 이벤트를 처리한 횟수:
    <span class="normal-count">0</span>
  </div>
  <div>
    스로틀 이벤트 핸들러가 scroll 이벤트를 처리한 횟수:
    <span class="throttle-count">0</span>
  </div>

  <script>
    const $container = document.querySelector('.container');
    const $normalCount = document.querySelector('.normal-count');
    const $throttleCount = document.querySelector('.throttle-count');

    const throttle = (callback, delay) => {
      let timerId;
      // throttle 함수는 timerId를 기억하는 클로저를 반환한다.
      return event => {
        // delay가 경과하기 이전에 이벤트가 발생하면 아무것도 하지 않다가
        // delay가 경과했을 때 이벤트가 발생하면 새로운 타이머를 재설정한다.
        // 따라서 delay 간격으로 callback이 호출된다.
        if (timerId) return;
        timerId = setTimeout(() => {
          callback(event);
          timerId = null;
        }, delay, event);
      };
    };

    let normalCount = 0;
    $container.addEventListener('scroll', () => {
      $normalCount.textContent = ++normalCount;
    });

    let throttleCount = 0;
    // throttle 함수가 반환하는 클로저가 이벤트 핸들러로 등록된다.
    $container.addEventListener('scroll', throttle(() => {
      $throttleCount.textContent = ++throttleCount;
    }, 100));
  </script>
```

- throttle 함수가 반환한 함수는 throttle 함수에 두번째 인수로 전달한 시간이 경과하기 이전에 이벤트가 발생하지 않다가 delay 시간이 경과했을때 이벤트가 발생하면 콜백 함수를 호출하고 새로운 타이머를 재설정한다. delay 시간간격으로 콜백함수가 호출된다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2d3d7fb6-f6a2-4de6-a3c1-b2367fb403aa/Untitled.png)

- scroll이벤트 처리나 무한 스크롤 ui구현에 유용하게 사용된다.
- 실무에서는 throttle 함수나 lodash의 throttle 함수를 사용하는 것을 권장