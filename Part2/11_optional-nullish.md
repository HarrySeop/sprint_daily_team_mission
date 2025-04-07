# ✅ 논리 연산자와 병합 연산자

## 🔹 `?.` 옵셔널 체이닝 (Optional Chaining)

### 📌 정의

- 옵셔널 체이닝은 **객체의 속성에 안전하게 접근**할 수 있도록 도와주는 연산자입니다.
- 중간에 `null` 또는 `undefined`가 있으면 **에러를 발생시키지 않고 undefined를 반환**합니다.

### 🧪 기본 예시

```js
const user = { profile: { name: 'Jane' } };

console.log(user.profile?.name); // "Jane"
console.log(user.account?.balance); // undefined (에러 발생하지 않음)
```

### 💼 사용 예시

#### ✅ 상황: API 응답 객체가 예상대로 오지 않을 때

```js
// 응답 객체가 종종 빈 객체이거나 profile이 없을 수 있음
const user = await fetchUser();
const userName = user?.profile?.name ?? '이름 없음';
```

---

## 🔹 `??` 널 병합 연산자 (Nullish Coalescing Operator)

### 📌 정의

- `??`는 **좌항이 null 또는 undefined일 때** 우항을 반환합니다.

### 🧪 기본 예시

```js
const name = null ?? 'Anonymous'; // → "Anonymous"
const count = 0 ?? 10; // → 0
```

### 💼 사용 예시

#### ✅ 상황: 입력값 또는 설정값이 없을 때 기본값 제공

```js
const pageSize = config.itemsPerPage ?? 10; // config.itemsPerPage가 null/undefined일 때만 기본값 사용
```

#### ✅ 상황: 사용자 입력값에서 "0", ""도 유효하게 처리해야 할 때

```js
function Price({ amount }) {
  const price = amount ?? '가격 정보 없음';
  return <p>{price}</p>;
}
```

> `amount = 0`인 경우에도 "가격 정보 없음"이 아니라 `0`으로 정상 출력됨 (if amount === 0 ⇒ keep it)

---

## 🔹 `||` OR 연산자와의 차이

### 📌 정의

- `||`는 좌항이 **falsy한 값이면** 우항을 반환합니다.
- `falsy` 값: `false`, `0`, `""`, `null`, `undefined`, `NaN`

### 🧪 비교 예시

```js
const a = 0 || 10; // 10
const b = 0 ?? 10; // 0

const name1 = '' || 'No Name'; // "No Name"
const name2 = '' ?? 'No Name'; // ""
```

### 💼 사용 예시

#### ✅ 상황: 기본값을 빠르게 지정할 때 (단, 0, false, "" 등도 무시됨에 유의)

```js
// 로그인한 사용자의 닉네임이 없으면 "게스트"로 표시
const displayName = user.nickname || '게스트';
```

#### ⚠️ 문제 예시 (사용자 입력값 0을 무시하게 됨)

```js
// 사용자 수 입력값이 0인데 무시됨
const userCount = inputValue || 1; // 0 입력해도 1이 되어버림

// → 해결: 널 병합 연산자 사용
const userCount = inputValue ?? 1; // 0은 그대로 유지
```

---

## ✅ 연산자 비교 요약

| 연산자 | 작동 조건                        | 반환 기준      | 주의할 점                    |
| ------ | -------------------------------- | -------------- | ---------------------------- |
| `?.`   | 객체 접근 중 null/undefined 발생 | undefined 반환 | 연쇄 접근 시 안전성 확보     |
| `??`   | null 또는 undefined              | 우항 반환      | 0, "", false는 그대로 유지   |
| `\|\|` | falsy 값 전체                    | 우항 반환      | 0, "", false도 무시됨에 주의 |

---

## 📚 참고 자료

- [MDN - Optional chaining (?.)](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Optional_chaining)
