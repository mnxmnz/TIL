# 💡 들어가며

> [벨로퍼트와 함께하는 모던 리액트](https://react.vlpt.us/)의 [배열에 항목 추가하기](https://react.vlpt.us/basic/13-array-insert.html), [배열에 항목 제거하기](https://react.vlpt.us/basic/14-array-remove.html)와 [배열 항목 수정하기](https://react.vlpt.us/basic/15-array-modify.html)를 읽고 정리한 글입니다.
>
> React에서 배열 값을 변경할 때 불변성을 유지하는 방법에 대해 정리했습니다.
>
> 배열에서 map 함수를 활용할 때 id 사용 방법과 함수 호출 방식 예제에 대해 작성했습니다.
>
> 사용자의 이름과 이메일을 다루는 예제를 사용했습니다.

<br>

<hr>

<br>

## ⭐ 리액트에서 불변성을 지키는 이유

> 불변성을 지켜주어야만 리액트 컴포넌트에서 상태가 업데이트됐음을 감지할 수 있고 이에 따라 필요한 리렌더링이 진행됩니다.
>
> 기존 상태를 직접 수정하게 되면, 값을 바꿔도 리렌더링이 되지 않습니다.
>
> 리액트에서는 불변성을 지켜주어야만 컴포넌트 업데이트 성능 최적화를 제대로 할 수 있습니다.

<br>

<hr>

<br>

## ✍ 배열 값 추가하기

> **배열 값을 변경**할 때에는 객체와 마찬가지로, **불변성**을 지켜주어야 합니다.
>
> 배열의 push, splice, sort 등의 함수를 사용하면 안 됩니다.
>
> 만약에 사용해야 한다면, 기존의 배열을 한번 복사하고 나서 사용해야 합니다.
>
> **불변성을 지키면서 배열에 새 항목을 추가하는 방법**은 두 가지가 있습니다.
>
> 첫 번째는 **spread 연산자**를 사용하는 것입니다.
>
> 두 번째는 **concat 함수**를 사용하는 것입니다.

<br>

<hr>

<br>

### 🐥 spread 연산자를 사용하기

> 기존의 배열을 수정하지 않고, 새로운 원소가 추가된 새로운 배열을 만들어줍니다.
>
> ...users 를 통해 기존 배열을 복사해서 새 배열을 생성했습니다.
>
> user 는 복사한 배열에 새 항목을 추가하기 위해 작성했습니다.

```jsx
setUsers([...users, user]);
```

### 🔗 concat 함수 사용하기

> concat 에서도 기존의 배열을 수정하지 않고, 새로운 원소가 추가된 새로운 배열을 만들어줍니다.
>
> 여러 개의 배열을 하나의 배열로 합칠 때 사용합니다.
>
> concat 뒤에 값은 배열이 아닌 값도 넣을 수 있습니다.
>
> 새로운 배열을 만든 후 맨 뒤에 user 항목을 추가하는 코드입니다.

```jsx
setUsers(users.concat(user));
```

#### 🔎 App.js Code

> 배열에 새 항목을 추가할 때, 새 항목에서 사용할 고유 id 를 관리하는 용도로 useRef 를 사용했습니다.

```jsx
import React, { useRef, useState } from 'react';
import UserList from './UserList';
import CreateUser from './CreateUser';

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

    setUsers(users.concat(user));

    setInputs({
      username: '',
      email: '',
    });

    nextId.current += 1;
  };

  return (
    <>
      <CreateUser username={username} email={email} onChange={onChange} onCreate={onCreate} />
      <UserList users={users} />
    </>
  );
}

export default App;
```

#### 🔎 CreateUser.js Code

```jsx
import React from 'react';

function CreateUser({ username, email, onChange, onCreate }) {
  return (
    <div>
      <input name="username" placeholder="계정명" onChange={onChange} value={username} />
      <input name="email" placeholder="이메일" onChange={onChange} value={email} />
      <button onClick={onCreate}>등록</button>
    </div>
  );
}

export default CreateUser;
```

#### 💻 실행 결과

![img](https://images.velog.io/images/mnz/post/974b8460-a0bc-4680-9ed7-353b056c8adc/%EB%85%B9%ED%99%94_2021_04_22_21_28_08_421.gif)

<br>

<hr>

<br>

## ❌ 배열 값 제거하기

> 불변성을 지키면서 특정 원소를 배열에서 제거하기 위해서는 filter 배열 내장 함수를 사용합니다.
>
> 이 함수는 배열에서 특정 조건이 만족하는 원소들만 추출하여 새로운 배열을 만들어줍니다.

<br>

<hr>

<br>

### ➰ filter 배열 내장 함수 사용하기

> user.id 값을 onRemove 함수의 파라미터로 넣어서 호출했습니다.
>
> user.id 를 파라미터로 사용하여 일치하지 않는 원소만 추출해서 새로운 배열을 만들었습니다.

```jsx
const onRemove = id => {
  setUsers(users.filter(user => user.id !== id));
};
```

#### 🔎 App.js Code

```jsx
import React, { useRef, useState } from 'react';
import UserList from './UserList';
import CreateUser from './CreateUser';

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

    setUsers(users.concat(user));

    setInputs({
      username: '',
      email: '',
    });

    nextId.current += 1;
  };

  const onRemove = id => {
    setUsers(users.filter(user => user.id !== id));
  };

  return (
    <>
      <CreateUser username={username} email={email} onChange={onChange} onCreate={onCreate} />
      <UserList users={users} onRemove={onRemove} />
    </>
  );
}

export default App;
```

#### 🔎 UserList.js Code

```jsx
import React from 'react';

function User({ user, onRemove }) {
  return (
    <div>
      <b>{user.username}</b> <span>({user.email})</span>
      <button onClick={() => onRemove(user.id)}>삭제</button>
    </div>
  );
}

function UserList({ users, onRemove }) {
  return (
    <div>
      {users.map(user => (
        <User user={user} key={user.id} onRemove={onRemove} />
      ))}
    </div>
  );
}

export default UserList;
```

#### 💻 실행 결과

![img](https://images.velog.io/images/mnz/post/96adefca-8234-48cb-8acf-c9dbe228a00e/%EB%85%B9%ED%99%94_2021_04_22_21_15_53_52.gif)

<br>

<hr>

<br>

## 📝 배열 값 수정하기

> 배열의 불변성을 유지하면서 배열을 업데이트할 때에도 map 함수를 사용할 수 있습니다.

<br>

<hr>

<br>

### 🎡 map 함수로 배열 업데이트 하기

> id 값을 비교해서 id 가 다르다면 그대로 두고, 같다면 active 값을 반전시키도록 구현했습니다.

```jsx
const onToggle = id => {
  setUsers(users.map(user => (user.id === id ? { ...user, active: !user.active } : user)));
};
```

#### 🔎 App.js Code

> User 컴포넌트에 계정명을 클릭했을 때 색상이 초록색으로 바뀌고, 다시 누르면 검은색으로 바뀌는 예제입니다.

```jsx
import React, { useRef, useState } from 'react';
import UserList from './UserList';
import CreateUser from './CreateUser';

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
      active: true,
    },
    {
      id: 2,
      username: 'tester',
      email: 'tester@example.com',
      active: false,
    },
    {
      id: 3,
      username: 'liz',
      email: 'liz@example.com',
      active: false,
    },
  ]);

  const nextId = useRef(4);

  const onCreate = () => {
    const user = {
      id: nextId.current,
      username,
      email,
    };

    setUsers(users.concat(user));

    setInputs({
      username: '',
      email: '',
    });

    nextId.current += 1;
  };

  const onRemove = id => {
    setUsers(users.filter(user => user.id !== id));
  };

  const onToggle = id => {
    setUsers(users.map(user => (user.id === id ? { ...user, active: !user.active } : user)));
  };

  return (
    <>
      <CreateUser username={username} email={email} onChange={onChange} onCreate={onCreate} />
      <UserList users={users} onRemove={onRemove} onToggle={onToggle} />
    </>
  );
}

export default App;
```

#### 🔎 UserList.js Code

```jsx
import React from 'react';

function User({ user, onRemove, onToggle }) {
  const { id, username, email, active } = user;

  return (
    <div>
      <b
        style={{
          color: active ? 'green' : 'black',
          cursor: 'pointer',
        }}
        onClick={() => onToggle(id)}
      >
        {username}
      </b>{' '}
      <span>({email})</span>
      <button onClick={() => onRemove(id)}>삭제</button>
    </div>
  );
}

function UserList({ users, onRemove, onToggle }) {
  return (
    <div>
      {users.map(user => (
        <User key={user.id} user={user} onRemove={onRemove} onToggle={onToggle} />
      ))}
    </div>
  );
}

export default UserList;
```

#### 💻 실행 결과

![img](https://images.velog.io/images/mnz/post/2ba34450-b31f-4319-b154-53d833ab1ce3/%EB%85%B9%ED%99%94_2021_04_22_21_21_10_982.gif)

<br>

<hr>

<br>

# 👏 마치며

> 배열에서 map 함수를 활용할 때 id 사용 방법과 함수 호출 방식에 대해 정리했습니다.
>
> 배열 관련 추가 내용은 [배열에 항목 추가하기](https://react.vlpt.us/basic/13-array-insert.html), [배열에 항목 제거하기](https://react.vlpt.us/basic/14-array-remove.html)와 [배열 항목 수정하기](https://react.vlpt.us/basic/15-array-modify.html) 링크에서 확인하실 수 있습니다.
