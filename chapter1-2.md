# 모던 리액트 딥 다이브: chapter1 [리액트 개발을 위해 꼭 알아야 할 자바스크립트]

## 1.4 클로저

### 1.4.1 클로저의 정의

- "함수와 함수가 선언된 어휘적 환경의 조합"으로 클로저는 함수와 그 함수가 선언된 렉시컬 환경의 조합이다. 클로저는 함수가 자신이 선언된 위치의 상위 스코프를 기억하고 있는 것을 의미한다.

```
function add() {
    const a = 10;
    function innerAdd() {
        const b = 20;
        console.log(a + b);
    }
    innerAdd(); //30
}

add();
```

- a 변수의 유효 범위는 add 전체이고, b의 유효점위는 innerAdd 전체이다. innerAdd는 add 내부에서 선언돼 있어 a를 사용할 수 있게 된 것이다. 즉 앞에서 말하는 "선언된 어휘적 환경" 이라는 것은 변수가 코드 내부에서 어디서 선언됐는지를 말하는 것이다.

### 1.4.2 변수의 유효범위, 스코프

- 클로저를 이해하기 위해서는 변수의 유효범위에 따라서 어휘적 환경이 결정된다고 언급했다. 이러한 변수의 유효 범위를 스코프라고 한다.

  - 전역스코프: 전역 레벨에 선언하는 것을 전역 스코프라고한다. 이 스코프에서 변수를 선언하면 어디서든 호출가능하다 브라우저 환경에서 전역 객체는 window, Node.js 환경에서는 global이다.
  - 함수스코프: 다른 언어와 달리 자바스크립트는 기본적으로 함수 레벨 스코프를 따른다. {} 블록이 스코프 범위를 결정하지 않는다.

  ```
  if(true) {
    var global = 'global';
  }
  console.log(global); // global
  ```

  - 자바스크립트에서 스코프는, 일단 가장 가까운 스코프에서 변수가 존재하는지를 먼저 확인해 본다.

  ### 1.4.3 클로저의 활용

  - 자바스크립트는 함수 레벨 스코프를 가지고 있으므로, 이렇게 선언된 함수 레벨 스코프를 활용해 어떤 작업을 할 수 있다는 개념이 바로 클로저이다.

  ```
  function outerFunction() {
   var x = 'hello'
   function innerFunction() {
       console.log(x);
   }
   return innerFunction;
  }
  const innerFunction = outerFunction();
  innerFunction(); // hello
  ```

  -클로저의 활용: 전역 스코프는 어디서든 원하는 값을 꺼내올 수 있다는 장점이 있지만, 반대로 이야기하면 누구든 접근할 수 있고 수정할 수 있다는 뜻도 된다.

  -리액트에서의 클로저: 클로저의 원리를 사용하고 있는 대표적인 것 중 하나가 바로 useState이다.

  ### 1.5 이벤트 루프와 비동기 통신의 이해

  - 자바스크립트는 싱글 스레드에서 작동한다. 한 번에 하나의 작업만 동기 방식으로 처리할 수 있다. 여기서 동기란 직렬 방식으로 작업을 처리하는 것을 의미하며, 이 요청이 시작된 이후에는 무조건 응답을 받은 이후에야 비로소 다른 작업을 처리 할 수 있다. 비동기 방식은 병렬 방식으로 작업을 처리하는 것을 의미한다.

  ## 1.5.1 싱클 스레드 자바스크립트

  - 자바스크립트는 싱글 스레드 언어이다. 왜 싱글 스레드로 설계되었을까? 멀티스레드는 여러가지 이점이 있지만 동시에 서로 같은 자원을 접근하다 이슈가 생기는 동시성 문제가 발생 할 수 있다.

  ## 1.5.2 이벤트 루프

  - 이벤트 루프는 ECMAScript, 즉 자바스크립트 표준에 나와 있는 내용은 아니다. 즉, 이벤트 루프란 자바스크립트 런타임 외부에서 자바스크립트의 비동기 실행을 돕기 위해 만들어진 장치라 볼 수 있다.

    - 호툴 스택: 자바스크립트에서 수행해야 할 코드나 함수를 순차적으로 담아두는 스택이다.

    ```
    function bar() {
        console.log('bar');
    }
    function baz() {
        console.log('baz');
    }
    function foo() {
        console.log('foo');
        setTimeout(bar, 0);
        baz();
    }
    foo();
    ```

    1. foo() 함수가 호출되면 호툴 스택에 foo가 쌓인다.
    2. foo() 함수내의 console.log가 호출스택에 들어간다.
    3. 2 의 실행이 완료된 이후에 다음 코드로 넘어간다. (foo()는 존재)
    4. setTimeout(bar().0)이 호출 스택에 들어간다.
    5. 4번에 대해 타이머 이벤트가 발생해 태스크 큐로 들어가고 스택에서 제거된다.
    6. baz() 함수가 호출 스택에 들어간다.
    7. baz() 함수 내의 console.log가 호출 스택에 들어간다.
    8. 7번의 실행이 완료된 이후에 다음 코드로 넘어간다. (foo(),baz()는 존재)
    9. baz() 에 남은 것이 없으므로 호출 스택에서 제거된다. (foo()는 존재)
    10. 더 이상 foo() 에 남은 것이 없으므로 호출 스택에서 제거된다.
    11. 호출 스택이 비워졌다.
    12. 이벤트 루프가 호출 스택이 비워져 있다는 것을 확인하고, 태스크 큐를 확인하니 4번에 들어갔던 bar()를 호풀스택에 들여보낸다.
    13. bar() 내부에 console.log가 호출 스택에 들어간다.
    14. 13의 실행이 끝나고, 다음 코드로 넘어간다 (bar()는 존재)
    15. 더 이상 bar()에 남은것이 없으므로 호출스택에서 제거된다.

    - 태스크 뮤는 자료구조의 큐가 아닌 set 형태를 띠고 있다. 태스크 큐에서 의마하는 실행햐야 할 태스크는 비동기 함수의 콜백 함수나 이벤트 핸들러를 말한다.
    - 이벤트 루프의 역할은 호출 스택에 실행 중인 코드가 있는지, 그리고 태스크 큐에 대기 중인 함수가 있는지 반복해서 확인하는 역할을 한다.

