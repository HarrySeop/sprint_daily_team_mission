# ⚛️ React - useEffect, useMemo, useCallback, 조건부 렌더링

## ✅ 1. useEffect란?

### 📌 정의

- `useEffect`는 **React 함수형 컴포넌트에서 side effect(부수 효과)** 를 처리하기 위한 **Hook**입니다.
- 컴포넌트의 생명주기(lifecycle)를 함수형 컴포넌트에서도 다룰 수 있도록 해줍니다.

### 🧾 기본 프로토타입

```jsx
useEffect(() => {
  // effect 실행 코드 (mount/update 시 실행)

  return () => {
    // cleanup 함수 (unmount 또는 다음 effect 실행 전 실행됨)
  };
}, [dependency]);
```

### ✅ 언제 사용하나요?

- **컴포넌트가 처음 렌더링될 때** 실행해야 하는 작업
- **의존성 값이 변경될 때**만 실행해야 하는 작업
- **언마운트 시 정리 작업**(ex. 이벤트 제거, 타이머 제거 등)

> ⚠️ `useEffect` 내에서 연산이 많은 로직이나 상태 변경 함수가 있을 경우 불필요한 리렌더링을 유발할 수 있습니다. <br/>
> 이 경우 `useMemo`, `useCallback`을 통해 연산을 최적화할 수 있습니다.

## 🔍 useEffect 실행 방식에 따른 분류

### ① 의존성 배열이 없는 경우

```jsx
useEffect(() => {
  console.log('항상 실행됩니다');
});
```

- 모든 렌더링마다 실행됩니다

### ② 빈 배열 `[]`인 경우 (마운트 시 1회)

```jsx
useEffect(() => {
  console.log('처음 한 번만 실행됩니다');
}, []);
```

- 컴포넌트 마운트 시 한 번만 실행됨

### ③ 특정 값이 의존성으로 있는 경우

```jsx
useEffect(() => {
  console.log('count가 변경될 때마다 실행됩니다');
}, [count]);
```

- 의존성 값이 바뀔 때만 실행됨

---

## 🧠 useMemo & useCallback로 성능 최적화하기

### 📌 useMemo란?

- `useMemo`는 **연산 비용이 큰 값을 메모이제이션**하여, **불필요한 재계산을 방지**하는 Hook입니다.

```jsx
const result = useMemo(() => heavyCalculation(input), [input]);
```

### 📌 useCallback이란?

- `useCallback`은 **함수 자체를 메모이제이션**하여, **컴포넌트가 리렌더링될 때마다 동일한 함수 인스턴스를 재사용**하게 합니다.

```jsx
const handleClick = useCallback(() => {
  console.log('clicked');
}, []);
```

### ✅ 차이점

| Hook          | 대상    | 사용 목적             |
| ------------- | ------- | --------------------- |
| `useMemo`     | 계산 값 | 연산 결과 캐싱        |
| `useCallback` | 함수    | 동일한 함수 참조 유지 |

---

## 🔁 예시 비교: useEffect vs useMemo vs useCallback

### 🎯 예: 무거운 연산을 포함한 상태 업데이트

```jsx
// ❌ 비효율적인 방식 (매번 계산)
useEffect(() => {
  setResult(heavyCalculation(input));
}, [input]);

// ✅ useMemo로 최적화
const result = useMemo(() => heavyCalculation(input), [input]);
```

### 🎯 예: 이벤트 핸들러 함수 재사용

```jsx
// ❌ 매 렌더링마다 새로운 함수 생성됨
<button onClick={() => console.log('click')}>Click</button>;

// ✅ useCallback으로 최적화
const handleClick = useCallback(() => {
  console.log('click');
}, []);
<button onClick={handleClick}>Click</button>;
```

> 📌 useEffect는 부수효과를 다루는 목적이고, useMemo/useCallback은 **렌더링 성능 최적화**에 더 적합합니다.

---

## ✅ 2. 조건부 렌더링이란?

### 📌 정의

- 조건부 렌더링은 **상태나 props 등의 조건에 따라 다른 UI를 렌더링**하는 방법입니다.
- UI를 동적으로 구성할 때 반드시 필요한 기법입니다.

### ✅ 어떤 상황에서 사용하나요?

| 상황                | 예시                                    |
| ------------------- | --------------------------------------- |
| 로그인 여부         | 로그인 시 유저 정보, 아니면 로그인 버튼 |
| 데이터 로딩 상태    | 로딩 중 메시지 또는 로딩 스피너 표시    |
| 에러 상태           | 에러 메시지 UI 표시                     |
| 역할 또는 권한 조건 | 관리자만 볼 수 있는 버튼 표시 등        |

---

## 🧪 조건부 렌더링 예시

### 📌 if 문 사용

```jsx
if (isLoggedIn) return <p>환영합니다!</p>;
return <p>로그인이 필요합니다.</p>;
```

### 📌 삼항 연산자

```jsx
<span>{online ? '접속 중' : '오프라인'}</span>
```

### 📌 && 연산자

```jsx
{
  message && <p>{message}</p>;
}
```

### 📌 기본값 처리 (`||`)

```jsx
<h1>{title || '기본 제목'}</h1>
```

### 📌 Early return

```jsx
if (!user) return null;
return <p>{user.name}</p>;
```

---

## ✅ 마무리 요약

| 항목          | 설명                                     |
| ------------- | ---------------------------------------- |
| `useEffect`   | 렌더링 이후 실행되는 부수효과 처리 훅    |
| `useMemo`     | 계산 결과 메모이제이션 (값 캐싱)         |
| `useCallback` | 함수 인스턴스 메모이제이션 (성능 최적화) |
| 조건부 렌더링 | 상태나 조건에 따라 다른 JSX 반환         |
| 최적화 목적   | 불필요한 연산/렌더링을 방지해 성능 향상  |
