# 팩토리 함수와 생성자 함수의 차이



## 1. 생성자 함수 constructor function 
- 여러 개의 동일한 프로퍼티를 가지는 객체를 생성하기 위해서 필요합니다.
- 일반 함수와 기술적인 차이는 없지만 생성자 함수는 아래 두 관례를 따릅니다. 
  1. 관례상 함수 이름의 첫 글자는 대문자로 시작합니다.
  2. 반드시 'new' 연산자를 붙여 실행합니다.

- 생성자의 의의는 재사용할 수 있는 객체 생성 코드를 구현하는 것입니다.
- return을 쓰지 않아도 this가 return 됩니다.
- 생성자 함수에는 보통 return문이 없습니다. (return문이 있는 생성자 함수는 거의 없다고 합니다.)
- 반환해야 할 것들은 모두 this에 저장되고, this는 자동으로 반환되기 때문에 반환문을 명시적으로 써 줄 필요가 없습니다.
- 생성자 함수가 반환해주는 빈 객체는 흔히 Instance 라고 부릅니다. 


### `new`가 하는 일 
1. 새로운 instance object를 인스턴스화 하고 constructor안의 this에 연결합니다.
2. Constructor.prototype에 instance.__proto__를 바인딩합니다.
3. 2의 부작용으로 Constructor에 instance.__proto_.constructor를 바인딩합니다.
4. 암시적으로 instance를 참조하는 this를 반환합니다. 

### Benefits of Constructors & `class`
- 추가 예정

### Drawbacks of Constructors & `class`
1. instance화의 세부사항이 호출 API로 누출됩니다. 
2. 생성자는 개방형/폐쇄형 원칙을 깨뜨립니다.


## 2. 팩토리 함수 Factory function
- 생성자 함수나 class가 아닌 경우를 팩토리 함수라고 합니다.
- 임의의 객체를 반환하고 임의의 프로토타입을 사용합니다.

### Benefits of Using Factories
- 리팩토링 걱정 없다.
  - 팩토리에서 생성자로 변환할 필요가 전혀 없으므로 리팩토링이 문제되지 않습니다.
- this가 표준으로 동작합니다.
  - 이를 사용하여 상위 개체에 액세스할 수 있습니다.
- `instanceof`가 없습니다.
- 캡슐화, data hiding을 얻을 수 있습니다.

### Drawbacks of Factories
- Factory.prototype의 인스턴스에 대한 링크를 만들 수 없습니다.
- this가 팩토리 함수 안의 새로운 object를 가리키지 않습니다.
- 동일한 객체를 만들 때 메모리 비용이 많이 필요합니다.


> 출처
> - [new 연산자와 생성자 함수](https://ko.javascript.info/constructor-new)
> - [JavaScript 팩토리 함수 vs 생성자 함수 vs 클래스](https://medium.com/javascript-scene/javascript-factory-functions-vs-constructor-functions-vs-classes-2f22ceddf33e)
> - [Constructor function vs Factory functions](https://stackoverflow.com/questions/8698726/constructor-function-vs-factory-functions)
