# Suspense와 동작 원리

`Suspense`는 **비동기 컴포넌트의 로딩을 자연스럽게 처리할 수 있도록 도와주는 컴포넌트**입니다.  
즉, 잠깐 기다리는 동안 보여줄 UI를 정의하는 래퍼 컴포넌트라고 할 수 있습니다.
<br/>
## 🧪 기본 사용 예시
```tsx
import React, { Suspense, lazy } from "react";

const LazyComponent = lazy(() => import("./HomePage"));

function App() {
  return (
    <Suspense fallback={<div>로딩 중...</div>}>
      <LazyComponent />
    </Suspense>
  );
}
```
- fallback에는 **로딩 중일 때 보여줄 UI**를 정의합니다.
- lazy()는 컴포넌트를 **코드 스플리팅하여 지연 로딩**합니다.
  
> 코드 스플리팅(Code Splitting): 필요한 코드만 나눠서(스플릿) 로드하고, 지금 필요한 것만 브라우저에 보내주는 방식

<br/>

## ⚙️ Suspense의 동작 원리

단순하게 로딩UI를 보여주는 것 처럼 보이지만 <br/>
내부적으로는 렌더링 중 **일시 정지(pause)** 상태를 감지하고 제어하는 역할을 합니다.

> 렌더링도중 준비되지 않은 컴포넌트나 데이터를 만나면 <br/>
> 렌더링을 잠시 멈추고 가장 가까운 Suspense의 fallbackUI를 보여주게 됩니다.

<br/>

### 🔍 React 내부에서는 이렇게 동작합니다

1. 컴포넌트가 렌더링 중 Promise를 던집니다. (Lazy()로 로드 중인 컴포넌트)
2. React는 Promise가 발생한 것을 감지하고, **가장 가까운 Suspense**를 찾습니다.
3. 해당 Suspense의 fallback UI를 렌더링합니다.
4. Promise가 resolve되면, 중단했던 렌더링을 재시작하며 원래 컴포넌트를 다시 렌더링합니다.

### 💡 핵심 포인트
- React는 렌더링 도중 Promise가 발생하면 렌더링을 중단합니다.
- 이는 기존 ErrorBoundary와 비슷하게 동작하지만,
에러가 아닌 `비동기 상태`도 멈출 수 있는 구조라는 점에서 확장된 개념입니다. 

<br/>

## What-ziny-say

리액트의 Suspense는 lazy로 감싸진 컴포넌트가 로딩 중일 때 보여줄 UI를 정의하는 컴포넌트 입니다. 

보통 로딩 화면이 필요한 컴포넌트를 lazy()로 감싸고,<br/>
그 컴포넌트를 Suspense로 감싼 뒤 fallback에 로딩 UI를 넣어서 처리 합니다.

컴포넌트가 렌더링 중에 promise를 던지면,<br/>
리액트는 promise를 감지하고, 가까운 곳에 있는 suspense를 찾아 fallback을 렌더링하고,
promise가 resolve되고 나면 원래의 컴포넌트를 랜더링 하는 방식입니다.

이런 구조를 가지고 있어, 로딩 상황에서도 사용자 친화적인 사이트를 만들수 있게 됩니다.

리액트는 에러 뿐만아니라 에러가 아닌 비동기 상태에서도 렌더링을 `잠시 멈추고 다시 시작하는` 구조를 가지고 있다는 것을 의미합니다.


