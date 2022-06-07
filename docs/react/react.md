## React 공식문서
## JSX
React는 별도의 파일에 마크업과 로직을 넣어 기술을 인위적으로 분리하지 않고 __둘 다 포함하는__ "컴포넌트"를 사용한다.
JS 코드 안에서 UI관련 작업을 할 때 시각적으로 더 도움이 될 수 있다.

- JavaScript를 확장한 문법
- JavaScript의 모든 기능이 포함되어 있다.
- React 엘리먼트를 생성한다.

```js
function formatName(user) {
  return user.firstName + ' ' + user.lastName;
}

const user = {
  firstName: 'Harper',
  lastName: 'Perez'
};

const element = (
  <h1>
    Hello, {formatName(user)}!
  </h1>
);

ReactDOM.render(
  element,
  document.getElementById('root')
);
```

- JSX도 표현식이다. (JSX를 if 구문 밑 for loop 안에 사용하고, 변수에 할당하고, 인자로 사용, 함수로부터 반환할 수 있다.)
```js
function getGreeting(user) {
  if (user) {
    return <h1>Hello, {formatName(user)}!</h1>;
  }
  return <h1>Hello, Stranger.</h1>;
}
```

- 어트리뷰트에 따옴표를 이용해 문자열 리터럴을 정의할 수 있다.
`const element = <a href="https://www.reactjs.org"> link </a>;`

- 중괄호 사용하여 어트리뷰트에 JS표현식을 삽입할 수도 있다.
`const element = <img src={user.avatarUrl}></img>;`

- JSX는 HTML 보다는 JS에 가깝기 때문에 React DOM은 HTML 어트리뷰트 이름 대신 camelCase 프로퍼티 명명 규칙을 사용한다.
	- class => className, tabindex => tabIndex

- 태그가 비어있다면 XML처럼 />를 이용해 바로 닫아주어야 한다.

- Babel은 JSX를 `React.createElement()` 호출로 컴파일한다.

```js
const element = (
  <h1 className="greeting">
    Hello, world!
  </h1>
);
```

```js
const element = React.createElement(
	'h1',
	{className: 'greeting'},
	'Hello, world!'
);
```  
이 두개는 동일하다.

## 엘리먼트 렌더링
- 엘리먼트는 React 앱의 가장 작은 단위이다.
- 엘리먼트는 화면에 표시할 내용을 기술한다.
- 엘리먼트는 컴포넌트의 __구성 요소__ 이다.
- 브라우저 DOM 엘리먼트와 달리 React 엘리먼트는 일반 객체이며 쉽게 생성할 수 있다. React DOM은 React 엘리먼트와 일치하도록 DOM을 업데이트한다.

### DOM에 엘리먼트 렌더링하기
`<div id="root"></div>`
이 안에 들어가는 모든 엘리먼트는 React DOM에서 관리하기 때문에 이것을 __root__ DOM 노드라고 부른다.
React로 구현된 애플리케이션은 일반적으로 하나의 루트 DOM 노드가 있다. React를 기존앱에 통합하려는 경우 원하는 만큼 많은 수의 독립된 루트 DOM 노드가 있을 수 있다.

- React 엘리먼트를 루트 DOM 노드에 렌더링하려면 둘 다 `ReactDOM.render()`로 전달하면 된다.

```js
const element = <h1>Hello, world</h1>;
ReactDOM.render(element, document.getElementById('root'));
```

### 렌더링 된 엘리먼트 업데이트하기
- React 엘리먼트는 불변객체이다. 엘리먼트를 생성한 이후에는 해당 엘리먼트의 자식이나 속성을 변경할 수 없다. 엘리먼트는 특정 시점의 UI를 보여준다.
- UI를 업데이트하는 유일한 방법은 새로운 엘리먼트를 생성하고 이를 ReactDOM.render()로 전달하는 것이다.

```js
function tick() {
  const element = (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {new Date().toLocaleTimeString()}.</h2>
    </div>
  );
  ReactDOM.render(element, document.getElementById('root'));
}

setInterval(tick, 1000);
```
위 함수는 setInterval을 이용해 1초마다 ReactDOM.render를 호출한다.  
__실제로 대부분의 React 앱은 ReactDOM.render()를 한 번만 호출한다.__

