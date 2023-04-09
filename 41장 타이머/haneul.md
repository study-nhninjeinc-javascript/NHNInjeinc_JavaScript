# 타이머

## 호출 스케줄링

> 함수를 명시적으로 호출하면 함수가 즉시 실행된다. 이때 함수를 일정 시간이 경과된 이후 호출되도록 함수 호출을 예약하려면 타이머 함수를 사용한다.
> 
- 타이머를 생성하는 setTimeout, setInterval 함수
- 타이머를 제거하는 clearTimeout, clearInterval 함수

<aside>
💡 타이머 함수는 빌트인 함수가 아닌 전역 객체의 메서드 즉 호스트 객체이다.

</aside>

- setTimeout 함수가 생성한 타이머는 단 한 번 동작하고, setInterval 함수가 생성한 타이머는 반복 동작한다.
    - setTimeout 함수의 콜백 함수는 타이머가 만료되면 단 한 번 호출
    - setInterval 함수의 콜백 함수는 타이머가 만료될 때마다 반복 호출

<aside>
💡 자바스크립트 엔진은 단 하나의 실행 컨텍스트 스택을 갖기 때문에 두 가지 이상의 작업을 동시에 실행할 수 없다. → 싱글 스레드

</aside>

- 타이머 함수는 비동기 처리 방식으로 동작한다.

## 타이머 함수

### setTimeout / clearTimeout

> 두 번째 인수로 전달받은 시간(ms)으로 단 한 번 동작하는 타이머를 생성한다. - 타이머 만료 시 첫 번째 인수로 전달받은 콜백 함수가 호출된다.
> 
- 첫 번째 인수는 콜백 함수
- 두 번째 인수는 시간
    - 시간이 되었을 때 콜백 함수가 즉시 호출되는 것이 보장되진 않는다. - 태스크 큐에 콜백 함수를 등록하는 시간을 지연할 뿐 → 태스크 큐는 비동기 프로그래밍에서 배움.
    - 두 번째 인수를 생략하면 기본값(0)이 지정된다.
    - 최소 지연 시간은 4ms다. (4ms 이하의 값이 들어오면 4ms 로 지정된다.)
- 이후 인수를 통해 콜백 함수에 전달해야할 인수를 전달할 수 있다.

```jsx
// 1초 후 타이머가 만료되면 콜백 함수 호출
setTimeout(() => console.log('Hi!'), 1000);

// 1초 후 콜백 함수 호출 + 인수로 문자열 'Lee' 전달
setTimeout(name => console.log(`Hi! ${name}`), 1000, "Lee');
```

- setTimeout 함수는 생성도니 타이머를 식별할 수 있는 고유한 타이머 id를 반환한다.
    - 해당 id는 브라우저 환경인 경우 숫자이며 Node.js 환경인 경우 객체다.
    - id를 통해 clearTimeout 함수의 인수로 전달하여 타이머를 취소할 수 있다.

```jsx
const timerId = setTimeout(() => console.log('Hi!'), 1000);

// setTimeout 함수가 반환한 id를 clearTimeout 함수의 인수로 전달하여 타이머를 취소한다.
clearTimeout(timerId); // 해당 id를 가진 타이머 함수의 콜백 함수가 실행되지 않는다.
```

### setInterval / clearInterval

> 두 번째 인수로 전달받은 시간으로 반복 동작하는 타이머를 생성한다. - 타이머 만료 시마다 첫 번째 인수로 전달받은 콜백 함수가 반복 호출됨
> 
- setInterval 함수 또한 타이머를 식별할 수 있는 id를 반환한다.
    - 브라우저인 경우 숫자 Node.js 환경인 경우 객체다.

```jsx
let count = 0;

// 1초마다 콜백 함수가 호출된다.
const timeoutId = setInterval(() => {
	console.log(++count);

	// count의 값이 5면 해당 타이머 id를 clearInterval 함수의 인수로 전달하여 타이머를 취소
	if (count ===5) clearInterval(timeoutId);
}, 1000);
```

## 디바운스와 스로틀

> scroll, resize, input, mousemove 같은 이벤트는 짧은 시간 간격으로 연속해서 발생한다.
> 
- 이러한 이벤트에 바인딩한 이벤트 핸들러는 과도하게 호출되어 성능에 문제를 일으킬 수 있다.

<aside>
💡 디바운스와 스로틀은 짧은 시간 간격으로 연속해서 발생하는 이벤트를 그룹화해서 과도한 이벤트 핸들러의 호출을 방지하는 프로그래밍 기법이다.

</aside>

```html
<!DOCKTYPE html>
<html>
<body>
	<button>click</button>
	<pre>NORMAL CLICK EVENT COUNTER     <sapn class="normal-msg">0</span></pre>
	<pre>DEBOUNCE CLICK EVENT COUNTER   <sapn class="debounce-msg">0</span></pre>
	<pre>THROTTLE CLICK EVENT COUNTER   <sapn class="throttle-msg">0</span></pre>
	<script>
		const $button = document.querySelector('button');
		const $normalMsg = document.querySelector('.normal-msg');
		const $debounceMsg = document.querySelector('.debounce-msg');
		const $throttleMsg = document.querySelector('.throttle-msg');

		const debounce = (callback, delay) => {
			let timerId;

			return (...args) => {
				if (timerId) clearTimeout(timerId);
				timerId = setTimeout(callback, delay, ...args);
			};
		};

		const throttle = (callback, delay) => {
			let timerId;

			return (...args) => {
				if (timerId) return;
				timerId = setTimeout(() => {
					callback(...args);
					timerId = null;
				}, delay);
			};
		};

		$button.addEventListener('click', () => {
			$normalMsg.textContent = +$normalMsg.textContent + 1;
		});

		$button.addEventListener('click', debounce(() => {
			$debounceMsg.textContent = +$debounceMsg.textContent + 1;
		}, 500));

		$button.addEventListener('click', throttle(() => {
			$throttleMsg.textContent = +$throttleMsg.textContent + 1;
		}, 500));
	</script>
</body>
</html>
```

