# 🧩 TypeScript 관련 데일리 팀미션 1번 주제

## 1️⃣ `Union`과 `Intersection`의 차이

### 🔸 Union 타입 (`|`)

- **여러 타입 중 하나**를 가질 수 있다는 의미
- 값이 여러 타입 중 **하나**만 만족하면 됩니다

```ts
let value: string | number;

value = 'hello'; // ✅
value = 123; // ✅
value = true; // ❌ boolean은 안 됨
```

#### ✅ 사용 예시

```ts
function printResult(result: number | string) {
  if (typeof result === 'string') {
    console.log(result.toUpperCase());
  } else {
    console.log(result.toFixed(2));
  }
}
```

📌 유니언은 OR 관계이며 **타입의 유연성이 필요한 곳에 유용**합니다.

#### ⚠️ 주의할 점

- **각 타입에 대한 분기처리**를 해줘야 하기 때문에 코드가 길어질 수 있음
- 타입 가드를 사용하지 않고 유니언 타입을 처리하면 오류 발생 가능

🔍 타입 가드(Type Guard)란?

> 조건문 등을 사용해 특정 타입임을 명확하게 검증하고, 그 타입으로 안전하게 사용할 수 있도록 해주는 구문입니다.

- `typeof`, `instanceof`, 사용자 정의 타입 가드 등이 있음

---

### 🔸 Intersection 타입 (`&`)

- **모든 타입을 동시에 만족**해야 한다는 의미
- 객체 타입을 합칠 때 주로 사용됩니다

```ts
type User = { name: string };
type Admin = { role: string };

type AdminUser = User & Admin;

const admin: AdminUser = {
  name: 'JiSeop',
  role: '관리자',
};
```

#### ✅ 사용 예시

```ts
type Timestamped = { createdAt: Date };
type Post = { title: string };

type TimestampedPost = Post & Timestamped;

const newPost: TimestampedPost = {
  title: 'TypeScript 배우기',
  createdAt: new Date(),
};
```

📌 인터섹션은 AND 관계이며 **속성을 결합**할 때 매우 유용합니다.

#### ⚠️ 주의할 점

- 두 타입이 **중복되는 속성**을 서로 다르게 정의하고 있으면 **충돌** 발생
- 일반적으로 객체 타입에만 사용하며, 원시 타입(intersection of string & number 등)은 실용성이 낮음

---

## 2️⃣ `void` 타입이란?

- **아무것도 반환하지 않는 함수**를 의미합니다.
- 함수가 명시적으로 값을 `return`하지 않거나 `return undefined`인 경우 사용됩니다.

```ts
function logMessage(message: string): void {
  console.log(message);
}
```

#### ✅ 사용 예시

- 이벤트 핸들러, 로그 출력 등 **결과값을 사용하지 않는 함수**에 적합

```ts
button.addEventListener('click', () => {
  console.log('Clicked'); // 아무것도 return 안함
});
```

#### ⚠️ 피해야 할 점

- `void` 함수에서 return 값을 변수에 할당하거나 사용하려는 경우 타입의 의미가 흐려질 수 있음

```ts
const result: void = logMessage('Hi');
console.log(result); // ❌ result는 undefined이므로 콘솔에 출력할 의미가 없음
```

✅ **권장**: 반환값이 없음을 명확히 표현할 때 사용하며, 반환값을 참조하지 않도록 주의

---

## 3️⃣ `unknown` 타입이란?

- **어떤 타입이든 담을 수 있지만, 사용할 때는 반드시 타입 검사를 해야 하는 타입**입니다.
- `any`와 달리 **안전한 타입**입니다.

```ts
let input: unknown;

input = 'hello';
input = 42;
input = true;
```

```ts
function handleInput(value: unknown) {
  // console.log(value.toUpperCase()); // ❌ 오류!

  // 타입 가드를 활용한 안전한 사용
  if (typeof value === 'string') {
    console.log(value.toUpperCase()); // ✅
  }
}
```

📌 `unknown`은 외부에서 전달되는 값의 타입이 명확하지 않을 때 안전하게 감싸주는 역할을 합니다.

#### ✅ 사용 예시

- 외부 API로부터 데이터를 받을 때 (예: `fetch` 결과)

```ts
async function getData(): Promise<unknown> {
  const res = await fetch('/api/data');
  return res.json();
}
```

- 이후에는 `as 타입` 또는 타입 가드 등을 통해 **명확한 타입으로 추론하거나 단언하여 사용**해야 합니다

```ts
const data = await getData();
if (typeof data === 'object' && data !== null && 'name' in data) {
  console.log((data as { name: string }).name); // 안전하게 사용
}
```

🔍 `as` 키워드란?

> 타입스크립트에서 특정 값의 타입을 개발자가 명시적으로 "단언(assert)"하여, 컴파일러에게 "이 값은 이 타입이야"라고 알려주는 방식입니다.

```ts
const value: unknown = 'typescript';
const strLength = (value as string).length; // ✅ 컴파일러가 string으로 인식
```

- `as`는 **컴파일러에게 힌트를 주는 것일 뿐**, 실제 런타임에서 타입 체크를 하지 않기 때문에 **잘못된 타입 단언은 오류를 숨길 수 있음**

```ts
const value: unknown = 123;
console.log((value as string).toUpperCase()); // ❌ 런타임 오류 (숫자를 string으로 단언)
```

✅ **권장**: 타입 가드로 타입이 보장된 후 사용하거나, 신뢰할 수 있는 구조일 때 사용하세요

#### 💡 `unknown` vs `any`

| 타입      | 설명                                         | 위험성             |
| --------- | -------------------------------------------- | ------------------ |
| `any`     | 아무 타입도 허용, 모든 연산 가능             | 매우 위험 (비추천) |
| `unknown` | 모든 타입을 담을 수 있지만 사용 전 검사 필수 | 안전 (권장)        |

- `any`는 **어떤 타입이든 될 수 있고, 이후 어떤 동작이든 할 수 있음** (컴파일러가 아무런 타입 오류를 잡아주지 않음)
- 반면 `unknown`은 **어떤 타입이든 될 수 있지만, 그 이후 어떤 동작도 허용하지 않음** → 사용하는 쪽에서 반드시 타입 체크 필요

```ts
let value: any = 'hello';
value.toUpperCase(); // 가능하지만, 오류가 있어도 감지 못함 (위험)

let safeValue: unknown = 'hello';
safeValue.toUpperCase(); // ❌ 오류 발생 → 타입 검사 없이는 사용 불가
```

✅ **권장**: 외부 입력값 또는 추론이 어려운 값을 다룰 때는 `unknown`을 사용하고, 이후 **명확한 타입으로 전환**하거나 **타입 가드를 사용**해 안전하게 처리
