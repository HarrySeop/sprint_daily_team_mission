# ⚛️ 클래스형 컴포넌트 vs 함수형 컴포넌트

React 컴포넌트를 작성하는 방식은 과거의 **클래스형 컴포넌트(Class Component)** 중심에서 현재는 **함수형 컴포넌트(Function Component) + Hooks** 방식으로 전환되었습니다.

---

## ✅ 클래스형 컴포넌트란?

### 📌 정의

- ES6의 `class` 문법을 기반으로 컴포넌트를 작성합니다.
- `React.Component`를 상속받고, 반드시 `render()` 메서드를 정의해야 합니다.
- 내부에서 `this.state`, `this.setState()`로 상태를 관리하며, `componentDidMount` 등 라이프사이클 메서드로 생명주기를 관리합니다.

### 🧪 예시

```jsx
class Welcome extends React.Component {
  constructor(props) {
    super(props);
    this.state = { count: 0 };
  }

  render() {
    return <p>Count: {this.state.count}</p>;
  }
}
```

---

## ✅ 함수형 컴포넌트란?

### 📌 정의

- 단순한 **JavaScript 함수**로 작성되는 컴포넌트입니다.
- React 16.8 이후 도입된 **Hooks**(`useState`, `useEffect` 등)를 통해 상태 관리와 생명주기 구현이 가능합니다.

### 🧪 예시

```jsx
function Welcome({ name }) {
  const [count, setCount] = useState(0);
  return (
    <p>
      Hello, {name} / Count: {count}
    </p>
  );
}
```

---

## 🔍 함수형 컴포넌트로 전환된 이유는?

### ✅ 1. 코드의 간결함과 가독성

- 클래스형은 `constructor`, `render()`, `this` 바인딩 등 **보일러플레이트 코드가 많음**
- 함수형은 **간단한 선언과 직관적인 상태 관리**가 가능함

### ✅ 2. `this` 바인딩 이슈 제거

- 클래스형에서는 이벤트 핸들러에서의 `this` 참조 오류가 흔한 문제
- 예: 아래 클래스 컴포넌트의 경우, 시간차 로직에서 `this.props` 값이 바뀌어 의도치 않은 결과 발생

```jsx
setTimeout(this.showMessage, 3000); // this가 예상과 다름 → 버그 발생 가능성
```

- 함수형 컴포넌트는 **`this`를 사용하지 않고, 클로저로 값을 안전하게 참조**하므로 의도된 동작 보장

### ✅ 3. 성능상의 이점

- 클래스형보다 **함수형이 메모리 사용이 적고, 빌드 후 파일 크기도 작음**
- `render()` 호출도 함수형은 더 가볍게 처리됨

### ✅ 4. 함수형 프로그래밍의 장점 활용

- 순수 함수 기반 구조
- props는 immutable하게 전달되어 **불변성과 예측 가능한 렌더링**을 보장
- 상태 및 효과를 `useState`, `useEffect` 등으로 **모듈화** 가능

---

## ⚠️ 클래스 컴포넌트의 대표적인 문제 예시

### 📌 클래스형 예시 코드

```jsx
class ProfilePage extends React.Component {
  showMessage = () => {
    alert(`${this.props.user}를 팔로우 했습니다`);
  };

  handleClick = () => {
    setTimeout(this.showMessage, 5000);
  };

  render() {
    return <button onClick={this.handleClick}>Follow</button>;
  }
}
```

### 🔍 예상 결과

- A 유저의 프로필 페이지에서 Follow 버튼을 누르면, 5초 후 `"A를 팔로우 했습니다"` 라는 메시지가 나올 것으로 기대함

### ⚠️ 실제 결과

- 버튼 클릭 후 5초 내에 B 유저 페이지로 이동하면, **`"B를 팔로우 했습니다"` 라는 메시지가 표시됨**
- 이유: `this.props.user`는 5초 뒤 실행 시점에 참조되며, 이미 변경된 props를 참조함 (this는 mutable)

### 🔧 이슈 원인