### 변경된 부분만 업데이트하기
tick을 개발자도구로 살펴보면 매초 전체 UI를 다시 그리도록 엘리먼트를 만들었지만 React DOM은 내용이 변경된 텍스트 노드만 업데이트 하고 있는 걸 확인할 수 있다.

## Components와 Props
1. 함수 컴포넌트
	- 컴포넌트를 정의하는 가장 간단한 방법은 js 함수를 작성하는 것
	```js
	function Welcome(props) {
  	return <h1>Hello, {props.name}</h1>;
	}
	```

2. 클래스 컴포넌트
	```js
	class Welcome extends React.Component {
		render() {
			return <h1>Hello, {this.props.name}</h1>;
		}
	}
	```

### 컴포넌트 렌더링
이전까지는 DOM 태그만을 이용해 React 엘리먼트를 나타냈다.(div, h1, h2...)  
React 엘리먼트는 사용자 정의 컴포넌트로도 나타낼 수 있다.
`const element = <Welcome name="Sara" />;`  
React가 사용자 정의 컴포넌트로 작성한 엘리먼트를 발견하면 JSX 어트리뷰트와 자식을 해당 컴포넌트에 단일 객체로 전달한다. 이 객체를 __props__ 라고 한다.

```js
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

const element = <Welcome name="Sara" />;
ReactDOM.render(
  element,
  document.getElementById('root')
);
```
1. `<Welcome name="Sara" />` 엘리먼트로 ReactDOM.render()를 호출한다.
2. React는 `{name: "Sara"}`를 props로 하여 Welcome 컴포넌트를 호출한다.
3. Welcome 컴포넌트는 결과적으로 `<h1>Hello, Sara</h1>` 엘리먼트를 반환한다.
4. ReactDOM은 `<h1>Hello, Sara</h1>` 엘리먼트와 일치하도록 DOM을 업데이트한다.


- 컴포넌트의 이름은 항상 __대문자__ 로 시작한다.
- React는 __소문자__ 로 시작하는 컴포넌트를 __DOM태그__ 로 처리한다. 소문자로 시작하는 경우에는 내장 컴포넌트라는 것을 뜻한다.

### 컴포넌트 합성
컴포넌트는 자신의 출력에 다른 컴포넌트를 참조할 수 있다.

```js
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

function App() {
  return (
    <div>
      <Welcome name="Sara" />
      <Welcome name="Cahal" />
      <Welcome name="Edite" />
    </div>
  );
}

ReactDOM.render(
  <App />,
  document.getElementById('root')
);
```
- 일반적으로 새 React앱은 최상위에 단일 __App__ 컴포넌트를 가지고 있다. 

### 컴포넌트 나누기
```js
function Comment(props) {
  return (
    <div className="Comment">
      <div className="UserInfo">
        <img className="Avatar"
          src={props.author.avatarUrl}
          alt={props.author.name}
        />
        <div className="UserInfo-name">
          {props.author.name}
        </div>
      </div>
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```

- author(객체), text(문자열) 및 date(날짜)를 props로 받은 후 소셜 미디어 웹 사이트의 코멘트를 나타내는 컴포넌트
1. 먼저 Avatar 추출
```js
function Avatar(props) {
  return (
    <img className="Avatar"
      src={props.user.avatarUrl}
      alt={props.user.name}
    />
  );
}
```
- props의 이름도 author에서 더욱 일반화된 user로 변경
- props의 이름은 사용될 context가 아닌 컴포넌트 자체의 관점에서 짓는 것을 권장함

2. UserInfo 컴포넌트 추출
```js
function UserInfo(props) {
  return (
    <div className="UserInfo">
      <Avatar user={props.user} />
      <div className="UserInfo-name">
        {props.user.name}
      </div>
    </div>
  );
}
```

- 결과
```js
function Comment(props) {
  return (
    <div className="Comment">
      <UserInfo user={props.author} />
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```

### props는 읽기 전용
```js
function sum(a, b) {
  return a + b;
}
```
이런 함수를 순수함수 라고 한다.

```js
function withdraw(account, amount) {
  account.total -= amount;
}
```
이 함수는 자신의 입력값을 변경하기 때문에 순수 함수가 아니다.

