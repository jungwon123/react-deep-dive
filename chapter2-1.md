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

  - JSXMemberExpression: JSXIdentifier.JSXIdentifier의 조합, 즉 .을 통해 서로 다른 식별자를 이어주는 것도 하나의 식별자로 취급된다. 두개 이상도 가능하다. 단 JSXNamespacedName과 이어서는 불가능하다.

    ```
    function vaild(){
    return <foo.bar></foo.bar>;
    }


    function vaild2(){
    return <foo.bar.baz></foo.bar.baz>;
    }
    function invaild(){
    return <foo.bar:baz></foo.bar:baz>;
    }

    ```

- JSXAttributes: JSXElement의 속성을 나타낸다. 필수값이 아니고 존재하지 않아고 에러가 나지 않는다.
- JSXSpreadAttributes: 자바스크립트의 전개 연산자와 동일한 역할을 한다고 볼 수 있다.
- JSXAttribute: 속성을 나타내는 키와 값으로 짝을 이루어 표현한다.
- JSXAttributeValue: 속성의 키에 할당할 수 있는 값, 다음 중 하나를 만족해야 한다.

  - 큰따옴표로 구성된 문자열, 작은따옴표료 구성된 문자열, 자바스크립트의 {AssignmentExpression}, JSXElement, JSXFragment, 배열, 함수 호출, 비어있는 표현식

- JSXChildren: JSXElement의 자식 요소를 나타낸다.
- JSXChild: JSXCildren을 이루는 기본 단위이다. 0개이상을 가질 수 있어 없어도 상관없다.
- JSXText: { }, <> 을 제외한 문자열 만약 {},<>를 표현하고 싶다면 문자열료 표시할수있다.

- JSXStrings: HTML에서 사용 가능한 문자열은 모두 JSXStrings에서도 가능하다 문자열이라 함은 "",'',혹은 JSXText를 의미한다.

### 2.1.2 JSX 예제

```
// A는 사용자가 정의한 컴포넌트
const ComponentA = <A>안녕하세요</A>

// 이렇게 정의되어 있을 것입니다
function A({ children }) {
  return <div>{children}</div>
}

//자식이 없이 닫혀 있는 형태도 가능하다.
const ComponentB = <A />

// 옵션을 {}와 전개 연산자로 넣을 수 있다.
const ComponentC = <A {...{a:1,b:2}} />
// 1. 기본적인 props 전달
function Button({ color, text }) {
  return (
    <button style={{ backgroundColor: color }}>
      {text}
    </button>
  );
}

// 사용
const MyButton = <Button color="blue" text="클릭하세요" />


// 2. 여러 방식의 props 전달
function UserProfile({ name, age, isAdmin, ...otherProps }) {
  return (
    <div className="profile">
      <h2>{name}</h2>
      <p>나이: {age}</p>
      {isAdmin && <span>관리자</span>}
    </div>
  );
}

// 일반적인 방식
const Profile1 = <UserProfile name="김철수" age={25} isAdmin={true} />

// 객체 전개 연산자 사용
const userProps = {
  name: "김영희",
  age: 30,
  isAdmin: false
}
const Profile2 = <UserProfile {...userProps} />


// 3. children props
function Card({ title, children }) {
  return (
    <div className="card">
      <h3>{title}</h3>
      <div className="card-content">
        {children}
      </div>
    </div>
  );
}

// 사용
const MyCard = (
  <Card title="안내">
    <p>이것은 카드의 내용입니다.</p>
    <button>확인</button>
  </Card>
)

//속성만 넣어도 가능하다.
const ComponentD = <A required />

// 속성에 함수를 넣을 수 있다.
const ComponentE = <A onClick={() => alert('클릭')} />

// 속성에 배열을 넣을 수 있다.
const ComponentF = <A list={[1,2,3]} />

// 속성과 속성값을 넣을 수 있다.
const ComponentG = <A required={true} />

// 속성에 객체를 넣을 수 있다.
const ComponentH = <A style={{color:'red'}} />

```

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

  - JSXMemberExpression: JSXIdentifier.JSXIdentifier의 조합, 즉 .을 통해 서로 다른 식별자를 이어주는 것도 하나의 식별자로 취급된다. 두개 이상도 가능하다. 단 JSXNamespacedName과 이어서는 불가능하다.

    ```
    function vaild(){
    return <foo.bar></foo.bar>;
    }


    function vaild2(){
    return <foo.bar.baz></foo.bar.baz>;
    }
    function invaild(){
    return <foo.bar:baz></foo.bar:baz>;
    }

    ```

