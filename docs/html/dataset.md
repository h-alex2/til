# HTMLElement.dataset
HTML 요소에 정의한 사용자 정의 어트리뷰트와 자바스크립트 간에 데이터를 교환할 수 있습니다.  
data 어트리뷰트는 data-user-id, data-role과 같이 data- 접두사 다음에 임의의 이름을 붙여 사용하고 data 어트리뷰트의 값은 HTMLElement.dataset 프로퍼티로 취득할 수 있습니다.  
dataset 프로퍼티는 HTML 요소의 모든 data 어트리뷰트의 정보를 제공하는 DOMStringMap 객체를 반환합니다.  
DOMStringMap 객체는 data어트리뷰트의 data- 접두사 다음에 붙인 임의의 이름을 카멜 케이스로 변환한 프로퍼티를 가지고 있습니다.  
이 프로퍼티로 data 어트리뷰트의 값을 취득하거나 변경할 수 있습니다.  


## data-*
사용자 지정 데이터 특성(custom data attributes)이라는 특성 클래스를 형성함으로써 임의의 데이터를 스크립트로 HTML과 DOM 사이에서 교환할 수 있는 방법을 제공합니다.  

HTMLElement 인터페이스의 읽기 전용 속성인 dataset은 element에 대한 사용자 정의 어트리뷰트에 대한 읽기/쓰기 액세스를 제공합니다.  
각 `data-*` 속성에 대한 항목이 있는 문자열 맵(DOMStringMap)을 노출합니다.  

## DOMStringMap_
DOMStringMap 인터페이스는 엘리먼트에 추가된 사용자 정의 속성에 대한 데이터를 나타내기 위해 HTMLElement.dataset / SVGElement.dataset 속성에 사용됩니다.

## In HTML
HTML에서 속성이름은 data-로 시작합니다. 문자, 숫자, 대시(-), 마침표(.), 콜론(:) 및 밑줄(_)만 포함할 수 있습니다. 모든 ASCII 대문자(A to Z)는 소문자로 변경됩니다.

## In JavaScript
JavaScript에서 사용자 정의 속성 이름은 data- 접두사가 없는 HTML 속성과 동일하고 대시(-)들은 카멜케이스로 변환됩니다.



> 참고 출처
> 1. [Global attributes](https://developer.mozilla.org/ko/docs/Web/HTML/Global_attributes)
> 2. [DOMStringMap](https://developer.mozilla.org/en-US/docs/Web/API/DOMStringMap)
> 3. [데이터 속성 사용하기](https://developer.mozilla.org/ko/docs/Learn/HTML/Howto/Use_data_attributes)
> 4. [HTMLElement.dataset](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/dataset)