- React 규칙  
__모든 React 컴포넌트는 자신의 props를 다룰 때 반드시 순수 함수처럼 동작해야 한다.__

## State와 생명주기(Lifecycle)
```js
function Clock(props) {
  return (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {props.date.toLocaleTimeString()}.</h2>
    </div>
  );
}

function tick() {
  ReactDOM.render(
    <Clock date={new Date()} />,
    document.getElementById('root')
  );
}

setInterval(tick, 1000);
```
앞의 tick 예제를 다시 살펴보자
```js
ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```
이렇게 만들어보기

### 함수에서 클래스로 변환하기
1. React.Component를 확장하는 동일한 이름의 ES6 class를 생성
2. render() 라고 불리는 빈 메서드를 추가
3. 함수의 내용을 render() 메서드 안으로 옮긴다.
4. render() 내용 안에 있는 props를 this.props로 변경
5. 남아있는 빈 함수 선언을 삭제

```js
const root = ReactDOM.createRoot(document.getElementById('root'));

class Clock extends React.Component {
	render() {
		return (
			<div>
				<h1>Hello, world!</h1>
				<h2>It is {this.props.date.toLocaleTimeString()}.</h2>
			</div>
		);
	}
}

function tick() {
  root.render(<Clock date={new Date()} />);
}

setInterval(tick, 1000);
```
- render 메서드는 업데이트가 발생할 때마다 호출되지만, 같은 DOM 노드로 `<clock />`을 렌더링하는 경우 Clock 클래스의 단일 인스턴스만 사용된다. 이것은 로컬 state와 생명주기 메서드와 같은 부가적인 기능을 사용할 수 있게 해줌

### 클래스에 로컬 State 추가하기
1. render() 메서드 안에 있는 this.props.date를 this.state.date로 변경
```js
class Clock extends React.Component {
  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```
2. 초기 this.state를 지정하는 class constructor를 추가
```js
class Clock extends React.Component {
	constructor(props) {
		super(props);
		this.state = {date: new Date()};
	}

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```

- 여기서 어떻게 props를 기본 constructor에 전달하는지 유의해야한다.
- 클래스 컴포넌트는 항상 `props`로 기본 constructor를 호출해야 한다.

