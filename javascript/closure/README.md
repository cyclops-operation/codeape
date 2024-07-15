# 클로저(Closure)

### 클로저(Closure)란?

클로저는 함수와 그 함수가 선언됐을때의 렉시컬 환경과의 조합이다.

```jsx
function outerFunc() {
  var x = 10;
  var innerFunc = function () {
    console.log(x);
  };
  innerFunc();
}

outerFunc(); // 10
```

> 함수 outerFunc() 내에서 내부함수 innerFunc() 가 선언되고 호출되었다. 이 때 내부 함수 innerFunc() 는 자신을 포함하고 있는 외부함수 outerFunc() 의 변수 x에 접근할 수 있다. 이는 함수 innerFunc() 가 함수 outerFunc() 의 내부에 선언되었기 때문이다.

> 스코프는 함수를 호출할 때가 아니라 함수를 어디에 선언하였는지에 따라 결정된다. 이를 렉시컬 스코핑(Lexical scoping)이라고 한다.

> 외부 함수 밖에서 내부함수가 호출되더라도 외부함수의 지역 변수에 접급할 수 있는걸 클로저(Clousure)라고 부른다.

### 클로저의 활용

클로저는 자신이 생성될때 환경(Lexical Scope)를 기억해야 하므로 메모리 차원에서 손해를 볼 수 있다. 하지만 클로저는 자바스크립트의 강력한 기능이므로 이를 활용해 유용하게 사용할 수 있다.

```jsx
<!DOCTYPE html>
<html>
	<body>
	  <button class="toggle">toggle</button>
	  <div class="box" style="width: 100px; height: 100px; background: red;"></div>

	  <script>
	    var box = document.querySelector('.box');
	    var toggleBtn = document.querySelector('.toggle');

	    var toggle = (function () {
	      var isShow = false;

	      // ① 클로저를 반환
	      return function () {
	        box.style.display = isShow ? 'block' : 'none';
	        // ③ 상태 변경
	        isShow = !isShow;
	      };
	    })();

	    // ② 이벤트 프로퍼티에 클로저를 할당
	    toggleBtn.onclick = toggle;
	  </script>
	</body>
</html>
```

> 1. 즉시실행 함수는 함수를 반환하고 즉시 소멸한다. 즉시실행함수가 반환한 함수는 자신이 생성됐을때 렉시컬 환경에 속한 변수 isShow 를 기억하는 클로저다. 클로저가 기억하는 변수 isShow 는 box 요소의 표시 상태를 나타낸다.
>
> 2. 버튼을 클릭하면 이벤트 프로퍼티에 할당한 이벤트 핸들러인 클로저가 호출된다. 이 때 .box 요소의 표시 상태를 나타내는 변수 `isShow` 의 값이 변경된다. 변수 `isShow` 는 클로저에 의해 참조되고 있기 때문에 유효하며 자신의 변경된 최신 상태를 계속해서 유지한다.
>
> 3. 버튼을 클릭하면 이벤트 프로퍼티에 할당한 이벤트 핸들러인 클로저가 호출된다. 이 때 .box 요소의 표시 상태를 나타내는 변수 isShow 의 값이 변경된다. 변수 isShow 는 클로저에 의해 참조되고 있기 때문에 유효하며 자신의 변경된 최신 상태를 계속해서 유지한다.
>
> 4. 전역변수를 이용해 사용하는 경우 언제든지 누구나 접근할 수 있고 변경할 수 있기 때문에 많은 부작용을 유발해 오류의 원인이 되므로 사용을 지양하고 클로저를 활용하는 것이 좋다.