![debounce, throttle.PNG](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/fd1c7127-ad79-432d-808c-acc1c642cf29/debounce_throttle.png)

### 디바운스

> 짧은 시간 간격으로 이벤트가 연속해서 발생하면 이벤트 핸들러를 호출하지 않다가 일정 시간 경과 후 이벤트 핸들러가 한 번만 호출되도록 한다.
> 

→ 짧은 시간 간격으로 발생하는 이벤트를 그룹화하여 마지막에 한 번만 이벤트 핸들러가 호출되도록 한다.

```html
<!DOCKTYPE html>
<html>
<body>
	<input type="text">
	<div class="msg"></div>

	<script>
		const $input = document.querySelector('input');
		const $msg = document.querySelector('.msg');

		// timerId 를 기억하는 클로저를 반환
		const debounce = (callback, delay) => {
			let timerId;
			
			return (...args) => {
				// delay 가 경과하기 이전에 이벤트 발생 시 이전 타이머 취소 및 새로운 타이머 설정
				// -> delay보다 짧은 간격으로 에벤트 발생 시 callback 호출 x (호출 전에 취소됨)
				if (timerId) clearTimeout(timerId);
				// timerId 를 새로운 타이머에 대한 id로 재할당.
				timerId = setTimeout(callback, delay, ...args);
			};
		};

		// 
		$input.oninput = debounce(e => {
			$msg.textContent = e.target.value;
		}, 500);
	</script>
</body>
</html>
```

<aside>
💡 디바운스는 자동완성 UI 구현, 버튼 중복 클릭 방지 처리 등에 유용하게 사용된다. → Underscore의 debounce 함수나 Lodash의 debounce 함수를 사용하는 것을 권장

</aside>

```jsx
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<meta http-equiv="X-UA-Compatible" content="IE=edge">
	<meta name="viewport" content="width=device-width, initial-scale=1.0">
	<title>LODASH TEST</title>

	<!-- Lodash lib -->
	<script src="js/lodash.js" type="text/javascript"></script>
</head>
<body>
	<div class="container">
		<div class="wrap" style="width: 300px; height: 300px; overflow: scroll;">
			<div class="sto" style="width: 300px; height: 1000vh; background-color: firebrick;">
			</div>
		</div>
	</div>

	<script>
		const scrollEvent = _.debounce(function() {
		    console.log('scrolling');
		}, 500);

		document.querySelector('.wrap').addEventListener('scroll', scrollEvent);
	</script>
</body>
</html>
```

### 스로틀

> 짧은 시간 간격으로 이벤트가 연속해서 발생하더라도 일정 시간 간격으로 이벤트 핸들러가 최대 한 번만 호출되도록 한다.
> 
- 짧은 시간 간격으로 연속해서 발생하는 이벤트를 그룹화해서 일정 시간 단위로 이벤트 핸들러가 호출되도록 호출 주기를 만든다.

```html
<!DOCKTYPE html>
<html>
<head>
	<style>
		.container {
			width: 300px;
			height: 300px;
			background-color: rebeccapurple;
			overflow: scroll;
		}

		.content {
			width: 300px;
			height: 1000vh;
		}
	</style>
</head>
<body>
	<div class="container">
		<div class="content"></div>
	</div>

	<div>
		NORMAL EVENT HANDLER SCROLL EVENT COUNT :
		<span class="normal-count">0</span>
	</div>
	<div>
		THROTTLE EVENT HANDLER SCROLL EVENT COUNT : 
		<span class="throttle-count">0</span>
	</div>

	<script>
		const $container = document.querySelector('.container');
		const $normalCount = document.querySelector('.normal-count');
		const $throttleCount = document.querySelector('.throttle-count');

		const throttle = (callback, delay) => {
			let timerId;

			// timerId를 기억하는 클로저를 반환
			return (...args) => {
				// delay가 경과하기 이전에 이벤트가 발생하면 아무것도 하지 않는다.
				// delay가 경과했을 때 이벤트가 발생하면 새로운 타이머를 설정한다.
				// delay 간격대로 callback이 호출된다.
				if (timerId) return;
				timerId = setTimeout(() => {
					callback(...args);
					timerId = null;
				}, delay);
			};
		};

		let normalCount = 0;
		$container.addEventListener('scroll', () => {
			$normalCount.textContent = ++normalCount;
		});

		let throttleCount = 0;
		$container.addEventListener('scroll', throttle(() => {
			$throttleCount.textContent = ++throttleCount;
		}, 100));
	</script>
</body>
</html>
```

- 짧은 시간 간격으로 연속해서 발생하는 이벤트를 그룹화해서 일정 시간 간격으로 이벤트 핸들러를 호출한다.
    - 주로 scroll 이벤트 처리나 무한 스크롤 UI 구현 등에 사용된다.

<aside>
💡 Underscore의 throttle 함수나 Lodash의 throttle 함수를 사용하는 것을 권장한다.

</aside>