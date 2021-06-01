# 💡 들어가며

> [벨로퍼트와 함께하는 모던 리액트](https://react.vlpt.us/)의 [props 를 통해 컴포넌트에게 값 전달하기](https://react.vlpt.us/basic/05-props.html)를 읽고 정리한 글입니다.
>
> props 의 정의와 defaultProps 및 props.children 적용 코드에 대해 정리했습니다.

<br>

<hr>

<br>

## 📝 props의 개념

> properties 의 줄임말입니다.
>
> 어떠한 값을 컴포넌트에게 전달해줘야 할 때, props 를 사용합니다.

<br>

<hr>

<br>

### 🎯 defaultProps로 기본값 설정하기

> 컴포넌트에 props 를 지정하지 않았을 때 기본적으로 사용할 값을 설정할 수 있습니다.

#### 🔎 Hello.js Code

> color 와 name props 를 사용하는 코드 예시입니다.
>
> Hello.defaultProps 를 통해 name 이라는 props 의 기본값을 "이름 없음"으로 설정했습니다.

```jsx
import React from 'react';

function Hello({ color, name }) {
  return <div style={{ color }}>{name}님 안녕하세요</div>;
}

Hello.defaultProps = {
  name: '이름 없음',
};

export default Hello;
```

#### 🔎 App.js Code

> 첫 번째 Hello 컴포넌트는 name 과 color 값을 모두 설정했지만
>
> 두 번째 Hello 컴포넌트는 color 값만 설정했습니다.

```jsx
import React from 'react';
import Hello from './Hello';

function App() {
  return (
    <>
      <Hello name="React" color="blue" />
      <Hello color="green" />
    </>
  );
}

export default App;
```

#### 💻 실행 결과

> 첫 번째 Hello 컴포넌트는 직접 설정한 "React" name props 값을,
>
> 두 번째 Hello 컴포넌트는 defaultProps 를 통해 설정한 "이름 없음"을 확인할 수 있습니다.

![default-props-img](https://images.velog.io/images/mnz/post/9fd95f8a-e173-4aac-b6b3-6e1380cb6f1f/%ED%99%94%EB%A9%B4%20%EC%BA%A1%EC%B2%98%202021-04-18%20030552.png)

<br>

<hr>

<br>

### 🎯 props.children로 컴포넌트 태그 사잇값 조회하기

> 컴포넌트 태그 사이에 넣은 값을 조회하고 싶을 때, props.children 을 사용합니다.

#### 🔎 Wrapper.js Code

> 위에서 만든 Hello 컴포넌트를 감싸기 위해 Wrapper 컴포넌트를 생성했습니다.

```jsx
import React from 'react';

function Wrapper() {
  const style = {
    border: '2px solid black',
    padding: 16,
  };

  return <div style={style}></div>;
}

export default Wrapper;
```

#### 🔎 App.js Code

> App 컴포넌트에서 Wrapper 컴포넌트를 사용했습니다.

```jsx
import React from 'react';
import Hello from './Hello';
import Wrapper from './Wrapper';

function App() {
  return (
    <Wrapper>
      <Hello name="React" color="blue" />
      <Hello color="green" />
    </Wrapper>
  );
}

export default App;
```

#### 💻 실행 결과

> 결과는 아래 사진과 같이, Wrapper 태그 내부에 Hello 컴포넌트는 확인할 수 없습니다.

![wrapper-img](https://images.velog.io/images/mnz/post/d7960fc8-3824-486d-af1c-a8f783c6f006/%ED%99%94%EB%A9%B4%20%EC%BA%A1%EC%B2%98%202021-04-18%20033335.png)

#### 🔎 수정 후 Wrapper.js Code

> 내부의 내용이 보이게 하기 위해 Wrapper 컴포넌트에서 props.children 을 렌더링했습니다.

```jsx
import React from 'react';

function Wrapper({ children }) {
  const style = {
    border: '2px solid black',
    padding: 16,
  };

  return <div style={style}>{children}</div>;
}

export default Wrapper;
```

#### 💻 실행 결과

> props.children 을 적용한 후, Hello 컴포넌트를 확인할 수 있습니다.

![wrapper-children-img](https://images.velog.io/images/mnz/post/59c85658-c867-46e9-a869-9eae8aaaab33/%ED%99%94%EB%A9%B4%20%EC%BA%A1%EC%B2%98%202021-04-18%20030639.png)

<br>

<hr>

<br>

# 👏 마치며

> props 관련 내용 중 defaultProps 및 props.children 에 대해 알아봤습니다.
>
> props 관련 추가 내용은 [props 를 통해 컴포넌트에게 값 전달하기](https://react.vlpt.us/basic/05-props.html) 링크에서 확인하실 수 있습니다.
