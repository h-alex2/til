# 박스 모델
출처 : <https://poiemaweb.com/css3-box-model#4-box-sizing-%ED%94%84%EB%A1%9C%ED%8D%BC%ED%8B%B0>  
<br>
모든 html 요소는 Box 형태의 영역을 가지고 있다. (Box 형태란 사각형을 의미)
<img src="https://drafts.csswg.org/css-box-3/images/box.png">  
출처 : <https://drafts.csswg.org/css-box-3/#intro>

브라우저는 박스 모델의 크기(dimension)와 프로퍼티(색, 배경, 모양 등), 위치를 근거로 하여 렌더링을 실행한다.

## width / height 프로퍼티
width와 height 프로퍼티는 요소의 너비와 높이를 지정하기 위해 사용된다. 이때 지정되는 요소의 너비와 높이는 콘텐츠 영역을 대상으로 한다.   
기본적으로 width와 height 프로퍼티는 콘텐츠 영역을 대상으로 요소의 너비와 높이를 지정하므로 박스 전체 크기는 다음과 같이 계산할 수 있다.

| 전체 너비|
|--|
|width + left padding + right padding + left border + right border + left margin + right margin|

|전체 높이|
|--|
|height + top padding + bottom padding + top border + bottom border + top margin + bottom margin|

width와 height 프로퍼티의 초기값은 auto : 이것은 브라우저가 상황에 따라 적당한 width와 height 값을 계산할 것을 의미한다.  

width와 height 프로퍼티를 비롯한 모든 박스모델 관련 프로퍼티(margin, padding, border, box-sizing 등)는 상속되지 않는다. 

## margin / padding 프로퍼티 
margin 프로퍼티에 auto 키워드를 설정하면 해당 요소를 브라우저 중앙에 위치 시킬 수 있다.  
요소 너비가 브라우저 너비보다 크면 가로 스크롤바가 만들어진다. 이 문제를 해결하기 위해서 max-width 프로퍼티를 사용할 수 있다. max-width 프로퍼티를 사용하면 브라우저 너비가 요소의 너비보다 좁아질 때 자동으로 요소의 너비가 줄어든다.  
- max-width: 300px; 
  - 브라우저 너비가 300px보다 작아지면 요소 너비는 브라우저의 너비에 따라서 작아진다. 
- min-width: 300px;
  - 브라우저 너비가 300px보다 작아져도 요소 너비는 지정 너비 300px를 유지한다. 

## border 프로퍼티
- border-style : 테두리 선의 스타일을 지정한다.
- border-width : 테두리의 두께를 지정한다. (border-width, border-color 프로퍼티는 border-style과 함께 사용하지 않으면 적용되지 않는다.)
- border-radius : 테두리 모서리를 둥글게 표현하도록 지정한다.  
- border : border-width, border-style, border-color를 한번에 설정하기 위한 shorthand 프로퍼티이다. 

## box-sizing 프로퍼티
width, height 프로퍼티의 대상 영역을 변경할 수 있다. 
- box-sizing 프로퍼티의 기본값은 content-box이다. 이는 width, height 프로퍼티의 대상 영역이 content 영역을 의미한다. box-sizing 프로퍼티의 값을 border-box로 지정하면 마진을 제외한 박스 모델 전체를 width, height 프로퍼티의 대상 영역으로 지정할 수 있어서 CSS Layout을 직관적으로 사용할 수 있게 한다. 

|키워드|설명|
|--|--|
|content-box|width, height 프로퍼티 값은 content 영역을 의미한다.|
|border-box|width, height 프로퍼티 값은 content 영역, padding, border가 포함된 값을 의미한다.|

- box-sizing 프로퍼티는 상속되지 않는다. 