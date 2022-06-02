# lodash의 debounce, throttle 구현하기

`_.debounce(func, [wait=0], [options={}])`

- `func (Function)`: The function to debounce.
- `[wait=0] (number)`: The number of milliseconds to delay.
- `[options={}] (Object)`: The options object.
- `[options.leading=false] (boolean)`: Specify invoking on the leading edge of the timeout.  (기본값으로는 false로 wait 시간 이후에 호출된다.)
- `[options.maxWait] (number)`: The maximum time func is allowed to be delayed before it's invoked. (스로틀처럼 maxWait시간만큼의 간격을 두고 함수 호출이 된다. 스로틀 + 디바운스 합쳐진 버전)
- `[options.trailing=true] (boolean)`: Specify invoking on the trailing edge of the timeout.


lodash의 throttle 코드
```js
function throttle(func, wait, options) {
  let leading = true
  let trailing = true

  if (typeof func !== 'function') {
    throw new TypeError('Expected a function')
  }
  if (isObject(options)) {
    leading = 'leading' in options ? !!options.leading : leading
    trailing = 'trailing' in options ? !!options.trailing : trailing
  }
  return debounce(func, wait, {
    leading,
    trailing,
    'maxWait': wait
  })
}
```
lodash debounce를 이용한다. wait을 maxWait으로 사용하고 leading값을 true로 바꿔준다. (debounce에서 기본값은 false)




lodash의 debounce 함수 (전체 코드는 밑 링크에)  
[lodash/debounce.js](https://github.com/lodash/lodash/blob/master/debounce.js)

```js
  function debounced(...args) {
    const time = Date.now()
    const isInvoking = shouldInvoke(time)

    lastArgs = args
    lastThis = this
    lastCallTime = time

    if (isInvoking) { //처음 호출시
      if (timerId === undefined) {
        return leadingEdge(lastCallTime) //함수 호출
      }
      if (maxing) { // 옵션 maxWait 값이 있을 경우 스로틀 실행
        // Handle invocations in a tight loop.
        timerId = startTimer(timerExpired, wait)
        return invokeFunc(lastCallTime)
      }
    }
    if (timerId === undefined) {
      timerId = startTimer(timerExpired, wait)
    }
    return result
  }
  debounced.cancel = cancel
  debounced.flush = flush
  debounced.pending = pending
  return debounced
}
```

1. leadingEdge
디바운스된 결과 반환 후 호출 또는 처음 호출시 호출의 끝부분을 담당하는 함수
```js
  function leadingEdge(time) {
    // Reset any `maxWait` timer.
    lastInvokeTime = time
    // Start the timer for the trailing edge.
    timerId = startTimer(timerExpired, wait)
    // Invoke the leading edge.
    return leading ? invokeFunc(time) : result
  }
```

2. startTimer
```js
  function startTimer(pendingFunc, wait) {

    if (useRAF) {
      root.cancelAnimationFrame(timerId)
      return root.requestAnimationFrame(pendingFunc)
    }
    return setTimeout(pendingFunc, wait)
  }
```

3. timerExpired
```js
  function timerExpired() {
		const time = Date.now()
    if (shouldInvoke(time)) {
			return trailingEdge(time)
    }
    // Restart the timer. shouldInvoke가 false면 timer를 재시작한다.
    timerId = startTimer(timerExpired, remainingWait(time))
  }
```

4. remainingWait
```js
  function remainingWait(time) {
    const timeSinceLastCall = time - lastCallTime
    const timeSinceLastInvoke = time - lastInvokeTime
    const timeWaiting = wait - timeSinceLastCall

    return maxing
      ? Math.min(timeWaiting, maxWait - timeSinceLastInvoke)
      : timeWaiting
  }
```

5. trailingEdge
```js
  function trailingEdge(time) {
    timerId = undefined

    // Only invoke if we have `lastArgs` which means `func` has been
    // debounced at least once.
    // 'func'가 한 번 이상 디바운스되었음을 의미하는 'lastArgs'가 있는 경우에만 호출합니다.
    if (trailing && lastArgs) {
      return invokeFunc(time)
    }
    lastArgs = lastThis = undefined
    return result
  }
```

6. invokeFunc
```js
  function invokeFunc(time) {
    const args = lastArgs
    const thisArg = lastThis

    lastArgs = lastThis = undefined
    lastInvokeTime = time
    result = func.apply(thisArg, args)
    return result
  }
```

직접 구현해 본 debounce 함수

```js
function debounceTest(func, wait, options) {
	if (typeof func !== "function") {
		throw new TypeError('Expected a function');
	}

	let lastCallTime;
	let isInvoking;
	let timeId;
	let maxing = false;
	let leading = false;

	if (typeof options === "object") {
		maxing = "maxWait" in options;
	}

	function invokeFunc(time) {
		lastInvokeTime = time;
		timeId = undefined;
		result = func();
		return result;
	}

	function invokeCondition(time) {
		const timeSinceLastCall = time - lastCallTime;
		if (!lastCallTime || timeSinceLastCall >= wait) {
			return "debouncedCondition";
		}

		if (maxing && timeSinceLastCall < wait) {
			return "throttledCondition";
		}

		return false;
	}

	function timerExpired() {
		const time = Date.now();
		isInvoking = invokeCondition(time);

		if (isInvoking === "throttledCondition") {
			if (timeId) {
				return;
			}

			return timeId = setTimeout(function(time) {
				invokeFunc(time);
			}, options.maxWait);
		}

		if (isInvoking === "debouncedCondition") {
			if (timeId) {
				return;
			}

			return invokeFunc(time);
		}

		timeId = setTimeout(timerExpired, wait);
	}

	function debounced() {
		const time = Date.now();
		isInvoking = invokeCondition(time);
		lastCallTime = time;

		if (isInvoking) {
			return leading ? timerExpired : setTimeout(timerExpired, wait);
		}
	}

	return debounced;
}
```
lodash 코드를 참고해서 구현했다.
lodash debounce의 maxWait 옵션과 leading을 사용할 수 있게 만들었다.
`invokeCondition` 으로 invoke 조건을 만족하는지 평가하고 true면 func를 실행하는데 실행하기 전 한 번 더 조건을 평가한다.
그 전에 throttle을 만들었을 때는 flag값으로 조건을 만족하는지 평가했지만 lodash 코드를 참고하여 setTimeout id를 이용해서 조건을 평가했다.

<img src="assets/220516-1.gif" alt="debounce">
<img src="assets/220516.gif" alt="throttle">


```js
function throttle(func, wait) {
	let leading = true;

	return debounceTest(func, wait, {
		leading,
		"maxWait" : wait,
	})
}
```
maxWait, leading 옵션이 있으면 lodash처럼 debounce 함수로 throttle도 간단하게 구현할 수 있다.
