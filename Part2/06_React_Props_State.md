# ⚛️ React - Props & State

## ✅ 1. Props란?

### 📌 정의

- **Props**(properties)는 부모 컴포넌트가 자식 컴포넌트에게 전달하는 **데이터 혹은 함수**입니다.
- 컴포넌트 외부에서 주어지며, 자식 컴포넌트에서는 해당 데이터를 **읽기만** 할 수 있습니다.

### ✅ 특징

- 단방향 데이터 흐름 (부모 → 자식)
- 자식 컴포넌트는 props를 통해 상태나 동작을 제어할 수 있는 **함수도 전달받을 수 있음**
- **읽기 전용(Read-only)** → 자식이 props를 직접 변경할 수 없음
- 컴포넌트를 **재사용 가능**하게 만듦

### 🧩 예시

```jsx
function Greeting(props) {
  return <h1>Hello, {props.name}!</h1>;
}

<Greeting name="Harry" />; // Hello, Harry!
```

## ✅ 2. State란?

### 📌 정의

- **State**는 컴포넌트 내부에서 선언되며, 컴포넌트가 **동적으로 관리하는 데이터**입니다.
- 사용자의 입력, 이벤트 처리, 외부 API 응답 등에 따라 변경될 수 있습니다.

### ✅ 특징

- 컴포넌트 **내부에서 선언하고, 소유**함
- 상태가 변경되면 해당 컴포넌트와 하위 자식 컴포넌트가 **자동으로 리렌더링**됨

### ✅ props와 state의 관계

- **state는 props의 영향을 받을 수 있습니다.**
- 부모로부터 받은 `props`를 통해 초기 `state`를 설정할 수 있습니다.
- 단, 이후 상태 변경은 컴포넌트 내부에서만 수행해야 하며, props로 받은 값을 그대로 state로 복사하는 것은 대부분 권장되지 않습니다.

```jsx
function WelcomeMessage({ initialName }) {
  const [name, setName] = useState(initialName);
  return <p>Welcome, {name}</p>;
}

<WelcomeMessage initialName="React" />;
```

- 위 예시처럼 props로 받은 값을 기준으로 state를 초기화하는 건 가능하지만, 이후에는 state만을 기준으로 동작해야 합니다.

### 🧩 예시

```jsx
function Counter() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>+1</button>
    </div>
  );
}
```

---

## 🔍 3. Props vs State 차이점 비교

| 항목        | Props                         | State                                  |
| ----------- | ----------------------------- | -------------------------------------- |
| 정의        | 부모 → 자식 전달 값           | 컴포넌트 내부에서 변경되는 값          |
| 소유자      | 부모 컴포넌트                 | 현재 컴포넌트                          |
| 변경 여부   | 읽기 전용 (변경 불가)         | 변경 가능 (`setState` 또는 `useState`) |
| 사용 목적   | 컴포넌트 재사용과 데이터 전달 | 동적 UI 처리, 사용자 상호작용 응답     |
| 데이터 흐름 | 단방향 (부모 → 자식)          | 내부 처리 및 필요 시 props에 영향 가능 |
| 초기화 관계 | state 초기값으로 사용 가능    | props를 기반으로 초기 설정만 가능      |

---

## ✅ 마무리 요약

- **Props**: 외부(부모)로부터 전달된 데이터, **읽기 전용**, 컴포넌트 간 데이터 전달
- **State**: 컴포넌트 내부에서 **변경 가능한 데이터**, 렌더링과 UI 상태를 제어
- 둘은 함께 사용되어 **유연하고 반응형인 UI**를 구성합니다.