- JSXAttributes: JSXElement의 속성을 나타낸다. 필수값이 아니고 존재하지 않아고 에러가 나지 않는다.
- JSXSpreadAttributes: 자바스크립트의 전개 연산자와 동일한 역할을 한다고 볼 수 있다.
- JSXAttribute: 속성을 나타내는 키와 값으로 짝을 이루어 표현한다.
- JSXAttributeValue: 속성의 키에 할당할 수 있는 값, 다음 중 하나를 만족해야 한다.

  - 큰따옴표로 구성된 문자열, 작은따옴표료 구성된 문자열, 자바스크립트의 {AssignmentExpression}, JSXElement, JSXFragment, 배열, 함수 호출, 비어있는 표현식

- JSXChildren: JSXElement의 자식 요소를 나타낸다.
- JSXChild: JSXCildren을 이루는 기본 단위이다. 0개이상을 가질 수 있어 없어도 상관없다.
- JSXText: { }, <> 을 제외한 문자열 만약 {},<>를 표현하고 싶다면 문자열료 표시할수있다.

- JSXStrings: HTML에서 사용 가능한 문자열은 모두 JSXStrings에서도 가능하다 문자열이라 함은 "",'',혹은 JSXText를 의미한다.

### 2.1.2 JSX 예제

```
// A는 사용자가 정의한 컴포넌트
const ComponentA = <A>안녕하세요</A>

// 이렇게 정의되어 있을 것입니다
function A({ children }) {
  return <div>{children}</div>
}

//자식이 없이 닫혀 있는 형태도 가능하다.
const ComponentB = <A />

// 옵션을 {}와 전개 연산자로 넣을 수 있다.
const ComponentC = <A {...{a:1,b:2}} />
// 1. 기본적인 props 전달
function Button({ color, text }) {
  return (
    <button style={{ backgroundColor: color }}>
      {text}
    </button>
  );
}

// 사용
const MyButton = <Button color="blue" text="클릭하세요" />


// 2. 여러 방식의 props 전달
function UserProfile({ name, age, isAdmin, ...otherProps }) {
  return (
    <div className="profile">
      <h2>{name}</h2>
      <p>나이: {age}</p>
      {isAdmin && <span>관리자</span>}
    </div>
  );
}

// 일반적인 방식
const Profile1 = <UserProfile name="김철수" age={25} isAdmin={true} />

// 객체 전개 연산자 사용
const userProps = {
  name: "김영희",
  age: 30,
  isAdmin: false
}
const Profile2 = <UserProfile {...userProps} />


// 3. children props
function Card({ title, children }) {
  return (
    <div className="card">
      <h3>{title}</h3>
      <div className="card-content">
        {children}
      </div>
    </div>
  );
}

// 사용
const MyCard = (
  <Card title="안내">
    <p>이것은 카드의 내용입니다.</p>
    <button>확인</button>
  </Card>
)

//속성만 넣어도 가능하다.
const ComponentD = <A required />

// 속성에 함수를 넣을 수 있다.
const ComponentE = <A onClick={() => alert('클릭')} />

// 속성에 배열을 넣을 수 있다.
const ComponentF = <A list={[1,2,3]} />

// 속성과 속성값을 넣을 수 있다.
const ComponentG = <A required={true} />

// 속성에 객체를 넣을 수 있다.
const ComponentH = <A style={{color:'red'}} />

```
