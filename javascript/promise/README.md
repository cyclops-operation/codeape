# Promise와 Async, Await

### Callback

대표적인 예로 다른 경로에 위치한 스크립트를 실행시키고 이 스크립트 내에서 동작하는 함수를 바로 실행하는 경우 함수가 존재하지 않는다는 에러가 발생한다.

#### Callback을 사용하지 않은 케이스

```jsx
function loadScript(src) {
  // <script> 태그를 만들고 페이지에 태그를 추가합니다.
  // 태그가 페이지에 추가되면 src에 있는 스크립트를 로딩하고 실행합니다.
  let script = document.createElement("script");
  script.src = src;
  document.head.append(script);
}

// 해당 경로에 위치한 스크립트를 불러오고 실행함 ( new Function() {...} 이 있다고 가정 )
loadScript("/my/script.js");

newFunction(); // 함수가 존재하지 않는다는 에러 발생
```

> 이런 문제가 발생하는 이유는 브라우저가 스크립트를 읽어올 수 있는 시간을 충분히 확보하지 못하는 상황에서 해당 스크립트 내 함수를 실행했기 때문에 발생하는데 이런 문제를 해결하기 위해 Callback을 사용한다.

<br />

#### Callback을 사용한 케이스 ( 일반적인 방식 )

```jsx
function loadScript(src, callback) {
  let script = document.createElement("script");
  script.src = src;

  script.onload = () => callback(script);

  document.head.append(script);
}

loadScript("/my/script.js", function () {
  // 정상적인 상태의 함수가 호출된다. ( 콜백 함수는 스크립트 로드가 끝나면 실행됨. )
  newFunction();
});
```

> 기존 코드를 변경하여 스크립트 함수가 실행된 이후 함수가 실행될 수 있도록 Callback을 사용하는 방식을 CallBack 기반 비동기 프로그래밍 이라고 하는데 이렇게 무언가를 비동기적으로 수행하는 함수는 동작이 모두 처리된 후 실행되어야 할 때 반드시 전달해야 한다.

<br />

#### Callback 속 Callback

실행할 스크립트가 두 가지가 있는 경우 순차적으로 불러오려면 첫번째 Callback 안에서 두번째 Callback을 실행하는 방식으로 불러올 수 있다.

```js
loadScript("/my/script.js", function (script) {
  alert(`${script.src}을 로딩했습니다. 이젠, 다음 스크립트를 로딩합시다.`);

  loadScript("/my/script2.js", function (script) {
    alert(`두 번째 스크립트를 성공적으로 로딩했습니다.`);
  });
});
```

> 이 방식에서 문제가 생기는데 두번째 스크립트가 실행된 이후 추가적인 스크립트를 실행하기 위해 매번 중첩으로 추가해서 실행하게 되는 구조가 발생하게 되는데 이를 Callback Hell ( 콜백 지옥 ), Pyramid of doom ( 멸망의 피라미드 ) 라고 한다. 비동기 동작이 순차적으로 하나씩 늘어나서 중첩으로 호출하게 되는 현상

<br />

#### Callback Hell(콜백 지옥)을 피하는 방법

중첩으로 호출을 만들어내는 방식이 아니라 각 동작을 독립적인 함수로 만들어서 문제를 해결할 수 있다.

```jsx
loadScript("1.js", step1);

// 첫번째 실행되는 함수
function step1(error, script) {
  if (error) {
    handleError(error);
  } else {
    // 두번째 함수 실행
    loadScript("2.js", step2);
  }
}

// 두번째 실행될 함수
function step2(error, script) {
  if (error) {
    handleError(error);
  } else {
    // 세번째 함수 실행
    loadScript("3.js", step3);
  }
}

// 세번째 실행될 함수
function step3(error, script) {
  if (error) {
    handleError(error);
  } else {
    // 모든 스크립트가 로딩되면 다른 동작을 수행합니다. (*)
  }
}
```

