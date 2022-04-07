## for...in, for...of, enumerable, iterator 
# for..in문 
- 해당 객체의 열거할 수 있는 모든 프로퍼티(Enumerable properties)를 순회할 수 있도록 해줍니다. (Symbol로 키가 지정된 속성은 무시)(prototype 속성까지 모두 순회하기 때문에 의도치 않은 동작을 야기할 수 있다.)
- 객체 자체의 모든 열거 가능한 속성들과 프로토타입 체인으로부터 상속받은 속성들에 대해 반복합니다.
- Enumerable properties란? 내부 열거 형 플래그가 __true__ 로 설정된 property를 말합니다.
- 특정 순서에 따라 인덱스를 반환하는 것을 보장할 수 없습니다.
- hasOwnProperty()를 함께 사용하는 것이 추천되는 것 같아보임... (상속된 속성을 표시하지 않으니까)

## hasOwnProperty() 
- 객체가 특정 프로퍼티를 가지고 있는지를 나타내는 불리언 값을 반환합니다.
- in 연산과는 다르게, 이 메소드는 객체의 프로토타입 체인을 확인하지 않기 때문에 상속된 속성을 표시하지 않습니다.

## enumerable이란? 
- 열거자라는 의미로 enumerable의 속성이 true라면 열거 가능하다는 의미입니다. 

### enumerable 플래그 확인하기
- `Object.getOwnPropertyDescriptor` 메서드를 사용하면 특정 프로퍼티에 대한 정보를 모두 얻을 수 있습니다.
- `let descriptor = Object.getOwnPropertyDescriptor(obj, propertyName);`

```js
let user = {
  name: "John"
};

let descriptor = Object.getOwnPropertyDescriptor(user, 'name');

alert( JSON.stringify(descriptor, null, 2 ) );
/* property descriptor:
{
  "value": "John",
  "writable": true,
  "enumerable": true,
  "configurable": true
}
*/
```
- `writable` : true이면 값을 수정할 수 있습니다. 
- `configurable` : true이면 프로퍼티 삭제나 플래그 수정이 가능합니다. 
- 프로퍼티 플래그는 특별한 경우가 아니고선 다룰 일이 없기 때문에 "평범한 방식"으로 프로퍼티를 만들면 해당 프로퍼티의 플래그는 모두 true가 됩니다.

### 플래그 수정하기 
- `Object.defineProperty` 메서드를 사용하면 플래그를 변경할 수 있습니다.
- `Object.defineProperty(obj, propertyName, descriptor)`
- 이 메서드는 객체에 해당 프로퍼티가 있으면 플래그를 원하는 대로 변경해줍니다. 프로퍼티가 없으면 인수로 넘겨받은 정보를 이용해 새로운 프로퍼티를 만듭니다. 이때 플래그 정보가 없으면 플래그 값은 자동으로 __false__ 가 됩니다. 
- 맞습니다. 이 방식은 앞서 말했던 평범한 방식과는 다릅니다. `defineProperty`를 사용해 프로퍼티를 만든 경우, descriptor에 플래그값을 명시하지 않으면 플래그 값이 자동으로 __false__ 가 됩니다. 

```js
let user = {
  name: "John",
  toString() {
    return this.name;
  }
};

Object.defineProperty(user, "toString", {
  enumerable: false
});

// 이제 for...in을 사용해 toString을 열거할 수 없게 되었습니다.
for (let key in user) alert(key); // name
```


# for...of문 
- 반복할 수 있는 객체(iterable objects)를 순회할 수 있도록 해주는 반복문입니다.
- (순회할 수 있는 객체는 iterator 프로토콜을 따른다.)
- 순서가 보장됩니다.
- 내부적으로 이터레이터의 next 메소드를 호출하여 이터러블을 순회하며 next 메소드가 반환한 이터레이터 result 객체의 value 프로퍼티 값을 for...of 문의 변수에 할당합니다. 그리고 이터레이터 result 객체의 done 프로퍼티 값이 false이면 이터러블의 순회를 계속하고 true이면 이터러블의 순회를 중단합니다.

## iterator란?
- ECMAScript 2015 (ES6)에는 새로운 문법이나 built-in 뿐만이 아니라, protocols(표현법들)도 추가되었습니다. iteration protocol이 그 중 하나입니다. 
- Iteration protocols에는 2개의 protocol이 있습니다. 1. iterable protocol 2. iterator protocol

