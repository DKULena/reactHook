# useContext
- useContext is a React Hook that allows you access data from any Component without explicitkly passing it down through props at every level. (Prevent Props Drilling).

- useContext is used to manage Global data in the React App.

## Reference ([리액트 공식문서](https://ko.react.dev/reference/react/useContext))
### useContext(SomeContext)
``` jsx
import { useContext } from 'react';

function MyComponent() {
    const theme - useContext(ThemeContext);
    ~~~
}
```


### 매개변수(parameter)
- SomeContext: `createContext`로 생성한 context.
- context 자체는 정보를 담고 있지 않음
- 컴포넌트에서 제공하거나 읽을 수 있는 정보의 종류


### return value
- 호출하는 컴포넌트에 대한 context값을 반환.
- SomeContext.Provider에 전달된 `value`로 결정
- provider가 없으면 반한된 값은 createContext에 전달환 default value.

## 사용법
### 트리 깊은 곳에 데이터 전달
- 컴포넌트의 최상위 수준에서 useContext호출
- context읽고 구독
``` jsx
// Button.jsx
import { useContext } from 'react';

function Button() {
    const theme = useContext(ThemeContext);
    // ...
}
```
- `useContext`는 전달한 `context`에 대한 `context Value`를 반환
- `context`값을 결정하기 위해 <b>상위에서 가장 가까운 `context provider` 탐색</b>

```jsx
function MyPage() {
    return(
        <ThemeContext.Provider value = "dark">
            <Form />
        </ThemeContext.Provider>
    )
}

function Form() {
    // ... 내부에서 버튼 렌더링. ...
}
```

- provider와 `Button`사이에 얼마나 많은 컴포넌트 레이어가 있는지 상관 X
- `Form` 내부의 `Button`이 어디에서나 `useContext(ThemeContext)`를 호출하면 `"dark"`를 값으로 받음

```jsx
import { createContext, useContext } from 'react';

const ThemeContext = createContext(null);

export default function MyApp() {
    return(
        <ThemeContext.Provider value = "dark">
            <Form />
        </ThemeContext.Provider>
    )
}

function Form() {
  return (
    <Panel title="Welcome">
      <Button>Sign up</Button>
      <Button>Log in</Button>
    </Panel>
  );
}

function Panel({ title, children }) {
  const theme = useContext(ThemeContext);
  const className = 'panel-' + theme;
  return (
    <section className={className}>
      <h1>{title}</h1>
      {children}
    </section>
  )
}

function Button({ children }) {
  const theme = useContext(ThemeContext);
  const className = 'button-' + theme;
  return (
    <button className={className}>
      {children}
    </button>
  );
}

```