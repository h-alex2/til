# 객체 object
<https://ko.javascript.info/object>

javascript는 8가지 자료형을 가짐  (7 + 1)
- 그 중 7가지 : 원시형 primitive type , 기본 자료형
	- 하나의 데이터만 담을 수 있습니다.
	- 속성과 메서드를 가질 수 없습니다. 
	- 값 그대로 저장, 할당되고 복사된다.
	- 스택에 값을 저장합니다. 
- 나머지 1가지 : 객체형 object type, 객체 자료형
	- 속성과 메서드를 가질 수 있습니다. 
	- 객체는 참조에 의해 저장되고 복사됩니다. (by reference)
		- 변수에는 객체 자체가 아닌 메모리상의 주소인 __참조__ 가 저장됩니다.
	- 스택과 힙을 연결하여 활용합니다. 자신의 크기를 늘릴 수 있기 때문에 속성과 메서드를 가질 수 있습니다.

## JS에서 함수는 일급 객체 
함수는 일급 객체입니다. first-class object

## 메서드 Method
객체 내부의 함수를 메서드라고 합니다.  
다른 프로그래밍 언어에서는 속성과 메소드가 확실하게 구분되지만 자바스크립트에서는 메서드도 속성의 일종이고 함수자료형의 속성일 뿐입니다. (by 윤인성님 강의)

## 객체 안에서 익명함수와 화살표 함수의 this 차이
익명함수 : this 바인딩을 합니다.  
	- 익명함수에서 this를 사용하면 자신을 의미합니다.
화살표함수 : this 바인딩할 객체가 정적으로 결정됩니다. 
	- 화살표함수에서 this를 사용하면 부모의 this를 의미합니다. (by 제로초님) 즉 언제나 상위 스코프의 this를 가리킵니다. 상위 스코프가 전역일 경우 window를 가리킵니다.  

다른 프로그래밍 언어에서는 this를 생략해도 문제없이 값을 출력하지만 JS에서는 this.를 생략하면 객체 내부에 있는 속성이라고 생각하지 않습니다.
```js
let object = {
    빵 : "bread",
    주스 : "juice",
    함수 : function (){
        console.log(빵, 주스);
    }
}
```
`object.함수()` 결과는 레퍼런스 에러가 나게됩니다. 
```js
let object = {
    빵 : "bread",
    주스 : "juice",
    함수 : function (){
        console.log(this.빵, this.주스);
    }
}
```
`object.함수()` 결과 : bread juice

## 빈 객체 만들기
1. 객체 생성자 문법
	- let user = new Object();
2. 객체 리터럴 문법 
	- let user = {};
	- let 변수명 = [] 도 배열 리터럴 방식입니다. 

## property 삭제하기
	`delete obj.prop`

## Property accessors 속성 접근자
1. Dot nation 점표기법 
	- '점'은 key가 __유효한 변수 식별자__ 인 경우에만 사용할 수 있습니다.
	- 변수 식별자에는 공백이 없어야하고 숫자로 시작하지 않아야 하며 $와 _를 제외한 특수문자가 없어야 합니다. 
	- 처음엔 점 표기법을 사용하다가 복잡한 상황이 발생했을 때 대괄호 표기법으로 바꾸는 경우가 많습니다. 

2. square Bracket Notation 대괄호표기법
	- 유효한 변수 식별자가 아니어도 됩니다. 
	- 키에 어떤 문자열이 있던지 상관없이 동작합니다.
	- 변수를 키로 사용할 수 있습니다.
	- 문자열뿐 아니라 모든 표현식의 평가 결과를 프로퍼티 키로 사용할 수 있습니다.

## computed property 계산된 프로퍼티
```js
let fruit = "apple";

let bag = {
  [fruit]: 5, // 변수 fruit에서 프로퍼티 이름을 동적으로 받아 옵니다.
};

alert( bag.apple ); // fruit에 "apple"이 할당되었다면, 5가 출력됩니다.
```

