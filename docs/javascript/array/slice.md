# Array.prototype.slice()
> slice() 메서드는 어떤 배열의 begin부터 end까지(end 미포함)에 대한 얕은 복사본을 새로운 배열 객체로 반환합니다. 원본 배열은 바뀌지 않습니다.

## 구문
`arr.slice([begin[, end]])`
- begin
  - 음수 인덱스 : 배열의 끝에서부터의 길이
  - begin이 undefined인 경우 : 0번 인덱스부터 slice
  - begin이 배열의 길이보다 큰 경우에는, 빈 배열을 반환합니다. 

- end
  - end 인덱스를 제외하고 추출
  - end 인덱스가 생략되면 배열의 끝까지(arr.length) 추출합니다. 
  - 만약 end 값이 배열의 길이보다 크다면, slice()는 배열의 끝까지(arr.length) 추출합니다. 


## slice() 는 원본을 대체하지 않는다. 얕은 복사만 할 뿐이다.