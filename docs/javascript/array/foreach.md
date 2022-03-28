# Array.prototype.forEach()
forEach() 메서드는 주어진 함수를 배열 요소 각각에 대해 실행합니다.

for 반복문을 forEach()로 바꾸기
```js
const items = ['item1', 'item2', 'item3'];
const copy = [];

// 이전
for (let i=0; i<items.length; i++) {
  copy.push(items[i]);
}

// 이후
items.forEach(function(item){
  copy.push(item);
});
```

(출처 : <https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach>)