# ⚛️ React Hooks 퀴즈 - useState in if statement

## ✅ 문제 코드

```jsx
function App() {
  const [ageFirst, setAgeFirst] = useState(true);

  let age, setAge, name, setName;

  if (ageFirst) {
    [age, setAge] = useState(20);
    [name, setName] = useState('John');
  } else {
    [name, setName] = useState('John');
    [age, setAge] = useState(20);
  }

  return (
    <div>
      <p>age: {age}</p>
      <p>name: {name}</p>
      <button onClick={() => setAgeFirst(!ageFirst)}>change</button>
    </div>
  );
}
```

## ❌ 예상: 오류가 발생할 것이다.

React 공식 문서에 따르면 Hook 사용 시 다음과 같은 규칙이 있습니다:

> 훅은 항상 컴포넌트의 최상위 레벨에서 호출해야 하며, **조건문, 반복문, 중첩 함수 안에서 호출해서는 안 됩니다.**

처음 보는 코드 형태기도 했고, 멘토님이 문제를 냈던 것 처럼 동작할 것 처럼 보이지만 동작하지 않는 코드일 것 같았습니다.<br/>
다만 클로저로 인해서 **"함수 Counter()의 실행이 끝나더라도 변수 counter는 사라지지 않는다."** 라고 합니다.

---

## ✅ 실제 결과

```plaintext
초기 렌더링 (ageFirst: true)
----------------------------
age: 20
name: John

버튼 클릭 (ageFirst: false)
----------------------------
age: John
name: 20
```

버튼 클릭 후 상태 값이 서로 뒤바뀌어 출력됩니다.

---

## 🔍 왜 동작했는가?

React는 내부적으로 다음과 같은 구조를 통해 상태를 관리합니다:

```js
let _states = [];
let cursor = 0;

function useState(initialValue) {
  const current = cursor;
  if (_states[current] === undefined) {
    _states[current] = initialValue;
  }
  const setState = newValue => {
    _states[current] = newValue;
    render();
  };
  return [_states[cursor++], setState];
}
```

- useState가 호출될 때마다 `cursor` 인덱스를 기준으로 `_states` 배열에 값을 저장합니다.
- 상태는 변수명과는 무관하게 **호출 순서(index)** 에 의해 관리됩니다.

### ✅ 첫 렌더링 시

```js
[0] useState(true)  → ageFirst
[1] useState(20)    → age
[2] useState("John") → name
```

### ✅ 버튼 클릭 후 (ageFirst가 false가 되면서 분기 변경)

```js
[0] useState(true)     → ageFirst
[1] useState("John")   → name
[2] useState(20)       → age
```

- Hook의 **총 호출 횟수(3개)** 와 **순서(0 → 1 → 2)** 는 같기 때문에 React는 에러를 발생시키지 않습니다.
- 하지만 변수에 할당되는 값이 뒤바뀌면서, `age`에 `"John"`, `name`에 `20`이 들어가게 됩니다.

---

## 🚫 이렇게 사용하면 안 되는 이유

- React는 Hook을 순서 기반으로 상태를 관리합니다.
- 조건문 안에서 Hook을 호출하면, **실행 흐름에 따라 Hook의 순서가 바뀔 위험이 생깁니다.**
- 이번 사례처럼 Hook 호출 수와 순서가 우연히 동일하게 유지되는 경우에는 동작은 하지만, 의도하지 않은 결과가 발생합니다.
- 만약 조건 분기에 따라 `useState`의 **호출 수 자체가 달라진다면** React는 실제로 오류를 발생시킵니다 (`Rendered fewer hooks than expected`).

따라서, 조건문 안에서 Hook을 호출하는 코드는 다음과 같은 문제점을 가집니다:

1. **코드의 동작을 예측하기 어려움**: 같은 컴포넌트가 조건에 따라 전혀 다른 동작을 보일 수 있음.
2. **유지보수가 어려움**: 리팩토링 중 실수로 Hook 순서가 달라지면 예기치 않은 버그가 발생할 수 있음.
3. **React의 내부 상태 추적과 불일치 위험**: 의도하지 않은 상태 할당 문제 유발.

---

## ✅ 결론

- 이 코드는 Hook 호출 순서가 **우연히 동일하게 유지되었기 때문에 동작**합니다.
- 하지만 React가 의도한 방식은 아니며, Hook 규칙을 위반하는 패턴입니다.
- Hook은 반드시 컴포넌트의 **최상단에서 호출**되어야 하며, **조건에 따라 호출 순서가 달라지면 안 됩니다.**

즉, 동작은 했지만, 코드의 신뢰성과 안정성을 위해서 React Hook의 규칙은 반드시 지켜져야 합니다.