### 1.5.3 태스크 큐와 마이크로 태스크 큐

- 태스크 큐와 다르게, 마이크로 태스크 큐라는 것도 있다. 이벤트 루프는 하나의 마이크르로 태스크 큐로 기존의 태스크 큐와는 다른 태스크를 처리한다. 대표적으로 Promise가 있다. 마이크로 태스크 큐는 기존 태스크 큐보다 우선권을 갖는다. ex(setTimeout과 setInterval은 Promise보다 늦게 실행된다.) 마이크로 태스크 큐가 빌 때까지는 기존 태스크 큐의 실행은 뒤로 미루어진다.
- 렌더링은 각 메이크로 태스크 큐 작업이 끝날때마다 한 번씩 렌더링할 기회를 얻게 된다. 마이크로 태스크 큐는 모든 마이크로테스크가 완료 될때까지 렌더링이 발생하지 않고, 태스큐 큐는 태스크 사이에 렌더링이 일어날 수 있다.

## 1.6 리액트에서 자주 사용하는 자바스크립트 문법

### 1.6.1 구조 분해 할당

- 배열 구조 분해 할당
- useState 함수는 2개짜리 배열을 반환한다. 첫 번째 값을 value로, 두 번째 값을 setter로 사용 가능하다.
- 갹재 구조 분해 할당
- 객체 구조 분해 할당은 말 그대로 객체에서 값을 꺼내온 뒤 할당하는 것을 의미한다, 배열 구조 분해 할당과는 달리, 객체는 객체 내부 이름으로 꺼내온다는 차이가 있다.

### 1.6.2 전개 구문

