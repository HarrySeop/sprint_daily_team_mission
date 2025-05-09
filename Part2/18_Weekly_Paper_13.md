# 💡 TypeScript란 무엇인가요?

TypeScript는 JavaScript의 상위 집합(Superset)으로, **정적 타입 검사(Static Type Checking)** 와 **ES6+ 기능 지원**을 통해 **개발 생산성과 안정성**을 향상시키는 언어입니다.

JavaScript로 작성된 코드에 타입 시스템을 추가한 언어이며, 최종적으로는 **순수 JavaScript 코드로 트랜스파일**되어 실행됩니다.

---

## JavaScript만 사용하는 것과 비교해 TypeScript를 사용하는 이유는?

### ✅ 1. 정적 타입 검사로 버그를 줄일 수 있다

- 컴파일 타임에 타입 오류를 미리 발견 가능 → 런타임 오류 감소

```ts
function sendMessage(message: string) {
  console.log('👋', message);
}

sendMessage('GPT에게 질문!'); // ✅ OK
sendMessage(404); // ❌ 에러: number 타입은 string에 할당할 수 없음
```

### ✅ 2. 개발자 도구 지원 향상 (자동완성, 인텔리센스)

- 타입 기반으로 함수 매개변수, 반환값 등을 자동 추론

```ts
interface User {
  id: number;
  name: string;
}

const user: User = { id: 1, name: '지섭' };

// user. → 자동완성으로 name, id 제공됨
```

### ✅ 3. 코드의 명확성과 문서화

- 타입 선언이 곧 함수/객체의 문서처럼 사용 가능

```ts
function createUser(id: number, name: string): User { ... }
```

→ 코드를 읽는 것만으로 어떤 데이터가 필요한지 바로 파악 가능

### ✅ 4. 리팩토링 안전성 ↑

- 변수/함수명을 변경해도 타입이 맞지 않으면 컴파일 에러로 감지 가능

### ✅ 5. 대형 프로젝트와 협업에서 효과적

- 여러 명이 동시에 작업할 때 함수나 객체의 **의도된 사용 방법**을 명확히 전달 가능

> 🧠 정리하면, "코드가 커지고 팀원이 늘어날수록 TypeScript가 점점 더 큰 힘을 발휘한다!"

---

## TypeScript의 동작 원리에 대해 설명해 주세요

### ✅ 동작 요약

> TypeScript → 컴파일 → JavaScript → 브라우저에서 실행

### 1️⃣ `.ts` 또는 `.tsx` 파일에서 코드를 작성합니다.

```ts
let userName: string = 'ChatGPT';
console.log(`Hello, ${userName}`);
```

### 2️⃣ TypeScript 컴파일러(`tsc`)가 타입 검사 및 트랜스파일 수행

- 타입 오류는 이 단계에서 발견됨
- 타입만 검사하고 실행은 하지 않음

### 3️⃣ JS 코드로 변환 후 실행

```js
var userName = 'ChatGPT';
console.log('Hello, ' + userName);
```

📌 브라우저는 `.ts` 파일을 직접 실행할 수 없고, 항상 JS로 변환된 코드만 인식합니다.

---

## 🛠 tsconfig.json 설정 예시

`tsconfig.json` 파일은 프로젝트 전역 TypeScript 컴파일러 옵션을 정의합니다.

```json
{
  "compilerOptions": {
    "target": "ES6",
    "module": "ESNext",
    "strict": true,
    "esModuleInterop": true
  },
  "include": ["src"],
  "exclude": ["node_modules"]
}
```

---

## ✨ TypeScript를 사용할만한 실제 예시

### 1️⃣ 협업 중 Props 오용 방지

```ts
// 컴포넌트 정의
interface ButtonProps {
  label: string;
  onClick: () => void;
}

function CustomButton({ label, onClick }: ButtonProps) {
  return <button onClick={onClick}>{label}</button>;
}

// 잘못된 사용 (onClick 빠짐)
<CustomButton label="클릭" />; // ❌ 컴파일 에러
```

### 2️⃣ API 응답 처리

```ts
interface Post {
  id: number;
  title: string;
  body: string;
}

async function fetchPosts(): Promise<Post[]> {
  const res = await fetch('/posts');
  const data = await res.json();
  return data;
}
```

→ 서버 응답 타입을 명시함으로써 오류 없이 안전하게 사용 가능

---

## 📌 요약 비교표

| 항목      | JavaScript         | TypeScript                 |
| --------- | ------------------ | -------------------------- |
| 타입 검사 | 없음 (런타임 오류) | 있음 (컴파일 타임 오류)    |
| 도구 지원 | 제한적             | 자동완성, 인텔리센스 우수  |
| 리팩토링  | 불안정             | 안정적, 규모가 클수록 유리 |
| 실행 환경 | 브라우저/Node.js   | 트랜스파일 후 JS로 실행    |
| 협업      | 명시적 문서 필요   | 타입이 곧 문서 역할        |
