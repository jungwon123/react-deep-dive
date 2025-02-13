# 모던 리액트 딥 다이브: chapter2 [리액트 개발을 위해 꼭 알아야 할 자바스크립트]

## 2.1 JSX란?

### 2.1.1 JSX의 정의

- JSX는 기본적으로 JSXElement, JSXAttributes, JSXChildren, JSXStrings 라는 4가지 컴포넌트를 기반으로 구성돼 있다.
  -JSXElement: JSX를 구성하는 가장 기본적인 요소로, HTML의 요소와 비슷한 역할을 한다.
  -JSXElementName: JSXElement의 요소 이름으로 쓸 수 있는것을 의미한다.

  - JSXIdentifier: 자바스크립트 식별자 규칙과 돌일하다. <$></$> <_></_> 가능하지만 숫자와 이외의 특수문자는 불가능하다.

  - JSXNamespacedName: JSXIdentifier:JSXIdentifier 의 조합 즉 :을 통해 서로 다른 식별자를 이어주는 것도 하느이 식별자로 취급된다. 두개 이상은 불가능하다.

    ```
    function vaild(){
    return <foo:bar></foo:bar>;
    }

    //불가능
    function invaild(){
    return <foo:bar:baz></foo:bar:baz>;
    }
    ```

  - JSXMemberExpression: 멤버 표현식으로, 객체의 속성에 접근할 때 사용한다.