3. `<Clock /> 요소에서 `date prop`을 삭제
```js
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}

ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```

### 생명주기 메서드를 클래스에 추가하기
Clock이 스스로 타이머를 설정하고 매초 스스로 업데이트하도록 만들어보자
- Clock이 처음 DOM에 렌더링 될 때마다 타이머를 설정하려고 한다. 이것은 React에서 __마운팅__ 이라고 한다.
- 또한 Clock에 의해 생성된 DOM이 삭제될 때마다 타이머를 해제하려고 한다. 이것은 __언마운팅__ 이라고 한다.

```js
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  componentDidMount() {
  }

  componentWillUnmount() {
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```
- 이러한 메서드들은 __생명주기 메서드__ 라고 불린다.
- `componentDidMount()` 메서드는 컴포넌트 출력물이 DOM에 렌더링 된 후에 실행된다.
```js
  componentDidMount() {
    this.timerID = setInterval(
      () => this.tick(),
      1000
    );
  }
```
- `this.props`가 React에 의해 스스로 설정되고 `this.state`가 특수한 의미가 있지만, 타이머 ID와 같이 데이터 흐름 안에 포함되지 않는 어떤 항목을 보관할 필요가 있다면 자유롭게 클래스에 수동으로 부가적인 필드를 추가해도 된다.

- `componentWillUnmount()` 생명주기 메서드 안에 있는 타이머를 분해하기
```js
 componentWillUnmount() {
    clearInterval(this.timerID);
  }
```

- 마지막으로 Clock 컴포넌트가 매초 작동하도록 하는 tick() 메서드 구현하기
- 이것은 컴포넌트 로컬 state를 업데이트하기 위해 `this.setState()`를 사용한다.

```js
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  componentDidMount() {
    this.timerID = setInterval(
      () => this.tick(),
      1000
    );
  }

  componentWillUnmount() {
    clearInterval(this.timerID);
  }

  tick() {
    this.setState({
      date: new Date()
    });
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}

ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```

### 요약
1. `<Clock />` 이 `ReactDOM.render()`로 전달되었을 때 React는 Clock 컴포넌트의 constructor를 호출한다. Clock이 현재 시각을 표시해야 하기 때문에 현재 시각이 포함된 객체로 `this.state`를 초기화한다. 나중에 이 state를 업데이트 할 것
2. React는 Clock 컴포넌트의 `render()` 메서드를 호출. 이를 통해 React는 화면에 표시되어야 할 내용을 알게 된다. 그 다음 React는 Clock의 렌더링 출력값을 일치시키기 위해 DOM을 업데이트한다.
3. Clock 출력값이 DOM에 삽입되면, React는 `componentDidMount()` 생명주기 메서드를 호출한다. 그 안에서 Clock 컴포넌트는 매초 컴포넌트의 `tick()` 메서드를 호출하기 위한 타이머를 설정하도록 브라우저에 요청한다.
4. 매초 브라우저가 `tick()` 메서드를 호출. 그 안에서 Clock 컴포넌트는 `setState()`에 현재 시각을 포함하는 객체를 호출하면서 UI 업데이트를 진행한다. `setState()` 호출 덕분에 React는 state가 변경된 것을 인지하고 화면에 표시될 내용을 알아내기 위해 `render()` 메서드를 다시 호출한다. 이 때 `render()` 메서드 안의 `this.state.date`가 달라지고 렌더링 출력값은 업데이트된 시각을 포함한다. React는 이에 따라 DOM을 업데이트한다.
5. Clock 컴포넌트가 DOM으로부터 한 번이라도 삭제된 적이 있다면 React는 타이머를 멈추기 위해 `componentWillUnmount()` 생명주기 메서드를 호출한다.

### State 올바르게 사용하기
`setState()` 에 대해서 알아야 할 세 가지

1. 직접 State를 수정하지 말기
```js
// Wrong
this.state.comment = 'Hello';
```
- 이 코드는 컴포넌트를 다시 렌더링하지 않는다.
- 대신 setState를 사용하기
- `this.state`를 지정할 수 있는 유일한 공간은 바로 __constructor__ 이다.
```js
// Correct
this.setState({comment: 'Hello'});
```

2. State 업데이트는 비동기적일 수 있다.
- React는 성능을 위해 여러 setState() 호출을 단일 업데이트로 한꺼번에 처리할 수 있다.
- `this.props`와 `this.state`가 비동기적으로 업데이트될 수 있기 때문에 다음 state를 계산할 때 해당 값에 의존해서는 안된다.
```js
// Wrong
this.setState({
  counter: this.state.counter + this.props.increment,
});
```
- 이를 수정하기 위해 객체보다는 함수를 인자로 사용하는 다른 형태의 `setState()` 사용하기. 그 함수는 이전 state를 첫 번째 인자로 받아들일 것이고, 업데이트가 적용된 시점의 props를 두 번째 인자로 받아들일 것이다.

```js
// Correct
this.setState((state, props) => ({
  counter: state.counter + props.increment
}))
```

3. State 업데이트는 병합된다.
`setState()`를 호출할 때 React는 제공한 객체를 현재 state로 병합한다.

- state는 다양한 독립적인 변수를 포함할 수 있다.
```js
  constructor(props) {
    super(props);
    this.state = {
      posts: [],
      comments: []
    };
  }
```
- 별도의 setState() 호출로 이러한 변수를 독립적으로 업데이트할 수 있다.
```js
  componentDidMount() {
    fetchPosts().then(response => {
      this.setState({
        posts: response.posts
      });
    });

    fetchComments().then(response => {
      this.setState({
        comments: response.comments
      });
    });
  }
```
- 병합은 얕게 이루어지기 때문에 `this.setState({comments})`는 `this.state.posts`에 영향을 주진 않지만 `this.state.comments`는 완전히 대체된다.

### 데이터는 아래로 흐른다.
- state는 종종 로컬 또는 캡슐화라고 불린다. state가 소유하고 설정한 컴포넌트 이외에는 어떠한 컴포넌트에도 접근할 수 없다.
- 컴포넌트는 자신의 state를 자식 컴포넌트에 props로 전달할 수 있다.
`<FormattedDate date={this.state.date} />`

- `FormattedDate` 컴포넌트는 date를 자신의 props로 받을 것이고 이것이 Clock의 state로부터 왔는지, Clock의 props에서 왔는지, 수동으로 입력한 것인지 알지 못한다.
```js
function FormattedDate(props) {
  return <h2>It is {props.date.toLocaleTimeString()}.</h2>;
}
```

- 일반적으로 이를 "top-down하향식" 또는 단방향식 데이터 흐름이라고 한다. 모든 state는 항상 특정한 컴포넌트가 소유하고 있으며 그 state로부터 파생된 UI 또는 데이터는 오직 트리구조에서 자신의 "아래"에 있는 컴포넌트에만 영향을 미친다.

## 이벤트 처리하기
React 엘리먼트에서 이벤트를 처리하는 방식은 DOM 엘리먼트에서 이벤트를 처리하는 방식과 매우 유사하다.

DOM과 React의 문법 차이
1. React의 이벤트는 소문자 대신 camelCase를 사용한다.
2. JSX를 사용하여 문자열이 아닌 함수로 이벤트 핸들러를 전달한다.

- HTML에서의 이벤트 처리
```html
<button onclick="activateLasers()">
  Activate Lasers
</button>
```

- React에서의 이벤트 처리
```js
<button onClick={activateLasers}>
  Activate Lasers
</button>
```

- 또 다른 차이점으로 React에서는 false를 반환해도 기본 동작을 방지할 수 없다. 반드시 `preventDefault`를 명시적으로 호출해야 한다.

```HTML
<form onsubmit="console.log('You clicked submit.'); return false">
  <button type="submit">Submit</button>
</form>
```
- 일반 HTML에서 폼을 제출할 때 가지고 있는 기본 동작을 방지하기 위해서 `return false`를 작성할 수 있다.

- React에서는 밑과 같이 작성
```js
function Form() {
  function handleSubmit(e) {
    e.preventDefault();
    console.log('You clicked submit.');
  }

  return (
    <form onSubmit={handleSubmit}>
      <button type="submit">Submit</button>
    </form>
  );
}
```
- 여기서 e는 합성 이벤트.
- React를 사용할 때 DOM 엘리먼트가 생성된 후 리스너를 추가하기 위해 addEventListener를 호출할 필요가 없다. 대신 엘리먼트가 처음 렌더링될 때 리스너를 제공하면 된다.

- ES6 클래스 사용 -> 일반적인 패턴은 이벤트 핸들러를 클래스의 메서드로 만드는 것
```js
class Toggle extends React.Component {
  constructor(props) {
    super(props);
    this.state = {isToggleOn: true};

    // 콜백에서 `this`가 작동하려면 아래와 같이 바인딩 해주어야 합니다.
    this.handleClick = this.handleClick.bind(this);
  }

  handleClick() {
    this.setState(prevState => ({
      isToggleOn: !prevState.isToggleOn
    }));
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        {this.state.isToggleOn ? 'ON' : 'OFF'}
      </button>
    );
  }
}

