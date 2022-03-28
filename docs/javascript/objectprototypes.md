# Object prototypes
(출처 : <https://developer.mozilla.org/ko/docs/Learn/JavaScript/Objects/Object_prototypes#%ED%94%84%EB%A1%9C%ED%86%A0%ED%83%80%EC%9E%85_%EC%86%8D%EC%84%B1_%EC%83%81%EC%86%8D_%EB%B0%9B%EC%9D%80_%EB%A9%A4%EB%B2%84%EB%93%A4%EC%9D%B4_%EC%A0%95%EC%9D%98%EB%90%9C_%EA%B3%B3>)

중요한 점은 prototype에 새 메소드를 추가하는 순간 동일한 생성자로 생성된 모든 객체에서 추가된 메소드를 바로 사용할 수 있다는 점입니다.

## 예제 
```js
  it("should use prototype to add to all objects", function () {
      function Circle(radius)
      {
        this.radius = radius;
      }

      var simpleCircle = new Circle(10);
      var colouredCircle = new Circle(5);
      colouredCircle.colour = "red";

      expect(simpleCircle.colour).toBe(undefined);
      expect(colouredCircle.colour).toBe("red");

      Circle.prototype.describe = function () {
        return "This circle has a radius of: " + this.radius;
      };

      expect(simpleCircle.describe()).toBe("This circle has a radius of: 10");
      expect(colouredCircle.describe()).toBe("This circle has a radius of: 5");
  });
```

`Circle.prototype.describe = function ()`로 prototype에 메소드를 추가하면 동일한 생성자로 생성된 모든 객체에서 추가된 메소드를 바로 이용할 수 있다.  

: `simpleCircle.describe())`
