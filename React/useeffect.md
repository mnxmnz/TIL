# 💡 들어가며

> [벨로퍼트와 함께하는 모던 리액트](https://react.vlpt.us/)의 [useEffect를 사용하여 마운트/언마운트/업데이트시 할 작업 설정하기](https://react.vlpt.us/basic/16-useEffect.html)를 읽고 정리한 글입니다.
>
> useEffect 라는 Hook 을 사용하여 컴포넌트가 마운트 됐을 때 (처음 나타났을 때), 언마운트 됐을 때 (사라질 때), 그리고 업데이트될 때 (특정 props가 바뀔 때) 특정 작업을 처리하는 방법에 대해 정리했습니다.

<br>

<hr>

<br>

## 👀 useEffect 사용 방법

> useEffect 를 사용할 때에는 첫 번째 파라미터에는 함수, 두 번째 파라미터에는 의존 값이 들어있는 배열 (deps)을 넣습니다.
>
> 만약에 deps 배열을 비우게 된다면, 컴포넌트가 처음 나타날 때만 useEffect 에 등록한 함수가 호출됩니다.
>
> useEffect 에서는 함수를 반환할 수 있는데 이를 cleanup 함수라고 부릅니다.
>
> deps 가 비어있을 때는 컴포넌트가 사라질 때 cleanup 함수가 호출됩니다.

<br>

<hr>

<br>

### 😎 마운트 / 언마운트

> 첫 번째 파라미터로 console.log('컴포넌트가 화면에 나타남'); 를 포함하고 있는 함수를 작성하고 두 번째 파라미터에는 빈 배열을 deps 로 작성했습니다.
>
> 그리고 useEffect 에서 반환하는 cleanup 함수로 console.log('컴포넌트가 화면에서 사라짐'); 를 포함하고 있는 함수를 작성했습니다.

```jsx
useEffect(() => {
  console.log('컴포넌트가 화면에 나타남');

  // cleanup 함수
  return () => {
    console.log('컴포넌트가 화면에서 사라짐');
  };
}, []);
```

### 🙄 마운트에 사용하는 작업

> 1. props 로 받은 값을 컴포넌트의 로컬 상태로 설정
>
> 2. 외부 API 요청 (REST API 등)
>
> 3. 라이브러리 사용
>
> 4. setInterval 을 통한 반복 작업 혹은 setTimeout 을 통한 작업 예약

### 😥 언마운트에 사용하는 작업

> setInterval, setTimeout 을 사용하여 등록한 작업들 clear 하기 (clearInterval, clearTimeout)
>
> 라이브러리 인스턴스 제거

#### 🔎 UserList.js Code

```jsx
import React, { useEffect } from 'react';

function User({ user, onRemove, onToggle }) {
  useEffect(() => {
    console.log('컴포넌트가 화면에 나타남');

    return () => {
      console.log('컴포넌트가 화면에서 사라짐');
    };
  }, []);

  return (
    <div>
      <b
        style={{
          cursor: 'pointer',
          color: user.active ? 'green' : 'black',
        }}
        onClick={() => onToggle(user.id)}
      >
        {user.username}
      </b>
      &nbsp;
      <span>({user.email})</span>
      <button onClick={() => onRemove(user.id)}>삭제</button>
    </div>
  );
}

function UserList({ users, onRemove, onToggle }) {
  return (
    <div>
      {users.map(user => (
        <User user={user} key={user.id} onRemove={onRemove} onToggle={onToggle} />
      ))}
    </div>
  );
}

export default UserList;
```

#### 💻 실행 결과

![img](https://images.velog.io/images/mnz/post/65c727ac-f7eb-441e-aae4-4d64789f32e4/kBFMe8m.png)

<br>

<hr>

<br>

## ⭐ deps 에 특정 값 넣기

> deps 에 특정 값을 넣게 된다면 컴포넌트가 처음 마운트 될 때도 호출이 되고 지정한 값이 바뀔 때도 호출이 됩니다.
>
> 그리고 deps 안에 특정 값이 있다면 언마운트시에도 호출이 되고 값이 바뀌기 직전에도 호출이 됩니다.
>
> useEffect 안에서 사용하는 상태나 props 가 있다면 useEffect 의 deps 에 작성해야 useEffect 에 등록한 함수가 실행될 때 useEffect 안에서 사용하는 상태가 최신 props / 상태를 가리키게 됩니다.

<br>

<hr>

<br>

### 🧐 useEffect 안에서 사용하는 상태, props 작성하기

> 첫 번째 파라미터로 console.log('user 값이 설정됨'); 를 포함하고 있는 함수를 작성하고 두 번째 파라미터에는 user 를 deps 로 작성했습니다.
>
> 그리고 useEffect 에서 반환하는 cleanup 함수로 console.log('user 가 바뀌기 전..'); 를 포함하고 있는 함수를 작성했습니다.
>
> useEffect 에서 user 라는 props 를 사용하고 있어서 deps 에 user 를 작성했습니다.

```jsx
useEffect(() => {
  console.log('user 값이 설정됨');
  console.log(user);

  return () => {
    console.log('user 가 바뀌기 전..');
    console.log(user);
  };
}, [user]);
```

#### 🔎 UserList.js Code

```jsx
import React, { useEffect } from 'react';

function User({ user, onRemove, onToggle }) {
  useEffect(() => {
    console.log('user 값이 설정됨');
    console.log(user);

    return () => {
      console.log('user 가 바뀌기 전..');
      console.log(user);
    };
  }, [user]);
  return (
    <div>
      <b
        style={{
          cursor: 'pointer',
          color: user.active ? 'green' : 'black',
        }}
        onClick={() => onToggle(user.id)}
      >
        {user.username}
      </b>
      &nbsp;
      <span>({user.email})</span>
      <button onClick={() => onRemove(user.id)}>삭제</button>
    </div>
  );
}

function UserList({ users, onRemove, onToggle }) {
  return (
    <div>
      {users.map(user => (
        <User user={user} key={user.id} onRemove={onRemove} onToggle={onToggle} />
      ))}
    </div>
  );
}

export default UserList;
```

#### 💻 실행 결과

![img](https://images.velog.io/images/mnz/post/dba1730a-7e9b-453e-987c-cf2564ab0c34/d2oJt9L.png)

<br>

<hr>

<br>

# 👏 마치며

> useEffect 라는 Hook 을 사용하여 컴포넌트가 마운트 됐을 때 (처음 나타났을 때), 언마운트 됐을 때 (사라질 때), 그리고 업데이트될 때 (특정 props가 바뀔 때) 특정 작업을 처리하는 방법에 대해 정리했습니다.
>
> 배열 관련 추가 내용은 [useEffect를 사용하여 마운트/언마운트/업데이트시 할 작업 설정하기](https://react.vlpt.us/basic/16-useEffect.html) 링크에서 확인하실 수 있습니다.
