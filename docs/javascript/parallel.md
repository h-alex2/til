# promise, async, await 쓰지 않고 parallel 만들기

## for문에서 var와 블록스코프의 차이
for문에서 setTimeout을 이용해서 차이를 알아보자.

### 1. var 사용
<img src="assets/220502_02.gif">  
Scope를 보면 오른쪽 Scope의 부분에 i의 스코프가 생기지 않는 걸 확인할 수 있다. var는 function 스코프기 때문에 for 문안에서 스코프가 생기지 않는다. 그래서 결과는 마지막 i의 값을 4번 setTimeout 하기 5가 4번 출력된다. var로 사용했을 때 let과 같은 결과를 내고 싶다면 즉시 실행 함수를 사용해서 함수 스코프를 만들어주면 된다. (하지만 var를 쓸 일이 있을까?)

### 2. let 사용
<img src="assets/220502_03.gif">  
let으로 선언해 주면 Block이라고 되어있는 스코프가 생기는 걸 확인할 수 있다.
그래서 setTimeout의 결과도 0, 1, 2, 3 이 된다.

### for문 안의 setTimeout으로 변수에 값을 할당할 수 있을까? 
<img src="assets/220502_04.gif">  

답은 네니오다.  
왜냐하면 할당이 되기 때문이다. 말이 이상한데 할당이 되지만 setTimeout 같은 WebAPI는 콜 스택에 올라가자마자 JS 내부에서 처리되는 것이 아니라 타이머 기능이 끝난 후 event loop에서 다시 콜 사택에 들어가면서 처리된다. event loop에서 callstack으로 보내기 위해서는 callstack이 모두 비워져있어야 한다. 어떠한 작업을 해주지 않는다면 함수 내부에서 할당된 변수의 값을 쓸 수 없다는 얘기가 된다. 변수가 할당되는 시점에 이미 함수인에서 for 문.. 함수.. 등등 webAPI가 쓰이지 않는 모든 것이 끝난 이후에 setTimeout이 실행되기 때문에 불러올 수 없게 된다.

위 이미지를 보게 되면 test라는 빈 배열을 만들고 for 문의 setTimeout으로 test 안에 i의 값을 push 해주었다.
if를 이용해 test의 length가 2가 되면 console을 출력하게 했지만 출력되지 않는다. 하지만 test에는 i의 값이 들어가 있다. 변수의 값을 할당된 "후"에 따로 불러와서 사용해야 할 때면 괜찮겠지만 함수 내에서 변수를 바로 다뤄야 하는 경우라면 문제가 있다.

```js
const testA = [];

function a() {
    for (let i = 0; i < 5; i++) {
        setTimeout(() => testA.push(i), 2000);
    }
}

function b() {
    console.log(testA[2])
}

console.log(a(), b()); // undefined undefined;
```
### 어떻게 해야 값을 할당한 변수를 바로 쓸 수 있을까? : 콜백 함수
과제를 진행하면서 많은 고난이 있었다.. 이것을 어떻게 해야 값을 받아올 수 있을까 계속 고민해봤는데 <https://poiemaweb.com/es6-promise> 을 보고 힌트를 얻었다.
"즉, 비동기 함수의 처리 결과에 대한 처리는 비동기 함수의 콜백 함수 내에서 처리해야 한다." 이 말에 답이 있었다. setTimeout안의 함수도 콜백함수니까.. 😮아하!

```js
const testA = [];
function a() {
    for (let i = 0; i < 5; i++) {
        setTimeout(() => {
            testA.push(i);
            console.log(testA)
    }, 2000);
}
}
undefined
a();
undefined // 2초 뒤
//  [0]
//  (2) [0, 1]
//  (3) [0, 1, 2]
//  (4) [0, 1, 2, 3]
//  (5) [0, 1, 2, 3, 4]
```
즉 setTimeout으로 값을 넣어준 testA 배열은 그 setTimeout 안에서 쓸 수 있다는 말이다.  
여기서 JS가 비동기를 처리할 때 왜 콜백 함수를 썼는지 알 수 있었다. 사실 과제를 할 때는 풀기에만 급급해서 제대로 이해하지 못했는데 이제야 이해가 된다.
비동기 처리를 통해 받은 값을 처리하기 위해서 또 콜백 함수를 넣고 넣고 하다 보니 콜백 헬 이 나오게 되는 거구나.


