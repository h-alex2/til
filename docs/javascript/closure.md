# 프로토타입 prototype

<!-- 프로토타입 객체들에는 이미 많은 수의 메소드가 정의되어 있습니다.  -->

자바스크립트 엔진은 메모리 힙과 단일 호출 스택 (Call Stack)을 가지고 있습니다. 
하나의 호출 스택만 가지고 있으므로 한번에 단 하나의 함수만 처리할 수 있습니다.  
[비동기적 Javascript – 싱글스레드 기반 JS의 비동기 처리 방법](https://hudi.blog/async-javascript/)  
[Object prototype 이해하기](http://insanehong.kr/post/javascript-prototype/)  
[코드가 어떻게 실행되는지 볼 수 있는 사이트](http://latentflip.com/loupe/)  
[[Javascript ] 프로토타입 이해하기](https://medium.com/@bluesh55/javascript-prototype-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-f8e67c286b67)
[자바스크립트 프로토타입(Prototype)이란? __ 프로토타입링크(Prototype Link)__프로토타입객체(Prototype Object)
](https://okayoon.tistory.com/entry/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%ED%94%84%EB%A1%9C%ED%86%A0%ED%83%80%EC%9E%85Prototype%EC%9D%B4%EB%9E%80-%ED%94%84%EB%A1%9C%ED%86%A0%ED%83%80%EC%9E%85%EB%A7%81%ED%81%ACPrototype-Link%ED%94%84%EB%A1%9C%ED%86%A0%ED%83%80%EC%9E%85%EA%B0%9D%EC%B2%B4Prototype-Object)
자바스크립트는 다른 함수가 실행되고 있을 때는 그 함수가 종료되기 직전까지 다른 작업이 중간에 끼어들 수 없습니다. 이것을 Run-to-completion 이라고 합니다. 하지만 자바스크립트는 자바스크립트 엔진으로만 돌아가는 것이 아니기 때문에 비동기 실행이 가능하게 됩니다. 
자바스크립트 엔진 밖에서 자바스크립트 실행에 관여하는 요소들이 존재합니다. 
1. Web API
브라우저에서 제공되는 API이며, AJAX나 Timeout 등의 비동기 작업을 실행합니다. setTimeout과 같은 함수를 실행하면 자바스크립트 엔진은 Web API에 setTimeout을 요청하고 동시에 setTimeout에 넣어준 Callback 까지 전달합니다. Callstack 에서는 Web API 요청 이후 setTimeout 작업이 완료되어 제거됩니다.  
Web API는 방금 요청받은 setTimeout을 완료하고, 동시에 전달받은 Callback을 Task Queue라는 곳에 넘겨줍니다. 

2. Task Queue
Task Queue는 Callback Queue 라고도 하는데 큐 형태로 Web API에서 넘겨받은 Call back 함수를 저장합니다. 
3. Event Loop 



## 객체지향언어 OOP Object-Oriented Programming 
객체지향언어에는 2가지의 큰 틀이 있습니다. 
1. 클래스 기반 객체지향언어  
클래스 기반의 언어에서는 객체의 형식이 정의된 클래스라는 개념을 가집니다. Class 안에 기술된 내용을 기반으로 인스턴스를 생성하여 객체를 사용합니다. 틀 자체를 상속시키고 상속시킨 틀을 이용해서 객체를 생성합니다. 

2. 프로토타입 기반 객체지향언어  
객체의 원형인 프로토타입을 이용하여 새로운 객체를 만들어내는 프로그래밍 기법입니다. 이렇게 만들어진 객체 역시 자기자신의 프로토타입을 갖습니다. 이 새로운 객체의 원형을 이용하면 또 다른 새로운 객체를 만들어 낼 수도 있으며 이런 구조로 객체를 확장하는 방식을 프로토타입 기반 프로그래밍이라고 합니다. 
  
자바스크립트의 모든 객체는 생성과 동시에 자기자신이 생성될 당시의 정보를 취한 Prototype Object 라는 새로운 객체를 Cloning 하여 만들어냅니다. 프로토타입이 객체를 만들어내기 위한 원형이라면 이 Prototype Object는 자기 자신의 분신이며 자신을 원형으로 만들어질 다른 객체가 참조할 프로토타입이 됩니다. 

E2015 부터 class 키워드를 지원하기 시작했으나, 문법적인 양념일 뿐이며 자바스크립트는 여전히 프로토타입 기반의 언어입니다. (class ) - class, extends 키워드가 나오기는 했지만 내부 구현은 prototype으로 되어있다고 합니다. 결국 틀은 프로토타입 기반입니다. 

## 프로토타입이 그래서 뭐야? 
자기 자신을 생성하기 위해 사용된 객체원형을 프로토 타입이라고 합니다. (새로운 객체가 생성되기 위한 원형이 되는 객체)
즉 같은 원형으로 생성된 객체가 공통으로 참조하는 공간입니다.

- 추가, 수정, 삭제는 함수의 prototype 속성을 통해 접근해야 합니다.  
- 접근은 prototype 속성 없이도 가능합니다. 

만들어진 객체 안에 `__proto__`(비표준) 속성이 자신을 만들어낸 원형을 의미하는 프로토타입 객체를 참조하는 숨겨진 링크가 있습니다. 이 숨겨진 링크를 프로토타입이라고 정의합니다. 

prototype 속성은 함수만 가지고 있던 것과는 달리 `__proto__` 속성은 모든 객체가 빠짐없이 가지고 있는 속성입니다. `__proto__`는 객체가 생성될 때 조상이었던 함수의 Prototype Object를 가리킵니다. 

## Prototype Object와 Prototype Link
__Prototype Object__ 와 __Prototype Link__
이 둘을 통틀어 Prototype이라고 부릅니다.  

## Prototype Object
객체는 언제나 함수로 생성됩니다. 객체를 만드는 방법은 2가지가 있는데 하나는 객체 리터럴사용, 또 하나는 new 생성자를 이용하는 방법입니다. 리터럴사용도 결국에는 함수를 이용하여 만들게됩니다. (Function, Array 모두 마찬가지입니다.)  
함수가 정의될 때는 2가지 일이 동시에 이뤄집니다. 
1. 해당 함수에 Constructor(생성자) 자격 부여
	- Constructor 자격이 부여되면 new를 통해 객체를 만들어 낼 수 있게 됩니다. 이것이 함수만 new 키워드를 사용할 수 있는 이유입니다. 
2. 해당 함수의 Prototype Object 생성 및 연결  
	- 함수를 정의하면 함수만 생성되는 것이 아니라 Prototype Object도 같이 생성이 됩니다. 그리고 생성된 함수는 prototype이라는 속성을 통해 Prototype Object에 접근할 수 있습니다.  


Prototype Object는 일반적인 객체와 같으며 기본적인 속성으로 constructor와 __proto__를 가지고 있습니다. 
(__proto__는 Prototype Link입니다. constructor는 밑에서 다시 설명합니다)

## `__proto__` : Prototype Link
prototype 속성은 함수만 가지고 있던 것과는 달리 __proto__속성은 모든 객체가 빠짐없이 가지고 있는 속성입니다. __proto__는 객체가 생성될 때 조상이었던 함수의 Prototype Object를 가리킵니다. 

```js 
let test = function () {
    this.a = 1;
    this.b = 2;
}
let o = new test();

console.log(o.__proto__); //test 함수의 Prototype Object를 가리킵니다.
```
그렇다면 `o.__proto__._proto__`는 무엇일까요?  
test 함수의 프로토타입 또한 프로토타입을 가지고 있습니다. 함수는 결국 객체이므로 Object로부터 만들어집니다. `o.__proto__.__proto__` 는 `Object.prototype`를 나타냅니다. Object의 프로토타입은 존재하지 않으므로 `o.__proto__._proto__.__proto__`은 `null`을 나타내고 이것은 `Object.prototype.__proto__`와 같습니다. null은 프로토타입 체인의 종점 역할을 합니다. 


__proto__의 사용을 권장하지 않는다고 합니다 이유는 밑 링크 확인
<https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/proto>



## Constructor는 또 뭔데? 
개발자가 특별히 할당하지 않더라도 모든 함수는 기본적으로 prototype 프로퍼티를 갖습니다. 디폴트 프로퍼티 prototype은 constructor프로퍼티 하나만 있는 객체를 가리키는데 여기서 constructor 프로퍼티는 함수 자신을 가리킵니다.

그리고 모든 객체(Object.create(null)로 만든 객체 제외)에는 constructor 속성이 있습니다. 

```js
let o = {}
o.constructor === Object // true

let o = new Object
o.constructor === Object // true

let a = []
a.constructor === Array // true

let a = new Array
a.constructor === Array // true

let n = new Number(3)
n.constructor === Number // true
```


```js
function Rabbit() {}

/* 디폴트 prototype
Rabbit.prototype = { constructor: Rabbit };
*/
```

```js
function Rabbit() {}
// 디폴트 prototype:
// Rabbit.prototype = { constructor: Rabbit }

let rabbit = new Rabbit(); // {constructor: Rabbit}을 상속받음

```
라고 할 때 rabbit.constructor 는 `Rabbit()`을 가리킵니다. 

모든 함수 객체의 Constructor는 prototype이란 프로퍼티를 가지고 있습니다. 이 prototype 프로퍼티는 객체가 생성될 당시 만들어지는 객체 자신의 원형이 될 prototype 객체를 가리킵니다. 
<https://ko.javascript.info/function-prototype>

constructor 프로퍼티는 기존에 있던 객체의 constructor를 사용해 새로운 객체를 만들 때 사용할 수 있습니다. 

```js
function User(name) {
  this.name = name;
}

let user = new User('John');
let user2 = new user.constructor('Pete');

alert( user2.name ); // Pete (잘 동작하네요!)
```

## 프로토타입 체인은 또 뭐야 ?? 
상위 프로토타입 객체로부터 메소드와 속성을 상속 받을 수도 있고 그 상위 프로토타입 객체도 마찬가지입니다. 이를 프로토타입 체인이라 부릅니다. 상속이라고 하지만 상속받은 프로퍼티가 상속받은 객체에 존재하는 것은 아닙니다. 엄밀히 말한다면 공유받은 것이라고 할 수 있겠습니다. 

정확히 말하자면 상속되는 속성과 메소드들은 각 객체가 아니라 객체의 생성자의 prototype이라는 속성에 정의되어 있습니다.

<https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Functions/Arrow_functions>
신기하네 화살표 함수는 prototype이 없다. 



클로저란? 
함수가 함수를 반환할 때, 반환되는 함수는 자신을 둘러싼 메모리 환경을 가지고 반환된다. 
외부 변수를 기억하고 이 외부 변수에 접근할 수 있는 함수를 의미합니다.


렉시컬 스코프 : 정적 스코프 
엔진이 function 키워드를 만나면 오브젝트를 생성하게 되는데 이 때 스코프가 결정됩니다. 이것을 렉시컬 스코프라고 합니다. 

Execution context 실행 콘텍스트
우리가 함수를 호출 할 때 새로운 execution context가 생성되어, execution stack의 맨 위로 올려집니다. 