ReactDOM.render(
  <Toggle />,
  document.getElementById('root')
);
```
- JS에서 클래스 메서드는 기본적으로 __바인딩__ 되어 있지 않다. this.handleClick을 바인딩하지 않고 onClick에 전달하였다면, 함수가 실제 호출될 때 this는 undefined가 된다.
- 이는 JS에서 함수가 작동하는 방식의 일부.
- 일반적으로 onClick={this.handleClick}과 같이 뒤에 ()를 사용하지 않고 메서드를 참조할 경우, 해당 메서드를 바인딩 해야한다.

### 이벤트 핸들러에 인자 전달하기
루프 내부에서는 이벤트 핸들러에 추가적인 매개변수를 전달하는 것이 일반적이다.
```js
<button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button>
<button onClick={this.deleteRow.bind(this, id)}>Delete Row</button>
```
- 위 두 줄은 같으며 각각 화살표 함수와 bind를 사용.
- 두 경우 모두 React 이벤트를 나타내는 e 인자가 ID 뒤에 두 번째 인자로 전달된다. 화살표 함수를 사용하면 명시적으로 인자를 전달해야 하지만 bind를 사용할 경우 추가 인자가 자동으로 전달된다.

## 조건부 렌더링
React에서는 원하는 동작을 캡슐화하는 컴포넌트를 만들 수 있다. 이렇게 하면 애플리케이션의 상태에 따라서 컴포넌트 중 몇 개만을 렌더링할 수 있다.

- React에서 조건부 렌더링은 JS 조건 처리와 같이 동작한다.
- if나 조건부 연산자와 같은 JS 연산자를 현재 상태를 나타내는 엘리먼트를 만드는 데에 사용하면 React는 현재 상태에 맞게 UI를 업데이트 할 것이다.

```js
function UserGreeting(props) {
  return <h1>Welcome back!</h1>;
}