```js

function parallel(tasks, finalCallback) {
	var count = 0;
  var results = [];

  if (!tasks.length) {
    return finalCallback();
  }

  for (let i = 0; i < tasks.length; i++) {
		tasks[i]((result) => {
			results[count] = result;
      console.log("i: ", i, " count: ", count, " value: ", result, " results: ", results)
      count++;
      if (results.length === tasks.length) {
				finalCallback(results);
			}
		})
  }

  const order = [];

parallel(
	[
		function (callback) {
			setTimeout(function () {
				order.push(1);
				callback(1);
			}, 150);
		},
		function (callback) {
			setTimeout(function () {
				order.push(2);
				callback("2");
			}, 300);
		},
		function (callback) {
			setTimeout(function () {
				order.push(3);
				callback({ data: 3 });
			}, 10);
		},
	],
	function (results) {
		console.log("order: ", order); // order의 순서는 [3, 1, 2]
		console.log("finalCallback: ", results) //results의 순서는 [1, "2", { data: 3 }]
	}
)
```
과제 코드 중 하나인데 parallel을 실행하게되면 
```
i:  3  count:  0  value:  {data: 3}  results:  [{…}]
i:  3  count:  1  value:  1  results:  (2) [{…}, 1]
i:  3  count:  2  value:  2  results:  (3) [{…}, 1, '2']
order:  (3) [3, 1, 2]
finalCallback:  (3) [{…}, 1, '2']
```
콘솔에 이렇게 출력된다.  
parallel 함수 안 for 문에서의 i는 var로 선언됐기 때문에 마지막 값인 3을 가지게 된다. 결국에는 i의 값은 tasks의 함수를 실행시킬 때 빼고는 쓸 수 없게 된다.  
그래서 var count 변수를 만들어 실행되는 순서대로 results 배열에 값을 넣어주었다.  

문제 조건인 results 배열 안의 순서는 tasks의 배열 순서와 같아야 한다. 하지만 tasks 안의 함수는 setTimeout으로 인해 실행 순서가 3, 1, 2를 가지게 된다.  
이 문제를 해결하기 위해서는 index 값을 사용해야 하는데 for 문안에서 var를 사용하게 되면 for 문안에서만 해결하는 것은 즉시실행함수 말고는 불가능하다. (다른 방법이 있으려나... 있을 수도.. 내가 모를 수도...)

count의 값도 결국에는 3, 1, 2의 순서대로 실행된다. 그럼 어떻게 해야할까? 결국에는 for문에서는 i값을 함수에 전달하는 역할만 하고 for문 바깥에 함수를 만들어 i값을 전달 받아야한다.

```js
function parallel(tasks, finalCallback) {
  var count = 0;
  var results = [];

  if (!tasks.length) {
    return finalCallback();
  }

  for (var i = 0; i < tasks.length; i++) {
		asyncHandler(i);
  }

	function asyncHandler(i) {
		tasks[i]((result) => {
      count++;
			results[i] = result;
      if (count === tasks.length) {
				finalCallback(results);
			}
		})
	}
}
```
이런식으로 진행되어야 한다. (코드의 인덴팅이 거지같이 나오는데 저렇게 입력하지 않았다..왜 저렇게 삐뚤빼뚤하게 나오는것이지)  

또는 즉시실행함수를 사용하기

```js
function parallel(tasks, finalCallback) {
  var count = 0;
  var results = [];

  if (!tasks.length) {
    return finalCallback();
  }

  for (var i = 0; i < tasks.length; i++) {
		(function (i) {
			tasks[i]((result) => {
				count++;
				results[i] = result;
				if (count === tasks.length) {
					finalCallback(results);
				}
			})
		})(i)
  }
}
```