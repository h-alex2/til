HTTP 
Hypertext Transfer Protocal
서버에 데이터를 요청해서 받아오는 방법 : AJAX 
Asynchronous Javascript And Xml
XHR : XMLHttpRequest
xml - html과 마찬가지로 데이터를 표현할 수 있는 방법

Client <- XMLHttpRequest , fetch() API -> Server
XML을 사용하면 불필요한 태그, 사이즈 커짐, 가독성 좋지 않음 -> 이제 XML은 많이 쓰지 않고 JSON을 많이 쓰고있따. 

JSON : JavaSCript Object Notation
1999 ecmaScript 3rd에 쓰여진 object에 큰 영감을 받아 만들어짐
Object {key: value} 의 형태
브라우저 뿐만 아니라 모바일 서버와 데이터를 주고 받을 때 또는 서버와 통신을 하지 않을 때 오브젝트를 파일 시스템에 저장할 때도 많이 이용

## JSON
- 데이터를 주고받을 때 사용할 수 있는 가장 간단한 포맷
- 텍스트를 기반으로 한 가벼운 구조
- 읽기 쉬움
- key-value 쌍
- 데이터를 서버와 주고받을 때 직렬화하고 데이터를 전송할 때 쓰임 (직렬화 : serialize)
- __언어, 플랫폼에 상관없이 쓸 수 있다.__



## 1. Object to JSON

```js
let json = JSON.stringify(['apple', 'banana']);
console.log(json) // '["apple","banana"]' (double quotes : json의 규격사항)

const rabbit = {
  name: 'tory',
  color: 'white',
  size: null,
  birthDate: new Date(),
  symbol: Symbol('id'),
  jump: () => {
    console.log('${name} can jump!');
  },
}

json = JSON.stringify(rabbit);
console.log(json) //'{"name":"tory","color":"white","size":null,"birthDate":"2022-04-30T08:54:56.234Z"}'

// 함수는 오브젝트에 있는 데이터가 아니기 때문에 제외된다. 그리고 JS에만 있는 symbol도 포함되지 않는다.

json = JSON.stringify(rabbit, ['name']);
console.log(json) //'{"name":"tory"}'

json = JSON.stringify(rabbit, (key, value) => {
  console.log(`key: ${key}, value: ${value}`);
  return key === 'name' ? 'ellie' : value;
});
// key: , value: [object Object]
// key: name, value: tory
// key: color, value: white
// key: size, value: null
// key: birthDate, value: 2022-04-30T08:54:56.234Z
// key: jump, value: () => {
//     console.log('${name} can jump!');
//   }
// 조금 더 세밀하게 조절하고 싶을 때 callback 함수를 사용할 수 있다.
```

## 2. JSON to Object

```js
const obj = JSON.parse(json);
console.log(obj);
//{name: 'ellie', color: 'white', size: null, birthDate: '2022-04-30T08:54:56.234Z'}

// rabbit에 있었던 jump 라는 api는 없어지게된다. JSON으로 변환했을 때 포함되지 않았기 때문

console.log(rabbit.birthDate.getDate());
// 30
// rabbit에 있었던 birthDate에는 new Date()가 있어 getDate() api를 쓸 수 있다.
console.log(obj.birthDate.getDate());
// Uncaught TypeError: obj.birthDate.getDate is not a function
// obj에는 값 자체가 string으로 들어가있기 때문에 getDate()를 쓸 수 없다.

// Date() 오브젝트 자체로 변환하고 싶다면 콜백함수를 이용할 수 있다.
const obj = JSON.parse(json, (key, value) => {
  console.log(`key: ${key}, value: ${value}`);
  return key === 'birthDate' ? new Date(value) : value;
})
console.log(obj.birthDate.getDate());
// 30
```

자바스크립트는 동기적으로 실행된다. synchronous
hoisting: var, function declaration 자동적으로 제일 위로 선언되는 것

asynchronous  

1. 동기(Synchronous) callback

```js
function printImmediately(print) {
  print();
}
printImmediately(() => console.log('hello'));
```
2. 비동기(Asynchronous) callback

```js
function printWithDelay(print, timeout) {
  setTimeout(print, timeout);
}
printWithDelay(() => console.log('async callback'), 2000);
// 2초 뒤 async callback
```

## 콜백 지옥

```js
class UserStorage {

  loginUser(id, password, onSuccess, onError) {
    setTimeout(() => {
      if (
        (id === 'ellie' && password === 'dream') ||
        (id === 'coder' && password === 'academy')
      ) {
        onSuccess(id);
      } else {
        onError(new Error('not fount'));
      }
    }, 2000);
  }

  getRoles(user, onSuccess, onError) {
    setTimeout(() => {
      if (user === 'ellie') {
        onSuccess({ name: 'ellei', role: 'admin' });
      } else {
        onError(new Error('no access'));
      }
    })
  }
}

const userStorage = new UserStorage();
const id = prompt('enter your id');
const password = prompt('enter your password');
userStorage.loginUser(
  id,
  password,
  (user) => {
    userStorage.getRoles(
      user,
      (userWithRole) => {
        alert(`Hello ${userWithRole.name}, you have a ${userWithRole.role} role`)
      },
      (error) => {
        console.log(error);
      })
  },
  (error) => {
  console.log(error)
})

// 가독성이 굉장히 좋지 않다.
```
promise로 만들어보기

