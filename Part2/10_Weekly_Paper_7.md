# ⚛️ React - Virtual DOM과 key의 역할

## ✅ 1. Virtual DOM이란?

### 📌 정의

- Virtual DOM은 **브라우저의 실제 DOM(Document Object Model)을 추상화한 자바스크립트 객체**입니다.
- 리액트는 **실제 DOM을 직접 조작하지 않고**, Virtual DOM에서 변화가 생긴 부분만 찾아 실제 DOM에 최소한의 변경을 적용합니다.

### 🧱 실제 DOM과 Virtual DOM의 차이

| 항목 | 실제 DOM                        | Virtual DOM                         |
| ---- | ------------------------------- | ----------------------------------- |
| 구조 | 브라우저가 렌더링하는 UI 트리   | 메모리 내에서 관리되는 JS 객체 트리 |
| 속도 | 변경 시 느림 (재렌더링 비용 큼) | 변경 감지와 업데이트가 빠름         |
| 조작 | 직접 DOM API로 조작             | React 내부에서 자동 처리            |

### 💡 동작 원리

1. 컴포넌트가 렌더링되면 Virtual DOM 트리를 생성합니다.
2. 상태(state)나 props가 변경되면 **새로운 Virtual DOM을 생성**합니다.
3. 이전 Virtual DOM과 새 Virtual DOM을 비교(diffing)합니다.
4. 변경된 부분만 실제 DOM에 적용합니다 (Reconciliation 과정).

### ✅ 사용하는 이유

- **실제 DOM 조작은 느림** → Virtual DOM은 메모리 내에서 작동하여 빠름
- 변화가 있는 부분만 업데이트 → **불필요한 렌더링 최소화**
- 성능 최적화 + 일관된 UI 업데이트 보장

### 📌 간단한 그림으로 요약

```
UI 업데이트 요청
   ↓
새로운 Virtual DOM 생성
   ↓
이전 Virtual DOM과 비교(diff)
   ↓
변화가 있는 부분만 실제 DOM 조작
```

---

## ✅ 2. 배열 렌더링과 key

### 📌 배열 렌더링 예시

```jsx
const items = ['React', 'Vue', 'Angular'];

return (
  <ul>
    {items.map((item, index) => (
      <li key={index}>{item}</li>
    ))}
  </ul>
);
```

### 📌 key란?

- key는 **리스트의 각 요소를 구분하는 고유 식별자**입니다.
- React는 key를 사용해 **각 요소가 변경/추가/삭제되었는지를 정확히 판단**합니다.

### ✅ 사용하는 이유 (중요)

React는 배열을 렌더링할 때 **Virtual DOM의 요소들을 이전과 비교(diffing)** 하여 어떤 요소가 바뀌었는지를 판단합니다. 이때 key가 없다면 React는 다음과 같은 문제를 겪습니다:

1. **비효율적인 렌더링**: 모든 요소를 재렌더링하거나 재생성하게 됨
2. **잘못된 상태 유지**: 컴포넌트의 상태(state)가 엉뚱한 요소에 연결될 수 있음

### ⚠️ 잘못된 예시

```jsx
<li key={index}>{item}</li>
```

- 요소가 중간에 삽입되거나 삭제될 경우, 이후 요소들의 key가 달라지면서 상태나 DOM이 예상과 다르게 동작할 수 있음

#### 예시 시나리오

```jsx
const list = ['A', 'B', 'C'];

// 사용자 입력으로 "D"가 맨 앞에 추가됨
const updated = ['D', 'A', 'B', 'C'];
```

- key가 index이면:
  - 기존 C가 D로 인식되고, B는 A로, A는 D로... 모든 요소가 재렌더됨
- key가 고유한 id이면:
  - React는 새로 들어온 D만 렌더링하고 나머지는 유지함

### ✅ 올바른 key 사용 예시

```jsx
const items = [
  { id: 1, name: 'React' },
  { id: 2, name: 'Vue' },
];

<ul>
  {items.map(item => (
    <li key={item.id}>{item.name}</li>
  ))}
</ul>;
```

---

## ✅ 마무리 요약

| 항목        | 설명                                                             |
| ----------- | ---------------------------------------------------------------- |
| Virtual DOM | 실제 DOM 변경을 최소화하기 위한 JS 객체. 빠른 렌더링 가능        |
| key         | 리스트 렌더링 시 요소의 고유 식별자 역할. 성능 향상 및 버그 방지 |
