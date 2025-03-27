# ⚛️ React State 업데이트와 Batching

## ❓ 두 코드의 차이 설명

### 📌 첫 번째 예시 (값 기반 업데이트)

```jsx
let prev = 4;
setState(prev + 1);
setState(prev + 1);
setState(prev + 1);
```

- 각 `setState(prev + 1)` 는 `prev`가 4인 상태를 기준으로 동작합니다.
- 따라서 각각 `setState(5)`로 처리되며, 결과적으로 최종 상태는 **5**가 됩니다.
- 이유: `setState`는 **비동기적으로 처리**되며, 이 경우 이전 값을 기준으로 동기화되지 않기 때문입니다.

### 📌 두 번째 예시 (업데이트 함수 기반)

```jsx
let prev = 4;
setState(prev => prev + 1);
setState(prev => prev + 1);
setState(prev => prev + 1);
```

- `setState`에 함수를 전달하면 **최신 state 값을 기반으로 업데이트**하게 됩니다.
- 이 경우에는
  1. 첫 번째 호출 → 5
  2. 두 번째 호출 → 6
  3. 세 번째 호출 → 7
- 최종 상태는 **7**이 됩니다.

### ✅ 결론

- **값 기반 업데이트**: 이전 상태가 정확히 반영되지 않을 수 있음
- **함수 기반 업데이트**: 상태의 최신 값을 보장함 → ✅ **권장 방식**

### 💡 추가 대안: State 없이 해결하는 방식

- 상태를 연속적으로 증가시키는 경우에는 반드시 state를 사용할 필요는 없습니다.
- 예를 들어, **useRef**를 이용하면 렌더링 없이 값을 누적할 수 있습니다.

```jsx
const counter = useRef(4);
counter.current += 1;
counter.current += 1;
counter.current += 1;
console.log(counter.current); // 7
```

- 이런 방식은 불필요한 렌더링을 줄이면서 로직을 단순화하는 데 효과적입니다.

---

## 🧠 React의 Batching이란?

> **Batching**은 여러 개의 상태 업데이트를 하나의 렌더링 사이클로 묶어 처리하는 React의 최적화 전략입니다.

### ✅ 특징

- 여러 `setState()` 호출이 있어도, React는 **렌더링을 최소 한 번만** 수행합니다.
- 렌더링 횟수를 줄여 성능을 향상시킵니다.

### 🧪 예시

```jsx
setCount(count + 1);
setName('Alice');
```

- 위 두 개의 상태 업데이트는 **하나의 렌더링 사이클**에서 처리됩니다.

---

## ⚙️ 자동 배칭 (Automatic Batching)

- React 18부터는 비동기 상황에서도 자동 배칭이 활성화됩니다.
- 예전에는 `setTimeout`, `fetch`, `Promise` 등 비동기 함수 내에서는 상태가 각각 렌더링을 발생시켰지만,
- React 18부터는 다음과 같은 코드도 한 번의 렌더링으로 처리됩니다.

```jsx
setTimeout(() => {
  setCount(c => c + 1);
  setName('Alice');
}, 0);
```

✅ 한 번만 렌더링됨.

---

## 📦 기타 유용한 도구

- `useReducer`를 사용하면 상태 업데이트 로직을 **명시적으로 관리**하고, 배칭과 무관하게 상태를 누적 처리할 수 있습니다.
- `react-tracked`, `recoil`, `zustand` 등의 상태 관리 라이브러리는 내부적으로 배칭을 잘 처리하거나 batching-safe 구조를 제공하기도 합니다.

---

## ✅ 정리

| 항목                 | 설명                                                     |
| -------------------- | -------------------------------------------------------- |
| 값 기반 setState     | 이전 상태를 제대로 반영하지 못할 수 있음 (덮어쓰기 현상) |
| 함수 기반 setState   | 최신 상태를 기반으로 안전하게 누적 업데이트 가능         |
| Batching             | 여러 상태 업데이트를 묶어 한 번에 렌더링하는 최적화 기법 |
| useRef/단일 setState | 상태가 꼭 필요하지 않은 경우, 대안으로 활용 가능         |

---

### 📚 참고 자료

- [React 공식 문서 - State: 컴포넌트의 기억 저장소](https://ko.react.dev/learn/state-a-components-memory)
- [React 공식 문서 - state 업데이트 큐](https://ko.react.dev/learn/queueing-a-series-of-state-updates)
- [리액트 배칭(Batching)의 모든 것 - 요즘IT](https://yozm.wishket.com/magazine/detail/2493/)