- 전개 구문: 구조 분해 할당과는 다르게 배열이나 객체, 문자열과 같이 순회 할 수 있는 값에 대해 전개해 간결하게 사용할 수 있는 구문이다.
- 배열 전개 구문: 과거에는 배열 간 합성을 위해 push(), concat(), splice() 등의 메서드를 사용해야 했다.
  ₩₩₩
  const arr1 = [1,2,3];
  const arr2 = [4,5,6];
  const arr3 = [...arr1, ...arr2];
  console.log(arr3); // [1,2,3,4,5,6]
  ₩₩₩
- 객체 전개 구문: 객체 전개 구문은 객체 내부의 값을 복사하는 것을 의미한다.

```
const obj1 = {a:1, b:2};
const obj2 = {c:3, d:4};
const obj3 = {...obj1, ...obj2};
console.log(obj3); // {a:1, b:2, c:3, d:4}
```

### 1.6.3 객체 초기자

- 객체 초기자: 객체를 선언할 때 객체에 넣고자 하는 키와 값을 가지고 있는 변수가 이미 존재한다면 해당 값을 간결하게 넣어줄 수 있는 방식이다.

  ```

  const a = 1;
  const b = 2;
  const obj = {a, b};
  console.log(obj); // {a:1, b:2}
  ```

### 1.6.4 Array 프로토타입의 메서드: map, filter, reduce,forEach

- Array.prototype.map, Array.prototype.filter, Array.prototype.reduce, Array.prototype.forEach는 모두 배열과 관련된 메서드이다.
- Array.prototype.map: 배열 내부의 모든 요소에 대해 콜백 함수를 실행한 뒤 그 결과를 새로운 배열로 반환한다.

```
const arr = [1,2,3,4,5];
const newArr = arr.map((item) => item * 2);
console.log(newArr); // [2,4,6,8,10]
```

- Array.prototype.filter: 배열 내부의 모든 요소에 대해 콜백 함수를 실행한 뒤 그 결과가 true인 요소만 모아서 새로운 배열로 반환한다.

```
const arr = [1,2,3,4,5];
const newArr = arr.filter((item) => item % 2 === 0);
console.log(newArr); // [2,4]
```

- Array.prototype.reduce: 배열 내부의 모든 요소에 대해 콜백 함수를 실행한 뒤 그 결과를 누적하여 하나의 값으로 반환한다.

```
const arr = [1,2,3,4,5];
const sum = arr.reduce((acc, item) => acc + item, 0);
console.log(sum); // 15
```

- Array.prototype.forEach: 배열 내부의 모든 요소에 대해 콜백 함수를 실행한다.

```
const arr = [1,2,3,4,5];
arr.forEach((item) => console.log(item)); // 1 2 3 4 5
```

### 1.6.5 삼항 조건 연산자

- 삼항 조건 연산자: 조건에 따라 값을 선택하는 연산자이다. 조건이 참이면 첫 번째 값을 반환하고, 조건이 거짓이면 두 번째 값을 반환한다.

```
const a = 1;
const b = 2;
const result = a > b ? 'a가 b보다 크다' : 'a가 b보다 작다';
console.log(result); // a가 b보다 작다
```

여기서 잠깐
JSX 내부에서 삼항 연산자 말고도 조건부 렌더링을 구현할 수 있나요?

```

import { useState } from 'react';

export default function App() {
    const [color, setColor] = useState('');
    return (
        <div>
         {(() => {
            if (color === 'red') {
                return '빨간색이다.'
            }
            else {
                return '빨간색이 아니다.'
            }
        })()}
        </div>
    );
}
```

## 1.7 선택이 아닌 필수, 타입스크립트

### 1.7.1 타입스크립트란?

- 기존 자바스크립트 문법에 타입을 가미한 것이 바로 타입스크립트라 할 수 있다. 자바스크립트는 동적 타입의 언어이기 떄문에 대부분의 에러를 코드를 실행했을 때만 확인 할 수 있다.
