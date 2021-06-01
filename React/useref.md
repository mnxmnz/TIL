# 💡 들어가며

> React 의 Hook 함수 중 하나인 useRef 에 대해 정리했습니다 🤔
>
> useRef 개념에 대해 알아본 후 활용 예제를 통해 실제 useRef 를 적용한 코드에 대해 살펴보겠습니다.

<br>

<hr>

<br>

## 1. useRef 가 무엇인가요? 📑

![useRef](https://drek4537l1klr.cloudfront.net/larsen3/v-3/Figures/06_img_0004.png)

> useRef는 .current 프로퍼티로 전달된 인자(initialValue)로 초기화된 변경 가능한 **ref 객체를 반환**합니다.
>
> 반환된 객체는 컴포넌트의 전 생애주기를 통해 유지될 것입니다.
>
> 본질적으로 useRef는 **.current 프로퍼티에 변경 가능한 값을 담고 있는 상자**와 같습니다.

React 공식 문서에 적혀 있는 useRef 정의입니다.

useRef 사용 예제를 통해 위 정의에 대해 자세히 알아보겠습니다.

<br>

<hr>

<br>

## 2. useRef 를 언제 사용하나요? 🤨

### 2-1. 특정 DOM 선택하기 🎯

![JS-DOM](https://learnjavascriptfast.com/wp-content/uploads/2017/12/Javascript-DOM-1.png)

JavaScript 를 사용할 때는, 특정 DOM 을 선택해야 하는 상황에 getElementById, querySelector 같은 DOM Selector 함수를 사용해서 DOM 을 선택합니다.

> 리액트에서는 **DOM 을 선택할 때 ref 를 사용**합니다.
>
> **함수형 컴포넌트**에서 ref 를 사용할 때는 **useRef** 라는 Hook 함수를 사용하고 클래스형 컴포넌트에서는 콜백 함수를 사용하거나 React.createRef 라는 함수를 사용합니다.

이 글에서는 함수형 컴포넌트의 useRef 를 사용합니다.

### 2-2. 컴포넌트 안의 변수 만들기 🔢

컴포넌트 안에서 조회 및 수정할 수 있는 변수를 관리할 수 있습니다.

> **useRef 로 관리하는 변수는 값이 바뀐다고 해서 컴포넌트가 리렌더링되지 않습니다.**
>
> 리액트 컴포넌트에서의 상태는 상태를 바꾸는 함수를 호출하고 나서 그다음 렌더링 이후로 업데이트된 상태를 조회할 수 있지만, **useRef 로 관리하는 변수는 설정 후 바로 조회**할 수 있습니다.

이 변수를 사용하여 다음과 같은 값을 관리할 수 있습니다.

- setTimeout, setInterval 을 통해서 만들어진 id
- 외부 라이브러리를 사용하여 생성된 인스턴스
- scroll 위치

### 2-3. 리렌더링 방지하기 🏃‍♀️

컴포넌트가 렌더링 된다는 것은 함수(컴포넌트)를 호출하여 실행되는 것을 말합니다.

함수가 실행될 때마다 내부에 선언되어 있던 표현식(변수나 또 다른 함수 등)도 매번 다시 선언되어 사용합니다.

> 컴포넌트는 **자신의 state가 변경**되거나, **부모에게서 받는 props가 변경**되었을 때마다 **리렌더링** 됩니다.

심지어 하위 컴포넌트에 최적화 설정을 해주지 않으면 부모에게서 받는 props가 변경되지 않았더라도 리렌더링 됩니다.

> useRef 로 관리하는 변수는 값이 바뀐다고 해서 컴포넌트가 리렌더링되지 않으므로 **리렌더링 방지에 활용**할 수 있습니다.

<br>

<hr>

<br>

## 3. useRef 로 DOM 선택하는 방법 📢

> **useRef()** 를 사용하여 **Ref 객체**를 만들고, 이 **객체를 선택하고 싶은 DOM 에 ref 값**으로 설정합니다.
>
> 그러면, **Ref 객체의 .current 값**은 **DOM** 을 가리키게 됩니다.

이 포스팅에서 다룰 예제에서는 onReset 함수에서 input 에 포커스를 하는 focus() DOM API 를 호출해주었습니다.

초기화 버튼을 클릭했을 때 이름 input 에 포커스가 잡히도록 useRef 를 사용하여 기능을 구현했습니다.

### 3-1. 객체 생성하기 ✨

> useRef 를 통해 nameInput 이라는 객체를 생성했습니다.

```jsx
const nameInput = useRef();
```

### 3-2. DOM API 사용 💌

> current 가 DOM을 가리키고 있으므로 DOM API 중 하나인 focus 를 사용했습니다.

```jsx
nameInput.current.focus();
```

### 3-3. DOM 설정을 통해 DOM에 직접 접근하기 📲

> nameInput 객체를 선택하고 싶은 DOM 에 설정하여 직접 접근할 수 있습니다.

```jsx
<input name="name" placeholder="이름" onChange={onChange} value={name} ref={nameInput} />
```

### 3-4. 예제 전체 코드 🔍

> InputSample.js Code

```jsx
import React, { useState, useRef } from 'react';

function InputSample() {
  const [inputs, setInputs] = useState({
    name: '',
    nickname: '',
  });

  const nameInput = useRef();

  const { name, nickname } = inputs;

  const onChange = e => {
    const { value, name } = e.target;

    setInputs({
      ...inputs,
      [name]: value,
    });
  };

  const onReset = () => {
    setInputs({
      name: '',
      nickname: '',
    });

    nameInput.current.focus();
  };

  return (
    <div>
      <input name="name" placeholder="이름" onChange={onChange} value={name} ref={nameInput} />
      <input name="nickname" placeholder="닉네임" onChange={onChange} value={nickname} />
      <button onClick={onReset}>초기화</button>
      <div>
        <b>값: </b>
        {name} ({nickname})
      </div>
    </div>
  );
}

export default InputSample;
```

> App.js Code

```jsx
import React from 'react';
import InputSample from './InputSample';

function App() {
  return <InputSample />;
}

export default App;
```

### 3-5. 실행 결과 💻

> 초기화 버튼을 클릭했을 때 이름 input 에 포커스가 잡힙니다.

![img](https://images.velog.io/images/mnz/post/88cd8d00-fd6a-40cc-af74-54422073ce6b/image.png)

<br>

<hr>

<br>

## 4. useRef 로 컴포넌트 안의 변수 만드는 방법 🧙‍♂️

> useRef() 를 사용할 때 파라미터를 넣어주면, 이 값이 **.current 값의 기본값**이 됩니다.
>
> 그리고 이 값을 수정할 때는 .current 값을 수정하면 되고 조회할 때는 .current 를 조회하면 됩니다.

### 4-1. useRef의 파라미터 💭

> 현재 3개의 id 가 있으므로 다음 id 의 기본값을 4로 지정했습니다.

```jsx
const nextId = useRef(4);
```

### 4-2. 예제 전체 코드 🔍

> UserList.js Code

```jsx
import React from 'react';

function User({ user }) {
  return (
    <div>
      <b>{user.username}</b> <span>({user.email})</span>
    </div>
  );
}

function UserList({ users }) {
  return (
    <div>
      {users.map(user => (
        <User user={user} key={user.id} />
      ))}
    </div>
  );
}

export default UserList;
```

> App.js Code

```jsx
import React, { useRef, useState } from 'react';
import UserList from './UserList';

function App() {
  const [inputs, setInputs] = useState({
    username: '',
    email: '',
  });

  const { username, email } = inputs;

  const onChange = e => {
    const { name, value } = e.target;

    setInputs({
      ...inputs,
      [name]: value,
    });
  };

  const [users, setUsers] = useState([
    {
      id: 1,
      username: 'velopert',
      email: 'public.velopert@gmail.com',
    },
    {
      id: 2,
      username: 'tester',
      email: 'tester@example.com',
    },
    {
      id: 3,
      username: 'liz',
      email: 'liz@example.com',
    },
  ]);

  const nextId = useRef(4);

  const onCreate = () => {
    const user = {
      id: nextId.current,
      username,
      email,
    };

    console.log(user.id);

    setUsers(users.concat(user));

    setInputs({
      username: '',
      email: '',
    });

    nextId.current += 1;
  };

  return (
    <>
      <div>
        <input name="username" placeholder="계정명" onChange={onChange} value={username} />
        <input name="email" placeholder="이메일" onChange={onChange} value={email} />
        <button onClick={onCreate}>등록</button>
      </div>
      <UserList users={users} />
    </>
  );
}

export default App;
```

### 4-3. 실행 결과 💻

> id 값으로 nextId.current 를 사용합니다.

![img](https://images.velog.io/images/mnz/post/974b8460-a0bc-4680-9ed7-353b056c8adc/%EB%85%B9%ED%99%94_2021_04_22_21_28_08_421.gif)

<br>

<hr>

<br>

## 5. 리렌더링 방지하는 방법 🤗

위 예시 코드에는 input 값이 변경될 때마다 리렌더링 된다는 단점이 있습니다.

> onChange 구현 부분을 ref 값으로 대체해서 단점을 해결할 수 있습니다.
>
> state 로 event 의 value 에 접근하지 않고 **refObject.current.value 를 사용하는 방법**입니다.

### 5-1. ref 속성 사용하기 ⭐

> input 의 onChange 를 ref 속성으로 대체했습니다.

```jsx
<div>
  <input name="username" placeholder="계정명" ref={usernameRef} />
  <input name="email" placeholder="이메일" ref={emailRef} />
  <button onClick={onCreate}>등록</button>
</div>
```

### 5-2. 예제 전체 코드 🔍

> UserList.js Code

```jsx
import React from 'react';

function User({ user }) {
  return (
    <div>
      <b>{user.username}</b> <span>({user.email})</span>
    </div>
  );
}

function UserList({ users }) {
  return (
    <div>
      {users.map(user => (
        <User user={user} key={user.id} />
      ))}
    </div>
  );
}

export default UserList;
```

> App.js Code

```jsx
import React, { useRef, useState } from 'react';
import UserList from './UserList';
import InputSample from './InputSample';

function App() {
  const [inputs, setInputs] = useState({
    username: '',
    email: '',
  });

  const [users, setUsers] = useState([
    {
      id: 1,
      username: 'velopert',
      email: 'public.velopert@gmail.com',
    },
    {
      id: 2,
      username: 'tester',
      email: 'tester@example.com',
    },
    {
      id: 3,
      username: 'liz',
      email: 'liz@example.com',
    },
  ]);

  const nextId = useRef(4);
  const usernameRef = useRef('');
  const emailRef = useRef('');

  const onCreate = () => {
    const user = {
      id: nextId.current,
      username: usernameRef.current.value,
      email: emailRef.current.value,
    };

    setUsers(users.concat(user));

    setInputs({
      username: '',
      email: '',
    });

    nextId.current += 1;
  };

  console.log('Render');

  return (
    <>
      <div>
        <input name="username" placeholder="계정명" ref={usernameRef} />
        <input name="email" placeholder="이메일" ref={emailRef} />
        <button onClick={onCreate}>등록</button>
      </div>
      <UserList users={users} />
      <InputSample />
    </>
  );
}

export default App;
```

### 5-3. 실행 결과 💻

> 처음 컴포넌트를 불러왔을 때, 등록 버튼을 눌렀을 때 2번만 렌더링 되는 것을 확인할 수 있습니다.

![inputvalue](https://images.velog.io/images/mnz/post/3dca198c-aed8-4387-a4d6-51df8fcbe5f6/%ED%99%94%EB%A9%B4%20%EC%BA%A1%EC%B2%98%202021-05-14%20205808.png)

<br>

<hr>

<br>

# 👏 마치며

> useRef 개념에 대해 알아본 후 대표적인 3가지 활용 방법을 예제와 함께 정리했습니다 🔮
>
> **1. 특정 DOM 선택하기** > **2. 변수 생성하기** > **3. 리렌더링 방지하기**

<br>

<hr>

<br>

#### 참고 자료 📩

[Hooks API Reference](https://ko.reactjs.org/docs/hooks-reference.html#useref)

[Chapter 7: The Javascript Document Object Model](https://learnjavascriptfast.com/chapter-7-the-javascript-document-object-model/2/)

[6 Managing component state with the useRef hook](https://livebook.manning.com/book/react-hooks-in-action/chapter-6/v-3/)

[useRef 로 특정 DOM 선택하기](https://react.vlpt.us/basic/10-useRef.html)

[useRef 로 컴포넌트 안의 변수 만들기](https://react.vlpt.us/basic/12-variable-with-useRef.html)

[배열에 항목 추가하기](https://react.vlpt.us/basic/13-array-insert.html)

[[짤막글] useRef가 뭔가요?](https://velog.io/@kysung95/%EC%A7%A4%EB%A7%89%EA%B8%80-useRef%EA%B0%80-%EB%AD%94%EA%B0%80%EC%9A%94)