## in 연산자로 프로퍼티 존재 여부 확인하기 
  javascript 객체의 중요한 특징 중 하나는 다른 언어와는 달리, 존재하지 않는 프로퍼티에 접근하려 해도 에러가 발생하지 않고 __undefined__를 반환한다는 것입니다. 
	- 이 특징을 응용하여 프로퍼티 존재 여부를 쉽게 확인할 수 있습니다. 
	- undefined 이용 외에도 `"key" in object` 연산자 in을 사용하여 확인할 수 있습니다. 

```js
let user = { name: "John", age: 30 };

alert( "age" in user ); // user.age가 존재하므로 true가 출력됩니다.
alert( "blabla" in user ); // user.blabla는 존재하지 않기 때문에 false가 출력됩니다.
```
in 왼쪽엔 반드시 __프로퍼티__ 이름이 와야 합니다. 

## for...in 반복문
(상속된 열거 가능한 속성들을 포함하여 객체에서 문자열로 키가 지정된 모든 열거 가능한 속성에 대해 반복합니다. by mdn)
객체의 모든 키를 순회할 수 있습니다. 

```js
let user = {
  name: "John",
  age: 30,
  isAdmin: true
};

for (let key in user) {
  // 키
  alert( key );  // name, age, isAdmin
  // 키에 해당하는 값
  alert( user[key] ); // John, 30, true
}
```

```js
const object = { a: 1, b: 2, c: 3 };

for (const property in object) {
  console.log(`${property}: ${object[property]}`);
}

// expected output:
// "a: 1"
// "b: 2"
// "c: 3"
```
이렇게 key와 프로퍼티를 바꾸는 방법도 가능합니다. 

## 객체 정렬 방식
객체는 __특별한 방식__ 으로 정렬됩니다.  
정수 프로퍼티 (integer property)는 자동으로 정렬되고, 그 외의 프로퍼티는 객체에 추가한 순서 그대로 정렬됩니다. 
```js
let codes = {
  "49": "독일",
  "41": "스위스",
  "44": "영국",
  // ..,
  "1": "미국"
};

for (let code in codes) {
  alert(code); // 1, 41, 44, 49
}
```
'정수 프로퍼티’라는 용어는 변형 없이 정수에서 왔다 갔다 할 수 있는 문자열을 의미합니다.

# 참조에 의한 객체 복사 by reference 
<https://ko.javascript.info/object-copy>

- 원시값의 경우
```js
let message = "Hello!";
let phrase = message;
```
각각의 문자열 "Hello"가 저장됩니다.

- 객체의 경우 
```js
let user = {
  name: "John"
};
```
객체는 메모리 내 어딘가에 저장되고, 변수 user에는 객체를 __참조__ 할 수 있는 값이 저장됩니다. 따라서 객체를 복사할 땐 참조값을 복사하는 것이고 객체는 복사되지 않습니다.  

## new연산자와 생성자 함수
	- 생성자 함수(constructor function)와 일반 함수에 기술적인 차이는 없습니다. 
	- new연산자와 생성자 함수를 사용하면 유사한 객체 여러 개를 쉽게 만들 수 있습니다. 
	- 유사한 객체를 여러 개 만들 때 유용합니다.
	- 아래 두 관례를 따릅니다. 

1. 함수 이름의 첫 글자는 대문자로 시작합니다. 
2. 반드시 __"new"__ 연산자를 붙여 실행합니다. 

```js
function User(name) {
  this.name = name;
  this.isAdmin = false;
}

let user = new User("보라");

alert(user.name); // 보라
alert(user.isAdmin); // false
```

new User(...)를 써서 함수를 실행하면 아래와 같은 알고리즘이 동작합니다.
1. 빈 객체를 만들어 this에 할당합니다.
2. 함수 본문을 실행합니다. this에 새로운 프로퍼티를 추가해 this를 수정합니다.
3. this를 반환합니다. : return this (this가 암시적으로 반환됨)
아래 코드를 입력한 것과 동일하게 동작합니다. 
```js
let user = {
  name: "보라",
  isAdmin: false
};
```