> 이 방식은 단순히 Callback Hell(콜백 지옥)을 피하기 위한 방법이기 때문에 깊은 중첩은 없지만 코드의 가독성과 재사용성이 떨어지는 구조를 가지고 있다. 이런 문제를 해결하기 위해 Promise 가 등장한다.

<br />

### Promise의 등장

비동기 처리를 위해 Callback 패턴을 사용해 코드의 복잡도가 생기는 문제로 ES6 버전부터 Promise가 도입이 되었는데 Callback 을 사용한 방식보다 간단해지고 성공, 실패를 직관적으로 나타내기 때문에 비동기 작업을 더 쉽게 구성하고 에러를 일관적으로 처리할 수 있게되었다.

```jsx
const promise = new Promise((resolve, reject) => {
    // 비동기 작업 수행
  if (/* 성공 */) {
      resolve(value);
  } else { // 실패
      reject(error);
  }
});
```

> Promise는 세 가지 상태 대기(Pending) , 이행 (Fulfilled) , 거부 (Rejected) 를 가지고 있고 한번의 실행으로 연쇄적인 작업을 진행할 수 있다.

<br />

#### Promise의 3가지 상태

- 대기 (Pending) → 비동기 처리 로직이 아직 완료되지 않은 상태
- 이행 (Fulfilled) → 비동기 처리가 완료되어 Promise가 결과 값을 반환한 상태
- 거부 (Rejected) → 실패라고도 불리며 비동기 처리가 실패하거나 오류가 발생한 상태

> 3가지 상태와 값의 순서
> 상태: pending → resolve → fulfilled ( reject가 호출되면 rejected로 변경됨 )
> 값: undefined → resolve(value)가 호출되면 되면 value → reject(error)가 호출되면 error

<br />

#### Resolve와 Reject

```jsx
// Resolve 사용 예시
function getData() {
  return new Promise(function (resolve, reject) {
    var data = 100;
    resolve(data);
  });
}

// resolve()의 결과 값 data를 resolvedData로 받음
getData().then(function (resolvedData) {
  console.log(resolvedData); // 100
});
```

> Promise에서 resolve로 값을 전달하여 실행하면 이행(fulfilled) 상태가 되고 .then 메서드를 이용해 전달받은 값을 사용할 수 있다.

```jsx
// Reject 사용 예시
function getData() {
  return new Promise(function (resolve, reject) {
    reject(new Error("Request is failed"));
  });
}

// reject()의 결과 값 Error를 err에 받음
getData()
  .then()
  .catch(function (err) {
    console.log(err); // Error: Request is failed
  });
```

> Promise에서 reject로 에러를 전달해서 실행하면 거부(Rejected) 로 상태가 변경되고 .catch 메서드로 실패한 이유(실패 처리의 결과 값)을 전달 받을 수 있다.

<br />

#### .then, .catch, .finally 메서드

```jsx
promise.then(
  function (result) {
    /* 결과(result)를 다룹니다 */
  },
  function (error) {
    /* 에러(error)를 다룹니다 */
  }
);
```

> 첫 번째 인수는 요청이 성공했을때 실행되는 함수이고 결과를 전달받는다.
> 두 번째 인수는 거부 되었을때(실패 했을때) 실행되는 함수이고 에러를 전달받는다.

```jsx
let promise = new Promise((resolve, reject) => {
  setTimeout(() => reject(new Error("에러 발생!")), 1000);
});

// .catch(f)는 promise.then(null, f)과 동일하게 작동합니다
promise.catch(alert); // 1초 뒤 "Error: 에러 발생!" 출력
```

> .catch는 에러가 발생한 경우 에러를 전달받아 함수를 실행한다.
> .then(null, f) 와 같이 null을 첫번째 인수로 전달하면 .catch와 동일하게 작동한다.

<br />

#### 에러처리는 가급적 .catch()를 사용하기

```jsx
// then()의 두 번째 인자로는 감지하지 못하는 오류
function getData() {
  return new Promise(function (resolve, reject) {
    resolve("hi");
  });
}

getData().then(
  function (result) {
    console.log(result);
    throw new Error("Error in then()"); // Uncaught (in promise) Error: Error in then()
  },
  function (err) {
    console.log("then error : ", err);
  }
);
```