function GuestGreeting(props) {
  return <h1>Please sign up.</h1>;
}
```
```js
function Greeting(props) {
  const isLoggedIn = props.isLoggedIn;
  if (isLoggedIn) {
    return <UserGreeting />;
  }
  return <GuestGreeting />;
}

ReactDOM.render(
  // Try changing to isLoggedIn={true}:
  <Greeting isLoggedIn={false} />,
  document.getElementById('root')
);
```
- isLoggedIn prop 에 따라서 다른 인사말을 함

### 엘리먼트 변수
엘리먼트를 저장하기 위해 변수를 사용할 수 있다. 출력의 다른 부분은 변하지 않은 채로 컴포넌트의 일부를 조건부로 렌더링 할 수 있다.
```js
function LoginButton(props) {
  return (
    <button onClick={props.onClick}>
      Login
    </button>
  );
}

function LogoutButton(props) {
  return (
    <button onClick={props.onClick}>
      Logout
    </button>
  );
}
```
```js
class LoginControl extends React.Component {
  constructor(props) {
    super(props);
    this.handleLoginClick = this.handleLoginClick.bind(this);
    this.handleLogoutClick = this.handleLogoutClick.bind(this);
    this.state = {isLoggedIn: false};
  }

  handleLoginClick() {
    this.setState({isLoggedIn: true});
  }

  handleLogoutClick() {
    this.setState({isLoggedIn: false});
  }

  render() {
    const isLoggedIn = this.state.isLoggedIn;
    let button;
    if (isLoggedIn) {
      button = <LogoutButton onClick={this.handleLogoutClick} />;
    } else {
      button = <LoginButton onClick={this.handleLoginClick} />;
    }

    return (
      <div>
        <Greeting isLoggedIn={isLoggedIn} />
        {button}
      </div>
    );
  }
}

ReactDOM.render(
  <LoginControl />,
  document.getElementById('root')
);
```

### JSX안에서 인라인으로 조건 처리하기
1. 논리 && 연산자로 if를 인라인으로 표현하기
- JSX안에는 중괄호를 이용해서 표현식을 포함할 수 있다.

```js
function Mailbox(props) {
  const unreadMessages = props.unreadMessages;
  return (
    <div>
      <h1>Hello!</h1>
      {unreadMessages.length > 0 &&
        <h2>
          You have {unreadMessages.length} unread messages.
        </h2>
      }
    </div>
  );
}

const messages = ['React', 'Re: React', 'Re:Re: React'];
ReactDOM.render(
  <Mailbox unreadMessages={messages} />,
  document.getElementById('root')
);
```
- JavaScript에서 true && expression은 항상 expression으로 평가되고 false && expression은 항상 false로 평가된다.
- && 뒤의 엘리먼트는 조건이 true일 때 출력된다. false라면 React는 무시하고 건너뛴다.

2. 조건부 연산자로 if-else 구문 인라인으로 표현하기
```js
render() {
  const isLoggedIn = this.state.isLoggedIn;
  return (
    <div>
      The user is <b>{isLoggedIn ? 'currently' : 'not'}</b> logged in.
    </div>
  );
}
```

### 컴포넌트가 렌더링하는 것을 막기
가끔 다른 컴포넌트에 의해 렌더링될 때 컴포넌트 자체를 숨기고 싶을 때가 있을 수 있다. 이때는 렌더링 결과를 출력하는 대신 null을 반환하면 해결할 수 있다.

- 컴포넌트의 render 메서드로부터 null을 반환하는 것은 생명주기 메서드 호출에 영향을 주지 않는다.