- `this`가 클로저처럼 값을 고정해두지 않고, 클래스 인스턴스의 최신 상태를 참조함
- 시간 차 로직(setTimeout)에서 `this.props`가 바뀌는 경우, **버그 발생 가능성**이 높음

---

### ✅ 함수형 컴포넌트 해결 예시

```jsx
function ProfilePage({ user }) {
  const showMessage = () => alert(`${user}를 팔로우 했습니다`);
  const handleClick = () => setTimeout(showMessage, 5000);

  return <button onClick={handleClick}>Follow</button>;
}
```

### ✅ 함수형에서의 실제 결과

- A 유저의 페이지에서 클릭 후 B 페이지로 이동해도, 5초 후에 **정확하게 `A를 팔로우 했습니다` 메시지 출력**
- 이유: `user` 값이 `showMessage` 함수 내부에서 **클로저로 캡처되어 고정**됨

### ✨ 핵심 차이점 요약

| 항목      | 클래스형 컴포넌트                     | 함수형 컴포넌트                   |
| --------- | ------------------------------------- | --------------------------------- |
| 참조 대상 | 실행 시점의 최신 props (`this.props`) | 정의 시점의 props (클로저로 캡처) |
| 결과      | 의도치 않은 값 출력 가능성            | 예상한 값 그대로 유지             |

---

## 🔄 차이점 정리

| 항목            | 클래스형 컴포넌트              | 함수형 컴포넌트                       |
| --------------- | ------------------------------ | ------------------------------------- |
| 작성 문법       | class 문법 + render            | 함수 선언 방식                        |
| 상태 관리       | `this.state` / `this.setState` | `useState`, `useReducer` 등 Hook 사용 |
| 생명주기 처리   | `componentDidMount`, 등 메서드 | `useEffect` 하나로 처리               |
| this 사용       | 필수 (바인딩 필요)             | 사용하지 않음                         |
| props 처리      | `this.props`                   | 함수 매개변수로 전달                  |
| 가독성/유지보수 | 구조가 복잡                    | 직관적이고 짧음                       |
| 최신 React 지원 | 새로운 기능 제한됨             | 공식 권장 방식 (Hook 기반)            |

---

## ✅ 배치 업데이트 (Batching Update)

- React는 여러 개의 `setState` 호출을 **하나의 업데이트로 묶어 처리**하여 렌더링 성능을 최적화함
- 이 최적화는 클래스형/함수형 모두에 적용됨
- 하지만 함수형에서는 상태 관리를 더 세밀하게 **분리하고 추상화**하기 쉽기 때문에 성능 튜닝 측면에서 유리

---

## ✅ 결론

- 클래스형 컴포넌트는 과거에는 표준이었지만, `this` 바인딩 문제와 반복적인 구조로 인해 유지보수가 어려웠습니다.
- 함수형 컴포넌트는 **Hooks 도입 이후 리액트의 새로운 표준**이 되었으며, **간결함, 안정성, 성능**에서 강점을 가집니다.

📌 새로운 프로젝트나 팀 협업에서는 **함수형 컴포넌트를 기본으로 사용**하는 것이 권장됩니다.

---

## 📚 참고 자료

- [보일러플레이트 코드](https://velog.io/@yellowbutter0327/%EB%B3%B4%EC%9D%BC%EB%9F%AC%ED%94%8C%EB%A0%88%EC%9D%B4%ED%8A%B8%EC%BD%94%EB%93%9C)
- [리액트 공식문서 - Component](https://ko.react.dev/reference/react/Component)
- [React 클래스형 컴포넌트에서 함수형 컴포넌트로 바뀐 이유?](https://velog.io/@shinyejin0212/React-%ED%81%B4%EB%9E%98%EC%8A%A4%ED%98%95-%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8%EC%97%90%EC%84%9C-%ED%95%A8%EC%88%98%ED%98%95-%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8%EB%A1%9C-%EB%B0%94%EB%80%90-%EC%9D%B4%EC%9C%A0)
