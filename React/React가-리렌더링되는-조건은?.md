# React가 리렌더링되는 조건은?

React가 똑똑한 이유 중 하나는 **필요한 부분만 똑똑하게 업데이트하는 방식** 때문입니다.  
React는 변경이 발생하면 **가상 DOM(Virtual DOM)** 을 이용해 이전과 현재를 비교하고,  
**실제 DOM에는 필요한 부분만 최소한으로 반영**합니다.

> 이 방식은 전체 DOM을 새로 그리는 것보다 훨씬 효율적이고 빠르며, 사용자 경험도 향상됩니다.
<br/>

## ⚙️ React의 렌더링 흐름

1. 상태 변화나 props 변경이 발생합니다.
2. 새로운 가상 DOM 트리를 생성합니다.
3. 이전 가상 DOM과 비교(diff)합니다.
4. 변경된 부분만 찾아서 실제 DOM에 최소한으로 반영합니다.
<br/>

## 🔎 리렌더링이 발생하는 조건

컴포넌트가 언제 리렌더링되는지 이해하는 것은 **성능 최적화, 디버깅**에 매우 중요합니다.

1. **state가 변경되었을 때**
```jsx
function Counter() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>{count}</p>
      <button onClick={() => setCount(count + 1)}>증가</button>
    </div>
  );
}
```
- 컴포넌트 내부에서 `seState`, `useReducer` 등으로 관리되는 state가 변경되면 해당 컴포넌트는 리렌더링됩니다.
- 단, 값이 같으면 리렌더링이 발생하지 않습니다.

<br/>

2. **props가 변경되었을 때**
```jsx
function Parent() {
  const [text, setText] = useState("Hello");
  return <Child message={text} />;
}

function Child({ message }: { message: string }) {
  return <p>{message}</p>;
}
```
- 부모로부터 전달받은 `props`가 변경되면 자식 컴포넌트는 리렌더링됩니다.

<br/>

3. **context의 값이 변경되었을 때**
```jsx
const ThemeContext = createContext("light");

function Component() {
  const theme = useContext(ThemeContext);
  return <div className={`theme-${theme}`}>...</div>;
}
```
- `useContext`로 구독 중인 값이 변경되면, 해당 컴포넌트는 리렌더링됩니다.
- ThemeContext.Provider의 `value`가 변경되면 모든 구독 컴포넌트가 다시 렌더링됩니다.

<br/>

4. **부모 컴포넌트가 리렌더링될 때**
```jsx
function Parent() {
  const [count, setCount] = useState(0);
  return (
    <>
      <Child />
      <button onClick={() => setCount(count + 1)}>클릭</button>
    </>
  );
}
```
- 부모 컴포넌트가 리랜더링 되면, 기본적으로 자식 컴포넌트도 리렌더링 됩니다.
- props가 없어도 리렌더링 되기 때문에, 방지하기위해 `React.memo` 등의 최적화가 필요합니다.

<br/>

### 리랜더링 방지 또는 최적화 방법

리렌더링이 무조건 나쁜 건 아니지만,
불필요한 리렌더링은 **성능 저하와 렌더 낭비**를 유발할 수 있습니다.

1. `React.memo(Component)` : 부모 컴포넌트에서 받은 props가 변경되지 않는다면, 리렌더링이 되지 않습니다. 
2. `useMemo()` : 계산된 결과를 캐싱하여 재 계산을 방지 합니다. 
3. `useCallback()` : 함수를 메모리제이션 해서, 동일한 참조값을 유지하도록 합니다.
4. `useRef()` : 리렌더링에 관계 없이 값을 저장하고 참조하는 기능을 합니다. 사용에 주의해야 합니다.

💡 단, useRef()는 렌더링과 무관하게 데이터를 유지하는 용도로 사용되며, 렌더링과 직접 연결되면 안 됩니다.

<br/>

## 🗣️ What-ziny-say

React는 상태(state), props, context, 또는 부모의 렌더링이 변경되면 컴포넌트를 다시 렌더링합니다.<br/>
하지만 **가상 DOM을 통해 변경 전후를 비교(diff)**하고, <br/>
실제 DOM에는 필요한 부분만 업데이트하기 때문에 성능이 좋습니다.

불필요한 리렌더링을 막기 위해선 React.memo, useMemo, useCallback 등의 최적화 도구를 적절히 사용하는 것이 중요합니다.

