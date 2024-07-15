# 호이스팅(hoisting)

### 호이스팅이란?

**변수&함수 선언**만 해당 스코프의 최상단으로 끌어올려지는 걸 호이스팅이라고 한다.

```jsx
console.log(a); // undefined
var a;

// 단 변수의 선언만 끌어올려지므로 값을 할당해도 결과는 undefined가 된다.
console.log(b); // undefined
var b = 1;
```

### 호이스팅이 발생하는 이유

자바스크립트 엔진에서 변수는 선언 → 초기화 → 할당을 거쳐 생성된다.

| 단계        | 설명                                                                                                                                              |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| 선언 단계   | 변수를 실행 컨텍스트 (실행 코드에 제공할 정보 객체)의 변수 객체에 등록                                                                            |
| 초기화 단계 | 변수 객체에 등록된 변수를 위한 메모리 공간을 확보한다. ( 이 때 undefined로 초기화됨 ) 메모리가 할당되면 메모리 참조를 통해 변수에 접근할 수 있다. |
| 할당 단계   | 사용자가 정의한 값을 변수에 할당                                                                                                                  |

> 1. 자바스크립트 엔진은 코드를 실행하기 전에 실행 컨텍스트에 등록된 변수 객체에 접근할 수 있다.
> 2. var 의 경우 변수 선언과 초기화가 함께 진행되므로 변수 객체 등록과 동시에 메모리 공간도 할당 받는다.
> 3. 메모리를 할당받은 상태이므로 호이스팅시 메모리 참조를 통해 변수 접근이 가능하다.

#### let, const

let과 const 는 var 와 달리 중복을 허용하지 않는다.

```jsx
var a = 5;
console.log(a); // 5

var a = 10;
console.log(a); // 10

a = 15;
console.log(a); // 15
```

> var 는 같은 범위 내 중복선언, 재할당 모두가 가능하다.
> 이로 인해 다른곳에서 같은 변수명을 사용하는 경우 뜻밖의 결과를 나타내게 되어 많은 버그를 발생시킬 수 있다.

<br />

```jsx
let greeting = "say Hi";
let times = 4;

if (times > 3) {
  let hello = "say Hello instead";
  console.log(hello); // "say Hello instead"
}
console.log(hello); // hello is not defined
```

> let 은 같은 범위(스코프) 내에서 재할당이 가능하고 재선언은 불가능하다.
> var 와 다른점은 선언과 동시에 초기화 되지 않고 선언 이전에 변수를 사용하려고 하면 에러가 발생한다. ( var 와 같이 호이스팅은 동일하게 발생한다. )

<br />

```jsx
const greeting = "say Hi";
// error: Assignment to constant variable.
greeting = "say Hello instead";

const greeting = "say Hi";
// error: Identifier 'greeting' has already been declared
const greeting = "say Hello instead";
```

> const 는 같은 범위(스코프) 내에서 재할당, 재선언이 불가능하다.
> const 로 선언한 객체 내 값을 재할당 할 수 있지만 처음 선언한 값 자체를 변경할 수 없다.
> let 과 마찬가지로 호이스팅은 발생하지만 초기화되지 않는다.

<br />

### TDZ (Temporal Dead Zone)

TDZ는 선언 단계 ~ 초기화 단계 사이를 이야기하는데 **선언보다 호출을 먼저하는 경우 에러가 발생**한다. 이를 TDZ에 걸렸다고 얘기한다.

### 함수 선언식과 함수 표현식

```jsx
// 함수 선언식
print(); // a

function print() {
  console.log("a");
}
```

> 함수 선언문의 경우 선언, 초기화, 할당이 동시에 진행되기 때문에 호이스팅은 물론 실행도 가능하다.

<br />

```jsx
// 함수 표현식
print(); // ReferenceError

const print = function () {
  console.log("a");
};
```

> 함수 표현식을 사용하면 호이스팅이 불가능하다.

### 레퍼런스

[[JS 호이스팅]](https://mong-blog.tistory.com/entry/JS-%ED%98%B8%EC%9D%B4%EC%8A%A4%ED%8C%85hoisting)

[[var, let, const의 차이점]](https://velog.io/@dongjun187/javascript-var-let-const-%EC%9D%98-%EC%B0%A8%EC%9D%B4%EC%A0%90)