```js


class UserStorage {
  loginUser(id, password) {
    return new Promise((resolve, reject) => {
      setTimeout(() => {
        if (
          (id === 'ellie' && password === 'dream') ||
          (id === 'coder' && password === 'academy')
        ) {
          resolve(id);
        } else {
          reject(new Error('not found'));
        }
      }, 2000);
    });
  }

  getRoles(user) {
    return new Promise((resolve, reject) => {
      setTimeout(() => {
        if (user === 'ellie') {
          resolve({ name: 'ellie', role: 'admin' });
        } else {
          reject(new Error('no access'));
        }
      }, 1000);
    });
  }
}

const userStorage = new UserStorage();
const id = prompt('enter your id');
const password = prompt('enter your password');

userStorage
  .loginUser(id, password)
  .then(userStorage.getRoles)
  .then(user => alert(`Hello ${user.name}`))
  .catch(console.log)

```

## Promise
비동기적인 것을 수행할 때 콜백함수 대신 유용하게 쓸 수 있다.

### point
1. State 상태
2. producer와 consumer의 차이
  
- pending (우리가 지정한 operation이 수행중일 때)
- fulfilled (pending을 성공적으로 끝내고 난 후)
- rejected
- Producer vs Consumer

```js
function fetchUser() {
    return new Promise((resolve, reject) => {
        resolve('ellie');
    });
}

const user = fetchUser();
console.log(user);  //Promise {<pending>}
```


```js
function fetchUser() {
    return new Promise((resolve, reject) => {
        resolve('ellie');
    });
}

const user = fetchUser();
console.log(user);  //Promise {<fulfilled>: 'ellie'}
```


```js
  //promise는 class 이기 때문에 new 키워드로 생성
const promise = new Promise((resolve, reject) => {
  //네트워크, 파일을 읽어오는 것 들은 시간이 걸리는 경우가 많기 때문에 비동기적으로 처리하는 것이 좋다.
  console.log('doing something...');
}) // 바로 실행된다.
```
promise를 만드는 순간 전달 받은 executor라는 콜백함수가 __자동적으로__ 바로 실행된다는 걸 유의해야한다.

### 1. Producer
```js
const promise = new Promise((resolve, reject) => {
  console.log('doing something...');
  setTimeout(() => {
    // resolve('ellie'); //작동이 잘 되었다면 'ellie'를 호출
    reject(new Error('no netword'));
  }, 2000)
})
```

### 2. Consumers
- then, catch, finally 를 이용해 값을 받아올 수 있다.

then : promise가 정상적으로 잘 수행이 되어서 마지막 최종적으로 전달한 값이 value의 파라미터로 전달된다. then은 값을 바로 전달할 수도 있고 promise를 전달할 수도 있다.
finally : 성공, 실패 상관없이 무조건 마지막에 수행
```js 
promise
  .then(value => {
    console.log(value);
  }) //then을 호출하게되면 promise가 리턴되고 그 리턴된 promise에 .catch를 등록하는 것
  .catch(error => {
    console.log(error);
  }) 
  .finally(() => {
    console.log('finally')
  });
```

## Promise chaining

```js
const fetchNumber = new Promise((resolve, reject) => {
  setTimeout(() => resolve(1), 1000)
})

fetchNumber
  .then(num => num * 2) // num = 2
  .then(num => num * 3) // num = 6
  .then(num => {
    return new Promise((resolve, reject) => {
      setTimeout(() => resolve(num - 1), 1000); // num = 5
    });
  })
  . then(num => console.log(num)); // 5


```


```js
const getHen = () =>
  new Promise((resolve, reject) => {
    setTimeout(() => resolve('🐓'), 1000);
  });
const getEgg = hen =>
  new Promise((resolve, reject) => {
    setTimeout(() => resolve(`${hen} => 🥚`), 1000);
  });
const cook = egg =>
  new Promise((resolve, reject) => {
    setTimeout(() => resolve(`${egg} => 🍳`), 1000);
  });

getHen()
  .then(hen => getEgg(hen)) //받아오는 value를 다른 함수 호출에 사용하는 경우에는
  .then(egg => cook(egg)) // 밑과 같이 줄여쓸 수 있다.
  .then(meal => console.log(meal));

```

```js
getHen()
.then(getEgg)
.then(cook)
.then(console.log);
```
이렇게 줄일 수 있다.

## 오류 처리
```js
const getHen = () =>
  new Promise((resolve, reject) => {
    setTimeout(() => resolve('🐓'), 1000);
  });
const getEgg = hen =>
  new Promise((resolve, reject) => {
    setTimeout(() => resolve(`${hen} => 🥚`), 1000);
  });
const cook = egg =>
  new Promise((resolve, reject) => {
    setTimeout(() => resolve(`${egg} => 🍳`), 1000);
  });

getHen()
  .then(getEgg)
  .catch(error => {
    return '🍟';
  } // 위 then에서 발생하는 에러를 처리하기 위해 바로 밑에 catch를 달아서 에러처리를 해줬다.
  .then(cook)
  .then(console.log);
```

## async await

syntactic sugar (class 같은 것)
앞에 async를 붙여주면 promise를 쓰지 않아도 자동적으로 promise를 만들어준다.
```js
async function fetchUser() {
  return 'ellie';
}

const user = fetchUser();
user.then(console.log);
console.log(user);
```

## await
async가 붙은 함수 안에서만 쓸 수 있다.

```js
function delay(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}

async function getApple() {
  
}