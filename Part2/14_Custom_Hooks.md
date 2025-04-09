# 🧩 커스텀 훅 (Custom Hook)

## ✅ 커스텀 훅이란?

React에서 **커스텀 훅(Custom Hook)** 은 `use`로 시작하는 **사용자 정의 훅 함수**입니다.<br/>
기존의 React 내장 훅(`useState`, `useEffect` 등)을 조합하거나 반복되는 로직을 하나의 함수로 추출하여 **재사용성을 높이고 컴포넌트 코드를 깔끔하게 유지**할 수 있도록 합니다.

> 💡 훅은 오직 함수 컴포넌트나 다른 커스텀 훅 내부에서만 호출되어야 합니다.

---

## 📦 커스텀 훅의 필요성

### 🔁 반복되는 훅 로직을 분리하고 싶을 때

```jsx
// before
useEffect(() => {
  const handler = () => {
    if (window.scrollY > 100) setShow(true);
    else setShow(false);
  };
  window.addEventListener('scroll', handler);
  return () => window.removeEventListener('scroll', handler);
}, []);

// after - useScrollVisible 훅 사용
const isVisible = useScrollVisible(100);
```

### 👇 관심사 분리 (Separation of Concerns)

컴포넌트는 UI에 집중하고, 훅은 비즈니스 로직을 담당하게 되어 코드 가독성 향상

---

## 🛠 자주 사용하는 커스텀 훅 예시

| 훅 이름                      | 설명                                                              |
| ---------------------------- | ----------------------------------------------------------------- |
| `useInput`                   | 입력 필드의 값을 관리하고 유효성 검사를 수행하는 훅               |
| `useTabs`                    | 여러 탭 간의 전환을 관리하는 훅                                   |
| `useFetch`                   | 네트워크 요청을 보내고 응답 데이터를 관리하는 훅                  |
| `useToggle`                  | boolean 상태를 토글하는 로직을 간단하게 관리하는 훅               |
| `useForm`                    | 폼 입력 및 제출 관리를 위한 훅, 유효성 검사와 폼 제출 처리를 포함 |
| `useLocalStorage`            | Local Storage에 데이터를 저장하고 관리하는 훅                     |
| `useEventListener`           | 이벤트 리스너를 추가하고 제거하는 로직을 캡슐화한 훅              |
| `useInterval` / `useTimeout` | setInterval과 setTimeout을 React 생명주기에 맞게 관리하는 훅      |

---

### 예시: `useToggle` - Boolean 상태를 간단하게 토글

```jsx
import { useCallback, useState } from 'react';

export function useToggle(initial = false) {
  const [value, setValue] = useState(initial);
  const toggle = useCallback(() => setValue(v => !v), []);
  return [value, toggle];
}
```

```jsx
const [isOpen, toggleOpen] = useToggle();
<button onClick={toggleOpen}>{isOpen ? '닫기' : '열기'}</button>;
```

---

## 🧩 커스텀 훅과 유틸 함수의 차이점

| 항목                | 커스텀 훅                        | 유틸 함수                              |
| ------------------- | -------------------------------- | -------------------------------------- |
| React Hook 사용     | ✅ (useState, useEffect 등 포함) | ❌ (일반 함수 로직만 포함)             |
| React 생명주기 의존 | 있음                             | 없음                                   |
| 호출 위치           | React 컴포넌트 내부              | 어디서든 호출 가능                     |
| 목적                | 상태 및 사이드이펙트 관리        | 순수 계산, 포맷팅, 변환 등의 로직 처리 |

### 예시 비교

```js
// 커스텀 훅
function useWindowWidth() {
  const [width, setWidth] = useState(window.innerWidth);
  useEffect(() => {
    const handleResize = () => setWidth(window.innerWidth);
    window.addEventListener('resize', handleResize);
    return () => window.removeEventListener('resize', handleResize);
  }, []);
  return width;
}

// 유틸 함수
function formatDate(date) {
  return date.toLocaleDateString('ko-KR');
}
```

---

## 🧭 커스텀 훅과 유틸 함수 분류 기준

### 커스텀 훅으로 만드는 것이 적절한 경우

- `useState`, `useEffect`, `useRef` 등 훅을 사용하는 로직일 때
- React 생명주기와 관련된 이벤트나 동작 처리
- 컴포넌트의 UI 상태를 다루는 경우

### 유틸 함수로 분리하는 것이 적절한 경우

- 순수 함수 (입력 → 출력이 고정)
- 날짜 포맷, 문자열 처리, 숫자 계산 등 단순 로직
- 상태나 라이프사이클과 무관한 공통 로직

> ✅ 일반적으로 훅 내부에서 유틸 함수를 사용할 수 있지만, 유틸 함수 내부에서 훅을 호출해서는 안 됩니다.

---

## 📌 프로젝트에서 커스텀 훅 사용할 때 팁

| 항목   | 설명                                                                           |
| ------ | ------------------------------------------------------------------------------ |
| 파일명 | `useInput.js`, `useFetch.js`처럼 `use`로 시작해야 React가 훅으로 인식함        |
| 위치   | `hooks/` 폴더를 만들어 관리하는 것이 일반적                                    |
| 의존성 | 내부에서 사용하는 `state`, `effect`, `callback` 등에 의존성 배열을 항상 주의   |
| 테스트 | 커스텀 훅은 단위 테스트 작성이 매우 용이함 → 재사용성 있는 로직 중심 작성 권장 |
