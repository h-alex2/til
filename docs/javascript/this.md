# this
- this 키워드는 항상 함수 내부에서 사용됩니다.
- this는 거의 대부분의 상황에서 __객체__ 입니다.
- this 값을 판별하기 위해서는 반드시 함수의 __실행문__ 을 찾아야 합니다.
- this의 값은 this를 사용하는 해당 함수를 __어떻게__ 실행하느냐에 따라 바뀝니다.
- 함수가 실행될 수 있는 방식에는 크게 4가지가 있습니다. 이것은 this의 값 또한 4가지 경우의 수가 있다는 의미합니다.

## 1. Regular Function Call 일반 함수 실행 방식
- this : Global Object (브라우저에서는 window 객체)
- __Strict Mode__ -> 글로벌이 아닌 undefined
- this가 만약에 window라면 보통 그건 오류라고 봅니다. 일반적으로 그렇게 쓰이는 경우는 잘 없기 때문입니다.

```js
function foo () {
	console.log(this); // this === global object
}

foo();
```

## 2. ot Notation (Object Method Call)

```js
var hyun = {
	age : 15,
	foo: function () {
		console.log(this.age);
	}
};

var func = hyun.foo;

// Dot Notation
hyun.foo();

// Regular Funtion Call
func();
```

`hyun.foo();` 의 경우에 this 는 . 앞에 있는 `hyun`입니다.

```js
var age = 30;

const person1 = {
	age: 20, 
	logAge: function () {
		console.log(this.age);
	}
};

const people = [ person1 ];

const employees = {
	list: people
};

employees.list[0].logAge();
```

``` js
function makePerson (name, age) {
	return {
		name: name, 
		age: age, 
		verifyAge: () => {
			return this.age > 21;
		}
	};
}

const ken = makePerson("ken", 30);

if (ken.verifyAge()){
	alert("Yes, Beer!");
} else {
	alert("No, Beer!");
}
```
__화살표 함수__ 에는 this가 없습니다. this를 찾기 위해 가까운 function을 찾고 makePerson이 실행된 곳을 찾아가보면 const ken에서 실행되고 있네요. 일반함수로 실행되고 있으므로 this는 global이 됩니다.

## Call, Apply, Bind  
## 3-1. `Function.prototype.call`

- 주어진 this 값 및 각각 전달된 인수와 함께 함수를 호출합니다. 
- apply()와 거의 동일하지만 call()은 인수 목록을 apply()는 인수 배열 하나를 받는다는 점이 중요한 차이입니다.
- 구문 : `func.call(thisArg[, arg1[, arg2[, ...]]])` 
- thisArc : func 호출에 제공되는 this의 값
- arg1, arg2, ... : 객체를 위한 인수


Explicit Binding : __명백__ 하게 지정하는 것을 의미합니다. Function.prototype.call, Function.prototype.bind, Function.prototype.apply 이 세 가지 함수들은 this 값을 명확하게 할 때 사용하는 방식입니다.

```js
function logAge () {
	console.log(this.age);
}

const person = {
	age: 20
};

logAge.call(person); //this = person 
```
- 여기서 `call`의 역할은
1. logAge 함수의 this를 첫 번째 인자로 받은 person으로 설정 
2. logAge 함수를 실행

```js
function foo (a, b, c) {
	console.log(this.age);
	console.log(a + b + c);
}

var ken = {
	age: 35
};

foo.call(ken, 1, 2, 3);
```
- call의 첫 번째 인자 : this
- 나머지 인자
- 1 : a
- 2 : b
- 3 : c

## 3-2. `function.prototype.apply`
- apply() 메서드는 주어진 this 값과 배열 (또는 유사 배열 객체)로 제공되는 arguments로 함수를 호출합니다.
- apply 메소드는 인자를 배열에 담아줘야한다. 배열 자체를 넘겨주는 것이 아니라 배열안의 요소를 넘겨주는 것입니다. 
- 항상 두 개의 인자만을 받습니다.
- 구문 : `func.apply(thisArg, [argsArray])`
- thisArg : 옵션. func를 호출하는데 제공될 this의 값. 
- argsArray : 옵션. func이 호출되어야 하는 인수를 지정하는 유사 배열 객체


```js
function foo (a, b, c) {
	console.log(this.age);
	console.log(a + b + c);
}

var ken = {
	age: 35
};

foo.apply(ken, [1, 2, 3]);
```


## 3-3. `Bind` 
.bind 메소드는 .call, .apply 와 약간의 차이가 있습니다.
- 함수를 바로 실행시키지 않고 생성하기만 합니다. 
- bind 메소드는 원본 함수를 복제한 "새로운 함수"를 반환 : 실행시키지 X
- 인자 넣는 건 call과 같습니다.
- 받게되는 첫 인자의 value로는 this 키워드를 설정하고, 이어지는 인자들은 바인드된 함수의 인수에 제공됩니다.
- 구문 : `func.bind(thisArg[, arg1[, arg2[, ...]]])`
- thisArg : 바인딩 함수가 대상 함수(target function)의 this에 전달하는 값입니다. 바인딩 함수를 new 연산자로 생성한 경우 무시됩니다. bind를 사용하여 setTimeout 내에 콜백 함수를 만들 때, thisArg로 전달된 원시값은 객체로 변환됩니다. 
- arg1, arg2, ... : 대상 함수의 인수 앞에 사용될 인수

```js
function foo () {
	console.log(this.age);
}

const ken = { age: 33 };

const bar = foo.bind();
const kar = foo.bind(ken);

bar(); //undefined
kar(); //33;
```

## 4. "new" Keyword : Constructor function (생성자 함수)
- new 키워드를 사용한 함수 내부에서 this를 사용할 경우, this의 값은 새로운 빈 객체가 됩니다.
- new 키워드와 함께 사용될 경우, 우리는 해당 함수를 "생성자 함수" 라고 부릅니다.
- new 키워드를 사용하면 기본적으로 return을 안써주더라도 this가 return 됩니다.

```js
function foo () {
	this.name = '바닐라코딩'; 
}

var coco = new foo();
console.log(coco); '바닐라코딩'
```

## 화살표 함수
- 화살표 함수에서 this는 자신을 감싼 정적 범위입니다. 
- 화살표 함수를 call(), bind(), apply()를 사용해 호출할 때 this의 값을 정해주더라도 무시합니다. 


> 출처 
> - [[바닐라코딩] 자바스크립트 "this" 정리 by 바닐라코딩 YouTube](https://www.youtube.com/watch?v=ayyuU0xdbIU&t=137s)
> - [this by MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/this)
> - [Function.prototype.call() by MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Function/call)
> - [Function.prototype.apply() by MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Function/apply)
> - [Function.prototype.bind() by MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Function/bind)