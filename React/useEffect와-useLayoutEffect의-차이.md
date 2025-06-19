# useEffect와-useLayoutEffect의-차이

React를 사용할 때 useEffect는 매우 자주 사용하지만, <br/>
useLayoutEffect는 상대적으로 덜 익숙할 수 있습니다.<br/>
두 훅 모두 컴포넌트가 마운트되거나 업데이트된 후 실행되지만, <br>
> 가장 중요한 차이점은 바로 **실행 시점의 차이** 입니다.

<br/>

## ✅ 핵심 차이

`useEffect`는 브라우저가 화면을 그린 후 실행됩니다.<br/>
렌더링을 차단하지 않기 때문에 비동기 작업이나 API 호출에 적합합니다.

`useLayoutEffect`는 렌더링이 일어나기 직전에 동기적으로 실행됩니다.<br/>
그래서 DOM의 레이아웃을 측정하거나, 스타일을 강제로 조정해야 하는 경우에 사용됩니다.

<br/>

### ✨ 왜 이런 차이가 중요할까요?

useLayoutEffect는 사용자에게 화면이 그려지기 전에 DOM을 조작할 수 있는 기회를 줍니다. <br/>
반면 useEffect는 화면이 그려진 뒤에 실행되기 때문에, DOM을 측정하거나 조정하는 데 늦을 수 있습니다.

## 예제 코드 비교

***useEffect 예제***
```tsx
import { useEffect } from "react";

function Profile() {
  useEffect(() => {
    fetch('/api/user').then(res => res.json()).then(console.log);
  }, []);

  return <div>사용자 정보를 불러오는 중...</div>;
}
```
- API 호출 등 사용자에게 영향 없는 작업에 사용합니다.
- 화면을 먼저 렌더링한 뒤 데이터를 불러오는 작업입니다.

***useLayoutEffect 예제***
```tsx
import { useLayoutEffect, useRef } from "react";

function AlertBox() {
  const boxRef = useRef<HTMLDivElement>(null);

  useLayoutEffect(() => {
    const width = boxRef.current?.offsetWidth;
    console.log("화면에 그려지기 전 너비:", width);
    if (boxRef.current) {
      boxRef.current.style.transform = "translateX(10px)";
    }
  }, []);

  return <div ref={boxRef}>팝업 내용</div>;
}
```
- DOM 요소의 크기나 위치 측정할때 사용합니다.
- 화면에 깜빡임 없이 조정해야 할 때 적합합니다.

## ⚠️ 주의할 점
```tsx
if (typeof window !== "undefined") {
  useLayoutEffect(() => {
    // 클라이언트에서만 실행
  }, []);
}
```
- useLayoutEffect는 **렌더링을 블로킹** 하기 때문에 성능에 민감한 앱에서는 최소한으로 사용해야 합니다.
- Next.js 같은 SSR 환경에서는 경고가 발생할 수 있으므로 조건부 실행 또는 useEffect 대체가 필요합니다.

## 🗣️ What-ziny-say
useEffect와 uselayoutEffect 모두 **렌더링 후에 실행되는 훅** 입니다.<br/>
하지만 가장 큰 차이점은 실행 시점의 정확한 타이밍입니다.

useEffect는 **브라우저가 화면을 그린 후(PAINT 후)** 비동기로 실행됩니다.<br/>
따라서 API 호출, 이벤트 등록, 로그 출력 등 사용자에게 영향을 주지 않는 비동기 작업에 적합합니다.

반면 useLayoutEffect는 **화면이 그려지기 직전(PAINT 전)** 에 동기적으로 실행됩니다.<br/>
이 시점에는 아직 브라우저가 실제로 화면을 표시하지 않았기 때문에, <br/>
DOM 요소의 크기나 위치를 측정하거나 스타일을 조정해야 할 때 사용하면 화면 깜빡임 없이 자연스럽게 적용할 수 있습니다.

> 사용자에게 보이기 전에 처리해야 하는 작업이면 useLayoutEffect,
> 보인 후 처리해도 괜찮다면 useEffect를 사용하면 됩니다.
