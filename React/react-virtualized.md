# 💡 들어가며

> **react-virtualized 를 사용한 Infinity Scroll 최적화에 대해 정리했습니다.**
>
> windowing 기법과 크롬 개발자 도구를 활용한 성능 측정 방법에 대해 먼저 알아본 후 react-virtualized 라이브러리 사용 방법을 소개합니다.
>
> 본 포스팅에서 다루는 To Do List 예제는 [리액트를 다루는 기술](https://github.com/velopert/learning-react) 코드를 사용했습니다.

<br>

<hr>

<br>

## 1. react-virtualized 가 무엇인가요? 🎠

![img](https://images.velog.io/images/mnz/post/df9c8cec-26f2-45b2-a231-9fc16ffa8c27/image.png)

react-virtualized 란 React 공식 문서 [성능 최적화 페이지](https://ko.reactjs.org/docs/optimizing-performance.html)에서 소개하는 두 개의 라이브러리 중 하나입니다.

> react-virtualized 를 널리 알려진 **windowing 라이브러리**라고 소개하고 있습니다.

여기서 windowing 기법이란 무엇일까요?

React 공식 문서에는 이렇게 적혀 있습니다.

> 주어진 시간에 목록의 **부분 목록만 렌더링**하며 컴포넌트를 다시 렌더링하는 데 걸리는 시간과 생성된 DOM 노드의 수를 크게 줄일 수 있습니다.

windowing 기법에 대해 조금 더 자세히 알아보겠습니다.

<br>

<hr>

<br>

### 1-1. windowing 기법 🏄‍♀️

![windowing](https://addyosmani.com/assets/images/react-window.png)

많은 데이터를 list 로 만들고 초기 렌더링에 모든 데이터를 불러오려면 굉장히 오랜 시간이 걸립니다.

사용자로서 긴 렌더링 시간이 불편하게 느껴질 수 있습니다.

> 이를 해결하기 위해서 list 중 **viewport 에 보이는 부분만 렌더링**하고 나머지는 스크롤 할 때 보이도록 하는 것을 권장합니다.

그리고 이 방법을 windowing 기법이라고 합니다.

<br>

<hr>

<br>

### 1-2. react-virtualized 를 선택한 이유 🚩

위에서 react-virtualized 에 대해 작성할 때 React 공식 문서에서 소개하는 두 개의 라이브러리 중 하나라고 했는데, 다른 하나는 react-window 라이브러리입니다.

이 포스팅에서 react-window 가 아닌 react-virtualized 를 다루는 이유는 다음과 같습니다.

![virtualized](https://images.velog.io/images/mnz/post/ef0fbdf3-4382-4099-a357-bb72a43c5ac1/%EB%8B%A4%EC%9A%B4%EB%A1%9C%EB%93%9C.png)

> **react-virtualized 사용자**

위 사진은 최근 6개월 동안 react-virtualized 와 react-window 의 다운로드 횟수를 나타낸 것입니다.

react-virtualized 패키지의 사용자가 더 많은 것을 확인할 수 있습니다.

또한, react-virtualized 에는 react-window 에 없는 기능이 있는데 그중 두 가지는 아래와 같습니다.

> **window 스크롤**

react-window 는 컨테이너의 너비와 높이를 명시적으로 지정하여 해당 컨테이너 내에서만 가상화가 가능합니다.

react-virtualized 의 WindowScroller 컴포넌트를 사용하면 페이스북이나 트위터처럼 **window 스크롤을 기준으로 목록이 가상화되도록 구현**할 수 있습니다.

> **요소의 동적인 height 측정**

콘텐츠형 광고 페이지 내에서 렌더링 되는 요소의 height 값은 요소의 내용에 따라 변하기 때문에 동적인 height 측정할 수 있어야 합니다.

react-window 는 요소의 동적인 height 값을 측정할 방법이 없지만, react-virtualized 에서 CellMeasurer 라는 컴포넌트는 **원하는 요소의 동적인 height 값을 측정**해주는 역할을 담당하고 있습니다.

동적인 height 값에 대해서는 [react-virtualized](http://bvaughn.github.io/react-virtualized/#/components/List) 링크에서 확인할 수 있습니다.

> 그러나 위 사진의 size 항목에서 확인할 수 있는 거처럼 react-window 가 react-virtualized 보다 훨씬 작아서 **추가적인 기능을 사용하지 않는다면 react-window 사용을 권장**하기도 합니다.

<br>

<hr>

<br>

## 2. 크롬 개발자 도구로 성능 측정하는 방법 💫

성능을 분석해야 할 때는 느려졌다는 느낌이 아니라 정확히 몇 초가 걸리는지 확인이 필요합니다.

크롬 개발자 도구의 Performance 탭으로 이를 측정할 수 있습니다.

![performance](https://images.velog.io/images/mnz/post/2ef83db4-a3a4-4f9f-be08-71fbcdb5d085/%ED%99%94%EB%A9%B4%20%EC%BA%A1%EC%B2%98%202021-05-30%20202801.png)

녹화 버튼을 클릭한 후, 할 일 목록에 체크하고 화면에 변화가 반영되면 Stop 버튼을 누릅니다.

> 성능 분석 결과에 나타나는 **Timings** 를 열어 보면 **각 시간대에 컴포넌트의 어떤 작업이 처리되었는지 확인**할 수 있습니다.

App [update] 박스를 열어 작업 처리 시간을 확인했습니다.

2500개의 데이터가 있는데 1.4초가 걸린다는 것은 성능이 매우 나쁘다는 의미입니다.

최적화를 한 이후 이 성능을 비교해보겠습니다.

<br>

<hr>

<br>

## 3. react-virtualized 사용 방법 🚀

### 3-1. 패키지 설치 💾

우선 yarn 을 통해 react-virtualized 를 설치하겠습니다.

> **yarn add react-virtualized**

이제 react-virtualized 에서 제공하는 List 컴포넌트를 사용하여 To Do List 컴포넌트 성능 최적화를 하겠습니다.

<br>

<hr>

<br>

### 3-2. 각 항목의 px 단위 측정하기 🎨

최적화를 수행하기 전에 필요한 작업은 각 항목의 실제 크기를 px 단위로 알아내는 것입니다.

**크롬 개발자 도구**를 통해 이 크기를 쉽게 알아낼 수 있습니다.

![px](https://images.velog.io/images/mnz/post/1a645131-2bbe-4d51-ab25-5222a19087d4/%ED%99%94%EB%A9%B4%20%EC%BA%A1%EC%B2%98%202021-05-30%20195109.png)

각 항목의 크기는 가로 495px, 세로 57px 입니다.

여기서 첫 번째 항목이 아니라 두 번째 항목을 확인하는 이유는 테두리가 포함된 값을 알아내기 위해서입니다.

<br>

<hr>

<br>

### 3-3. 코드 수정하기 🧙‍♂️

> **예시 코드 구조**

```text
src
 ┣ components
 ┃ ┣ TodoInsert.js
 ┃ ┣ TodoInsert.scss
 ┃ ┣ TodoList.js
 ┃ ┣ TodoList.scss
 ┃ ┣ TodoListItem.js
 ┃ ┣ TodoListItem.scss
 ┃ ┣ TodoTemplate.js
 ┃ ┗ TodoTemplate.scss
 ┣ App.css
 ┣ App.js
 ┣ App.test.js
 ┣ index.css
 ┣ index.js
 ┣ logo.svg
 ┗ serviceWorker.js
```

> **수정 전 To Do List 코드**

```jsx
import React from 'react';
import TodoListItem from './TodoListItem';
import './TodoList.scss';

const TodoList = ({ todos, onRemove, onToggle }) => {
  return (
    <div className="TodoList">
      {todos.map(todo => (
        <TodoListItem todo={todo} key={todo.id} onRemove={onRemove} onToggle={onToggle} />
      ))}
    </div>
  );
};

export default TodoList;
```

> **react-virtualized 를 적용한 To Do List 코드**

- useCallback 과 memo 로 최적화를 한 이후에 react-virtualized 를 적용한 코드입니다.

- react-virtualized 관련 내용은 **return 문의 List 컴포넌트**입니다.

```jsx
import React, { useCallback } from 'react';
import { List } from 'react-virtualized';
import TodoListItem from './TodoListItem';
import './TodoList.scss';

const TodoList = ({ todos, onRemove, onToggle }) => {
  const rowRenderer = useCallback(
    ({ index, key, style }) => {
      const todo = todos[index];
      return <TodoListItem todo={todo} key={key} onRemove={onRemove} onToggle={onToggle} style={style} />;
    },
    [onRemove, onToggle, todos],
  );
  return (
    <List
      className="TodoList"
      width={495} // 전체 크기
      height={513} // 전체 높이
      rowCount={todos.length} // 항목 개수
      rowHeight={57} // 항목 높이
      rowRenderer={rowRenderer} // 항목을 렌더링할 때 쓰는 함수
      list={todos} // 배열
      style={{ outline: 'none' }} // List에 기본 적용되는 outline 스타일 제거
    />
  );
};

export default React.memo(TodoList);
```

List 컴포넌트를 사용하기 위해 rowRenderer 라는 함수를 새로 작성했습니다.

이 함수는 react-virtualized 의 List 컴포넌트에서 각 To Do Item 을 렌더링할 때 사용하며, 이 함수를 List 컴포넌트의 props 값으로 설정해야 합니다.

이 함수는 파라미터에 index, key, style 값을 객체 타입으로 받아 와서 사용합니다.

> List 컴포넌트를 사용할 때는 해당 리스트의 **전체 크기**와 **각 항목의 높이**, **각 항목을 렌더링할 때 사용해야 하는 함수**, 그리고 **배열**을 props 로 넣어 주어야 합니다.

그러면 이 컴포넌트가 전달받은 props 를 사용하여 자동으로 최적화를 해줍니다.

<br>

<hr>

<br>

### 3-4. 최적화 이후 성능 비교 ✨

![ms](https://images.velog.io/images/mnz/post/e3123f49-d18f-46b3-98a8-0d9e8e40980a/%ED%99%94%EB%A9%B4%20%EC%BA%A1%EC%B2%98%202021-05-30%20203601.png)

> **최적화 이전에는 1.4초가 걸렸지만, 최적화 이후 11ms(0.011초) 가 걸리는 것을 확인할 수 있습니다. 👍**

현재 다룬 예시는 react-virtualized 만 적용했을 때 최적화 성능은 아니지만, 다른 예시를 통해 최적화 적용 순서대로 성능을 측정했을 때는 다음과 같습니다.

- 최적화 적용 전 코드: 1.02초

- React.memo 적용 코드: 0.059초

- react-virtualized 적용 코드: 0.005초

<br>

<hr>

<br>

# 👏 마치며

> react-virtualized 사용 방법에 대해 알아봤습니다.
>
> 리액트 컴포넌트의 렌더링은 기본적으로 빨라서 컴포넌트를 개발할 때 최적화 작업에 대해 너무 큰 스트레스를 받거나 모든 컴포넌트에 일일이 React.memo 를 작성할 필요는 없습니다.
>
> **단, 리스트와 관련된 컴포넌트를 만들 때 보여 줄 항목이 100개 이상이고 업데이트가 자주 발생한다면, 최적화가 필요합니다.**

<br>

<hr>

<br>

#### 참고 자료 📩

- [Optimize Your Code by using React Virtualized](https://yudhajitadhikary.medium.com/optimize-your-code-by-using-react-virtualized-a9c814031c27)

- [성능 최적화](https://ko.reactjs.org/docs/optimizing-performance.html)

- [React Windowing](https://velog.io/@pandati0710/React-Windowing)

- [리액트를 다루는 기술](https://book.naver.com/bookdb/book_detail.nhn?bid=15372757)

- [windowing 라이브러리](https://maybe-b50.tistory.com/67)

- [Optimizing Performance](https://jooonho.com/react/2020-10-18-optimizing-performance/)

- [Rendering large lists with react-window](https://addyosmani.com/blog/react-window/)

- [[번역] React의 성능을 최적화하는 10가지 방법](https://uzihoon.com/post/ef453fd0-ab14-11ea-98ac-61734eebc216)

- [react-virtualized 를 활용한 렌더링 성능 최적화](https://velog.io/@kimjh96/react-virtualized-렌더링-성능-최적화)