```jsx
// catch()로 오류를 감지하는 코드
function getData() {
  return new Promise(function (resolve, reject) {
    resolve("hi");
  });
}

getData()
  .then(function (result) {
    console.log(result); // hi
    throw new Error("Error in then()");
  })
  .catch(function (err) {
    console.log("then error : ", err); // then error :  Error: Error in then()
  });
```

> .then(null, f) 으로 처리하는 방식과 .catch() 로 처리하는 두가지 방식이 있지만 첫 번째 콜백 내에서 발생하는 오류는 제대로 잡아내지 못하는 경우가 있다. 따라서 더 많은 예외 처리 상황을 위해 Promise 끝에 가급적 .catch() 메서드로 에러를 처리하는게 좋은 방향이다.

<br />

#### .finally

```jsx
new Promise((resolve, reject) => {
  /* 시간이 걸리는 어떤 일을 수행하고, 그 후 resolve, reject를 호출함 */
})
// 성공·실패 여부와 상관없이 프라미스가 처리되면 실행됨
.finally(() => 로딩 인디케이터 중지)
.then(result => result와 err 보여줌 => error 보여줌)
```

> Promise의 결과로 응답이 성공하거나 실패했다는 완료시점을 .finally() 메서드로 접근할 수 있다. 예를 들어 Promise를 실행함과 동시에 로딩 인디케이터를 노출시켰다면 성공하던지 실패하던지 실행이 종료됨과 동시에 로딩 인디케이터를 다시 비노출 시키는 동작을 .finally() 메서드로 처리하면 된다.

> .finally() 메서드는 자동으로 다음 핸들러에 결과와 에러를 전달한다. 예시 코드와 같이 .finally() 메서드 뒤에 .then() 메서드를 실행하면 원하는 값을 전달받을 수 있다.

<br />

### Async와 Await

async와 await 문법을 사용하면 Promise를 좀 더 편하게 사용할 수 있다. ES2017에서 비동기처리를 더욱 간결하고 직관적으로 만들 수 있도록 도입되었고 내부적으로 Promise를 사용한다.

#### Async

```jsx
// Promise로 리턴하지 않은 경우
async function f() {
  return 1;
}

f().then(alert); // 1

// Promise로 리턴한 경우
async function f() {
  return Promise.resolve(1);
}

f().then(alert); // 1
```

> function 앞에 async를 붙이면 이 함수는 항상 Promise를 반환한다. Promise가 아닌 값을 반환하더라도 이행 상태의 Promise로 값을 감싸 이행된 Promise가 반환된다. (Promise로 리턴하지 않아도 Promise로 리턴된다.)

<br />

#### Await

```jsx
async function f() {
  let promise = new Promise((resolve, reject) => {
    setTimeout(() => resolve("완료!"), 1000);
  });

  // await는 async 함수 안에서만 동작한다.
  let result = await promise; // 프라미스가 이행될 때까지 기다림 (*)
  alert(result); // "완료!"
}

f();
```

> Async 함수를 실행하고 함수 본문이 실행되는 도중에 await 을 만나면 실행이 잠시 중단 되었다가 Promise가 처리되고 나면 실행이 재개된다. 이 때 Promise 객체의 result 값이 변수 result에 할당된다.

> Promise가 처리되길 기다리는 동안에 다른 일(다른 스크립트 실행, 이벤트 처리 등)을 할 수 있기 때문에 CPU 리소스가 낭비되지 않는다.

> 일반 함수에는 await을 사용할 수 없다. 사용하게 되는 경우 에러가 발생한다.

### 레퍼런스

[[프로미스와 async, await]](https://ko.javascript.info/async)

[[자바스크립트 Promise 쉽게 이해하기]](https://joshua1988.github.io/web-development/javascript/promise-for-beginners/)

[[자바스크립트 비동기 처리와 Promise 이해하기]](https://f-lab.kr/insight/understanding-async-processing-and-promise-in-javascript)
