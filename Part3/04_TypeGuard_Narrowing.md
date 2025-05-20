# 📘 타입 관련 개념 정리 (Type Guard, 타입 좁히기 & 넓히기, 에러 케이스)

## ✅ [질문 1] 타입 가드(Type Guard)란?

타입 가드는 런타임에서 **타입을 좁혀서 특정 타입임을 확정하는 기술**입니다.

타입스크립트는 조건문 안에서 `typeof`, `instanceof`, `in`, 사용자 정의 타입 가드 등을 활용해 타입을 좁힐 수 있습니다.

### 📌 사용 예시 (권장)

```ts
function handleValue(val: string | number): void {
  if (typeof val === 'string') {
    console.log(val.toUpperCase()); // string으로 좁혀짐
  } else {
    console.log(val.toFixed(2)); // number로 좁혀짐
  }
}
```

### ✅ 사용자 정의 타입 가드 (복잡한 객체 구분 시 유용)

```ts
interface Admin {
  role: 'admin';
  accessLevel: number;
}
interface User {
  role: 'user';
  name: string;
}

type Person = Admin | User;

function isAdmin(person: Person): person is Admin {
  return person.role === 'admin';
}

function greet(person: Person): void {
  if (isAdmin(person)) {
    console.log(`관리자 레벨: ${person.accessLevel}`);
  } else {
    console.log(`사용자 이름: ${person.name}`);
  }
}
```

### 🔎 언제 사용하면 좋을까?

- 유니언 타입 내 특정 타입 분기 처리 시 매우 유용
- 복잡한 객체 타입을 안전하게 다룰 때 필수적
- **✔️ 적극 활용하는 것이 좋음** (가독성 + 안정성 향상)

> 🔧 정리: 타입 가드는 복잡한 타입 분기를 안전하게 처리할 수 있어 **적극 사용하는 것이 좋습니다.**

---

## ✅ [질문 2] 타입 좁히기 / 타입 넓히기

### 🔍 타입 좁히기 (Type Narrowing)

- **값의 타입을 구체적인 하위 타입으로 좁히는 것**
- 조건문, typeof, 타입 가드 등을 통해 사용

```ts
function getLength(val: string | string[]): number {
  if (Array.isArray(val)) {
    return val.length;
  }
  return val.length;
}
```

### 🔍 타입 넓히기 (Type Widening)

- 변수 선언 시 초기값이 없거나 null/undefined인 경우, 타입이 **any 또는 더 일반적인 타입으로 추론**됨

```ts
let msg: any;
msg = 'hello';

let list = null; // 타입은 any
list = [1, 2, 3];
```

### 🔎 언제 주의해야 할까?

- 타입이 **불분명한 상태에서 너무 넓어지면** 타입 검사가 무력화됨
- 가능한 경우 타입을 **직접 명시하거나**, 리터럴 초기화로 안전한 타입 유도

### ✅ 권장 전략

- **넓히기보다 좁히기를 우선**으로 고려
- any가 추론되지 않도록 초기값 또는 명시된 타입 지정
- 점진적 추론보다는 **명확한 타입 선언을 우선 고려** (단, 너무 과한 타입 선언은 복잡도 증가 위험 있음)

> 🔧 정리: 코드 일관성과 유지보수성을 위해서 **가능한 경우 타입을 명시적으로 선언하는 것이 바람직합니다.** 단, 반복적이고 자명한 타입은 추론을 활용할 수 있습니다.

---

## ✅ [질문 3] 에러 발생 코드 분석

```ts
const mapModeToDate = {
  month: new Date().getDate(),
  week: 2,
};

const getDate = (mode: string) => {
  return mapModeToDate[mode];
};
```

### ❗ 발생하는 에러

```bash
  Element implicitly has an 'any' type because expression of type 'string' can't be used to index type '{ month: number; week: number; }'.
  No index signature with a parameter of type 'string' was found on type '{ month: number; week: number; }'.
```

### 📌 이유

- `mode`는 `string`이지만, `mapModeToDate`는 "month"와 "week" 두 키만 존재하는 **리터럴 객체**입니다
- 타입스크립트는 문자열 전반(`string`)으로 인덱싱 시, 키가 보장되지 않는다고 판단하여 에러를 발생시킴

### ✅ 해결 방법 1: 유니언 타입으로 제한

```ts
type Mode = 'month' | 'week';

const getDate = (mode: Mode): number => {
  return mapModeToDate[mode];
};
```

- 타입이 제한되므로 안전하게 키 접근 가능

### ✅ 해결 방법 2: 인덱스 시그니처 사용 (유연하지만 위험성 존재)

```ts
const mapModeToDate: { [key: string]: number } = {
  month: 12,
  week: 2,
};
```

- `string` 전체에 대해 허용하지만, **실제 키에 대한 안전성은 떨어짐** (오타 등)

### 🔍 왜 명확한 타입 선언이 중요한가?

- 명시적 타입 선언은 **IDE 자동완성, 타입 안전성, 문서화 효과** 제공
- 반면 점진적 추론은 초기에는 편하지만, **규모가 커질수록 유지보수에 취약**해짐

### ✅ 선언 방식 선택 기준

| 방식             | 장점                        | 단점                         | 사용 시점                                |
| ---------------- | --------------------------- | ---------------------------- | ---------------------------------------- |
| 명시적 타입 선언 | 안정성 높음<br/>IDE 도움 큼 | 다소 장황                    | 공통 모듈, 팀 프로젝트, 외부 공개 API 등 |
| 타입 추론        | 빠른 개발                   | 추론 실패 시 any 유입 가능성 | 자명한 로컬 변수, 간단한 유틸 등         |

> ✅ **결론**: 팀 프로젝트에서는 **명확한 타입 선언을 우선**으로 하고, 추론은 자명한 경우에만 선택적으로 활용하는 것이 좋습니다.

---

## 🔚 요약 정리

| 항목             | 권장 여부      | 이유                                  |
| ---------------- | -------------- | ------------------------------------- |
| 타입 가드        | ✅ 적극 권장   | 타입 분기 처리와 안정성 확보에 효과적 |
| 타입 좁히기      | ✅ 필수적      | 복합 타입의 안전한 처리에 필요        |
| 타입 넓히기      | ⚠️ 최소화      | 불필요한 any 발생 가능성 존재         |
| 유니언 타입 제한 | ✅ 권장        | 안전한 키 접근 보장                   |
| 인덱스 시그니처  | ⚠️ 제한적 사용 | 유연하지만 안전성 낮음                |
| 명시적 타입 선언 | ✅ 권장        | 팀 협업, 유지보수에 안정성 보장       |
| 타입 추론        | ⚠️ 제한적으로  | 로컬 변수 등 간단한 상황에만 허용     |

> 💡 타입스크립트는 "정확한 추론"과 "안정성"을 기반으로 설계되었기 때문에, 복잡한 로직일수록 **명확한 타입 명시**가 더 효과적인 방식입니다.