### The Iteration protocol 
- 데이터 컬렉션을 순회하기 위한 프로토콜(미리 약속된 규칙)입니다. _이터레이션 프로토콜을 준수한 객체는 for...of 문으로 순회할 수 있고 Spread 문법의 피연산자가 될 수 있다._
- 이터레이션 프로토콜에는 iterable protocol과 iterator protocol이 있습니다. 

### 1. Iterable 이터러블
- 이터러블 프로토콜을 준수한 객체를 이터러블이라 합니다. 이터러블은 Symbol.iterator 메소드를 구현하거나 프로토타입 체인에 의해 상속한 객체를 말합니다.
- 이터러블은 for…of 문에서 순회할 수 있으며 Spread 문법의 대상으로 사용할 수 있습니다.
- 일반 객체는 Symbol.iterator 메소드를 소유하지 않습니다. 

```js
const obj = { a: 1, b: 2 };

// 일반 객체는 Symbol.iterator 메소드를 소유하지 않는다.
// 따라서 일반 객체는 이터러블 프로토콜을 준수한 이터러블이 아니다.
console.log(Symbol.iterator in obj); // false

// 이터러블이 아닌 일반 객체는 for...of 문에서 순회할 수 없다.
// TypeError: obj is not iterable
for (const p of obj) {
  console.log(p);
}
```

### 2. Iterator 이터레이터 
- 이터레이터 프로토콜은 next 메소드를 소유하며 next 메소드를 호출하면 이터러블을 순회하며 value, done 프로퍼티를 갖는 이터레이터 result 객체를 반환하는 것입니다. 이 규약을 준수한 객체가 이터레이터입니다. 
- 이터러블 프로토콜을 준수한 이터레이터는 next 메소드를 갖습니다. 

```js
// 배열은 이터러블 프로토콜을 준수한 이터러블이다.
const array = [1, 2, 3];

// Symbol.iterator 메소드는 이터레이터를 반환한다.
const iterator = array[Symbol.iterator]();

// 이터레이터 프로토콜을 준수한 이터레이터는 next 메소드를 갖는다.
console.log('next' in iterator); // true

// 이터레이터의 next 메소드를 호출하면 value, done 프로퍼티를 갖는 이터레이터 리절트 객체를 반환한다.
let iteratorResult = iterator.next();
console.log(iteratorResult); // {value: 1, done: false}
```

### 내장 iterables
- `String`, `Array`, `TypedArray`, `Map`, `Set` 는 모두 내장 iterable입니다. 이 객체들의 프로토타입 객체들은 모두 iteratoe 메소드를 가지고 있기 때문입니다.

### Iterable과 함께 사용되는 문법
- `for-of loops`, `spread operator`, `yield`, `destructuring assignment`는 iterable과 함께 사용되는 구문(statements)과 표현(expressions)입니다.




##  for..in과 for...of 의 차이
- for...in 루프는 객체의 모든 열거가능한 속성에 대해 반복합니다.
- for...of 구문은 __컬렉션__ 전용입니다. 모든 객체보다는, `[Symbol.iterator]` 속성이 있는 모든 컬렉션 요소에 대해 이 방식으로 반복합니다.

```js
Object.prototype.objCustom = function () {};
Array.prototype.arrCustom = function () {};

let iterable = [3, 5, 7];
iterable.foo = "hello";

for (let i in iterable) {
  console.log(i); // logs 0, 1, 2, "foo", "arrCustom", "objCustom"
}

for (let i of iterable) {
  console.log(i); // logs 3, 5, 7
}
```


>출처
>  - [for...in by MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/for...in)
>  - [Object.prototype.hasOwnProperty() by MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/hasOwnProperty)
>  - [for...of by MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/for...of)
>  - [Iteration protocols by MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Iteration_protocols)
>  - [반복기 및 생성기 by MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Iterators_and_Generators)
>  - [JS Enumerable(열거자) or Nonenumerable(비 열거자) ](http://mohwa.github.io/blog/javascript/2015/10/09/enumerable-inJS/)
>  - [프로퍼티 플래그와 설명자 by 모던 JavaScript 튜토리얼](https://ko.javascript.info/property-descriptors)
>  - [Object.defineProperty() by MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty)
>  - [이터레이션과 for...of 문 by poiemaweb](https://poiemaweb.com/es6-iteration-for-of)