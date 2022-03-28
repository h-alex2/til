# Array.prototype.join()

join() 메서드는 배열의 모든 요소를 연결해 하나의 문자열로 만듭니다.

```js
const elements = ['Fire', 'Air', 'Water'];

console.log(elements.join());
// expected output: "Fire,Air,Water"

console.log(elements.join(''));
// expected output: "FireAirWater"

console.log(elements.join('-'));
// expected output: "Fire-Air-Water"

var a = ['바람', '비', '불'];
var myVar1 = a.join();      // myVar1에 '바람,비,불'을 대입
var myVar2 = a.join(', ');  // myVar2에 '바람, 비, 불'을 대입
var myVar3 = a.join(' + '); // myVar3에 '바람 + 비 + 불'을 대입
var myVar4 = a.join('');    // myVar4에 '바람비불'을 대입
```
모든 배열 요소가 문자열로 변환된 다음 하나의 문자열로 연결됩니다.

(출처 : <https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/join>)

