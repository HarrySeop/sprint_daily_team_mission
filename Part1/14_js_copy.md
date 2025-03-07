# 🌟 JavaScript에서 얕은 복사(Shallow Copy)와 깊은 복사(Deep Copy)

## ✅ 얕은 복사(Shallow Copy)

### 📌 특징

- **객체의 1차원 프로퍼티(값)** 만 복사되고, **중첩된 객체(참조형 데이터)** 는 **주소(reference)만 복사됨**
- 원본 객체의 중첩된 객체가 변경되면, 복사된 객체도 영향을 받음

### 📌 얕은 복사 방법

```javascript
const obj1 = { a: 1, b: { c: 2 } };

// Object.assign() 사용
const obj2 = Object.assign({}, obj1);

console.log(obj1 === obj2); // false
console.log(obj1.b === obj2.b); // true (참조가 유지됨)

// 값 변경
obj1.b.c = 3;
console.log(obj2.b.c); // 3 (원본 변경이 복사본에도 영향)
```

✅ **언제 사용할까?**

- 중첩된 객체가 없는 **단순한 객체 복사** 시 사용 가능
- 하지만 중첩된 객체가 있을 경우 원본과 복사본이 연결되므로 주의 필요

---

## ✅ 깊은 복사(Deep Copy)

### 📌 특징

- **객체의 모든 프로퍼티(값 포함, 중첩된 객체까지)** 완전히 새로운 복사본을 생성
- 원본 객체와 복사본이 **완전히 독립적**이므로, 한쪽을 변경해도 다른 쪽에 영향을 주지 않음

### 📌 깊은 복사 방법

#### 1️⃣ `JSON.parse(JSON.stringify(obj))` (간단하지만 한계 있음)

```javascript
const obj1 = { a: 1, b: { c: 2 } };
const obj2 = JSON.parse(JSON.stringify(obj1));

obj1.b.c = 3;
console.log(obj2.b.c); // 2 (영향 없음)
```

✅ **한계점**:

- `undefined`, `function`, `Symbol`, `Date`, `Set`, `Map` 등은 복사되지 않음

#### 2️⃣ `structuredClone()` (최신 브라우저 지원)

```javascript
const obj1 = { a: 1, b: { c: 2 } };
const obj2 = structuredClone(obj1);

obj1.b.c = 3;
console.log(obj2.b.c); // 2 (영향 없음)
```

✅ **장점**:

- `structuredClone()`은 `JSON.stringify()`보다 더 강력하며, `Date`, `RegExp`, `ArrayBuffer` 등의 복사도 지원

#### 3️⃣ Lodash의 `cloneDeep()` 사용

```javascript
const _ = require('lodash');
const obj1 = { a: 1, b: { c: 2 } };
const obj2 = _.cloneDeep(obj1);

obj1.b.c = 3;
console.log(obj2.b.c); // 2 (영향 없음)
```

✅ **장점**:

- 모든 데이터 타입을 완벽하게 복사 가능
- 브라우저 및 Node.js 환경에서 사용 가능

---

## ✅ 얕은 복사와 불변 객체 만들기

### 📌 `Object.freeze()`를 사용한 얕은 동결

```javascript
const obj = { a: 1, b: { c: 2 } };
const frozenObj = Object.freeze(obj);

frozenObj.a = 3; // 실패 (변경 불가)
frozenObj.b.c = 3; // 성공 (중첩된 객체는 변경 가능)
```

✅ **문제점**: `Object.freeze()`는 **최상위 속성만 동결**하고, 중첩된 객체는 여전히 변경 가능함

### 📌 `deepFreeze()`를 사용한 깊은 동결

```javascript
function deepFreeze(obj) {
  Object.getOwnPropertyNames(obj).forEach(prop => {
    if (typeof obj[prop] === 'object' && obj[prop] !== null) {
      deepFreeze(obj[prop]);
    }
  });
  return Object.freeze(obj);
}

const obj = { a: 1, b: { c: 2 } };
const frozenObj = deepFreeze(obj);

frozenObj.b.c = 3; // 실패 (중첩된 객체도 변경 불가)
```

✅ **장점**: 모든 중첩된 속성을 포함하여 객체를 완전히 불변 상태로 만들 수 있음

---

## ✅ 얕은 복사 vs 깊은 복사 비교

| 비교 항목 | 얕은 복사 (Shallow Copy)               | 깊은 복사 (Deep Copy)                                         |
| --------- | -------------------------------------- | ------------------------------------------------------------- |
| 복사 방식 | 1차원 값만 복사, 중첩 객체는 참조 유지 | 중첩 객체까지 모두 새로운 값으로 복사                         |
| 원본 영향 | 중첩된 객체 변경 시 복사본도 영향받음  | 원본 변경이 복사본에 영향 없음                                |
| 사용 예시 | `Object.assign()`, Spread Operator     | `JSON.stringify()`, `structuredClone()`, Lodash `cloneDeep()` |

---

## ✅ 마무리

- **얕은 복사** (`Object.assign()`, `...`)는 **중첩된 객체가 없을 때만 안전**
- **깊은 복사** (`JSON.parse(JSON.stringify())`, `structuredClone()`, `_.cloneDeep()`)는 **완전히 독립적인 복사를 원할 때 필요**
- 최신 환경에서는 `structuredClone()`을, 모든 환경에서 사용하려면 Lodash의 `cloneDeep()`을 추천
- **객체를 불변하게 만들려면 `Object.freeze()` 대신 `deepFreeze()` 사용**

### 참고자료

[MDN - Object.assign()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)  
[MDN - structuredClone()](https://developer.mozilla.org/ko/docs/Web/API/Window/structuredClone)  
[Lodash cloneDeep()](https://siot0.tistory.com/48)
[코어 자바스크립트 - 1장 데이터타입 (박지섭)](https://github.com/FE-Study-Journey/Core-JS-Study/pull/6)
