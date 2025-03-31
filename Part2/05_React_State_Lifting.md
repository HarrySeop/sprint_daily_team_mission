# 🚀 리액트의 State Lifting

## 🔁 단방향 데이터 흐름 (Unidirectional Data Flow)

리액트에서 데이터는 항상 **위에서 아래로(Top-down)** 흐릅니다.<br/> 이 원칙은 다음과 같은 방식으로 작동합니다:

- 부모 컴포넌트는 자식 컴포넌트로 데이터를 `props`를 통해 전달합니다.
- 자식 컴포넌트는 **props를 읽을 수는 있지만, 직접 수정할 수는 없습니다.**
- 상태(state)는 그것을 **소유한 컴포넌트 내에서만 변경 가능**하며, 변경된 결과는 다시 props를 통해 자식에게 전달됩니다.

---

## ✅ 1. State Lifting이 필요한 경우는?

### 📌 문제 상황

- 두 개 이상의 자식 컴포넌트가 **같은 상태 값을 공유**해야 할 때
- 하나의 자식에서 발생한 변화가 **다른 형제 컴포넌트에도 반영되어야** 할 때

### 🔍 예시 상황

```jsx
<Parent>
  <Child1 />
  <Child2 />
</Parent>
```

- `Child1`에서 입력한 값을 `Child2`에서 표시해야 하는 경우,
- 상태를 `Parent`로 끌어올리지 않으면 형제 간 직접적인 공유가 불가능합니다.

---

## ✅ 2. 상태 끌어올리기 (Lifting State Up)

### 📌 개념 정의

- `"Lifting State Up"`은 자식 컴포넌트에 위치한 상태를 **가장 가까운 공통 부모 컴포넌트로 옮기는 패턴**입니다.
- 부모가 상태를 "소유"하고, 자식 컴포넌트에 `props`로 상태 값과 상태 변경 함수를 전달합니다.
- 자식은 **상태를 직접 수정하지 않고**, 부모로부터 전달받은 함수를 실행하여 상태를 간접적으로 변경합니다.

---

## 🔄 동작 방식 단계별 정리

1. **상태 정의**:

- 상태는 공통 부모 컴포넌트(`Parent`)에 정의됩니다.

2. **props 전달**:

- 상태 값(`sharedText`)과 상태 변경 함수(`setSharedText`)를 자식 컴포넌트에 전달합니다.

3. **자식에서 상태 변경**:

- `Child1`에서 전달받은 `setSharedText`를 통해 상태를 업데이트합니다.

4. **리렌더링 발생**:

- 상태 변경 시 `Parent`와 관련된 자식 컴포넌트 모두가 새로운 값을 반영하여 리렌더링됩니다.

---

## 🧩 코드 예시 (형제 간 상태 공유)

```jsx
function Parent() {
  const [sharedText, setSharedText] = useState('');

  return (
    <div>
      <h2>🧩 부모 컴포넌트</h2>
      <Child1 sharedText={sharedText} setSharedText={setSharedText} />
      <Child2 sharedText={sharedText} />
    </div>
  );
}

function Child1({ sharedText, setSharedText }) {
  return (
    <div>
      <h3>✏️ Child1 - 입력</h3>
      <input
        type="text"
        value={sharedText}
        onChange={e => setSharedText(e.target.value)}
        placeholder="텍스트를 입력하세요"
      />
    </div>
  );
}

function Child2({ sharedText }) {
  return (
    <div>
      <h3>📺 Child2 - 출력</h3>
      <p>입력한 값: {sharedText}</p>
    </div>
  );
}
```

### 🧪 동작 과정

1. `Parent`는 `useState`를 통해 `sharedText` 상태를 선언합니다.
2. `Child1`은 input을 통해 상태 변경 함수 `setSharedText`를 실행합니다.
3. 상태가 변경되면 `Parent` 컴포넌트가 리렌더링되며, `Child2`는 변경된 `sharedText` 값을 받게 됩니다.

### ✅ 출력 결과 예시

```
Child1 input: Hello
→ Child2 화면: 입력한 값: Hello
```

---

## 🧠 State Lifting의 장점과 한계

### ✅ 장점

- 상태의 단일 출처(Single Source of Truth) 유지
- 데이터 흐름이 예측 가능하고 추적하기 쉬움
- 디버깅과 테스트가 용이

### ⚠️ 단점

- 컴포넌트가 많아지면 부모 컴포넌트가 상태 관리로 비대해질 수 있음
- `props drilling`: 하위 컴포넌트로 많은 props가 전달되면 가독성과 유지보수가 어려워짐

---

## ✅ 정리

| 항목               | 설명                                                                |
| ------------------ | ------------------------------------------------------------------- |
| 단방향 데이터 흐름 | 리액트에서 데이터는 항상 부모 → 자식 방향으로 흐름                  |
| State Lifting      | 형제 컴포넌트 간 상태 공유를 위해, 공통 부모로 상태를 끌어올리는 것 |
| 방법               | useState → 부모에 위치 / 자식엔 props로 전달, 변경 함수도 함께 전달 |
| 장점               | 상태의 단일화, 예측 가능한 데이터 흐름 확보                         |
| 단점               | 컴포넌트 복잡도 증가 시 props drilling 발생 가능                    |

---

### 📚 참고 자료

- [[React] 상태 끌어올리기 (Lifting State Up)](https://velog.io/@yeonjin1357/React-%EC%83%81%ED%83%9C-%EB%81%8C%EC%96%B4%EC%98%AC%EB%A6%AC%EA%B8%B0-Lifting-State-Up)
- [React 공식 문서 - 컴포넌트 간 State 공유하기](https://ko.react.dev/learn/sharing-state-between-components)
