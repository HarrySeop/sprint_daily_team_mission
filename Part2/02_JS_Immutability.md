# 🔒 JavaScript의 불변성(Immutability)

## ❓ 불변성이란?

- 불변성(Immutability)이란 **한 번 생성된 값은 변경되지 않는다**는 개념입니다.
- 원시 타입(Primitive type)은 기본적으로 불변성을 가집니다.

```javascript
let a = 1;
let b = a;
b = 2;
console.log(a); // 1 (변하지 않음)
```

- 반면, 객체와 배열은 **참조 타입**이기 때문에 변경이 가능합니다.

```javascript
const obj1 = { name: 'Alice' };
const obj2 = obj1;
obj2.name = 'Bob';
console.log(obj1.name); // "Bob" (같은 참조)
```

---

## 🛠️ 불변성을 지키는 방법

### 1. **Spread 연산자 사용**

```javascript
const original = { name: 'Alice', age: 25 };
const copy = { ...original, age: 26 };
console.log(original); // { name: "Alice", age: 25 }
```

### 2. **Object.assign() 사용**

```javascript
const copy = Object.assign({}, original, { age: 26 });
```

### 3. **배열의 불변성 유지 (map, filter, concat 등)**

```javascript
const arr = [1, 2, 3];
const newArr = arr.map(num => num * 2);
console.log(arr); // [1, 2, 3] (원본은 유지)
```

### 4. **깊은 복사(Deep Copy)**

- 중첩된 객체를 불변하게 유지하려면 **깊은 복사**가 필요합니다.

```javascript
const state = {
  user: { name: 'Alice', age: 25 },
};
const newState = {
  ...state,
  user: { ...state.user, age: 26 },
};
```

### 5. **불변성 유지를 돕는 라이브러리 사용**

- `immer`, `immutable.js` 같은 라이브러리를 활용하면 편리합니다.

```javascript
import produce from 'immer';

const nextState = produce(state, draft => {
  draft.user.age = 26;
});
```

### 6. **Object.freeze()와 깊은 동결**

#### ✅ `Object.freeze()`란?

- 객체를 동결시켜 **속성 추가/삭제/수정이 불가능**하게 만듭니다.
- 하지만 **얕게만 동결**합니다 (1단계 속성만 불변).

```javascript
const user = {
  name: 'Alice',
  address: { city: 'Seoul' },
};

const frozenUser = Object.freeze(user);
frozenUser.name = 'Bob'; // 무시됨
frozenUser.address.city = 'Busan'; // 변경됨❗️

console.log(frozenUser); // { name: "Alice", address: { city: "Busan" } }
```

#### ✅ 깊은 동결 (Deep Freeze)

- 중첩된 객체까지 **재귀적으로** `Object.freeze()`를 적용하는 방법

```javascript
function deepFreeze(obj) {
  Object.getOwnPropertyNames(obj).forEach(prop => {
    const value = obj[prop];
    if (value && typeof value === 'object') {
      deepFreeze(value);
    }
  });
  return Object.freeze(obj);
}

const deepUser = {
  name: 'Alice',
  address: { city: 'Seoul' },
};

deepFreeze(deepUser);
deepUser.address.city = 'Busan'; // ❌ 변경 안 됨
console.log(deepUser); // { name: "Alice", address: { city: "Seoul" } }
```

#### 🔍 비교

| 구분           | Object.freeze | 깊은 동결 (deepFreeze) |
| -------------- | ------------- | ---------------------- |
| 적용 범위      | 얕은(1단계만) | 모든 중첩 객체까지     |
| 수정 방지 여부 | 부분적        | 전체                   |
| 기본 제공 여부 | 제공됨        | 직접 구현 필요         |

❗️ 참고: `Object.freeze()`는 **객체의 변경을 막는 목적**으로는 유용하지만, 상태를 복사하며 유지하는 방식과는 다릅니다. 즉, **불변성 유지를 위한 수단이라기보단, 객체를 강제로 고정하는 도구**입니다.

---

## 🤔 왜 불변성을 지켜야 할까?

| 이유                             | 설명                                                                  |
| -------------------------------- | --------------------------------------------------------------------- |
| 상태 추적 용이                   | 이전 상태를 쉽게 복원하거나 비교할 수 있음                            |
| 디버깅/테스트 용이               | 불변 객체는 변경 이력이 없으므로 예측 가능성이 높음                   |
| React와 같은 프레임워크에서 필수 | `useEffect`, `memo`, `PureComponent` 등은 불변성 기반으로 최적화 수행 |
| 참조 비교 가능                   | 참조값 비교만으로 변경 여부를 판단할 수 있음 (`prev !== next`)        |

### ❓ 그럼 Object.freeze를 꼭 사용해야 할까?

- `Object.freeze()`는 객체 자체를 **수정하지 못하도록 보호**하는 데에는 유용하지만,
- **불변성을 유지하는 일반적인 방식(복사 후 수정)** 과는 성격이 다릅니다.
- 대부분의 상황에서는 **spread 연산자나 immutable helper를 통해 상태를 복사하며 변경하는 방식이 더 유연하고 권장**됩니다.

✅ 결론: `freeze()`는 안전장치로 사용할 수 있지만, **불변성 유지를 위한 필수 수단은 아닙니다**.

---

## ✅ 정리

- 객체나 배열과 같은 참조 타입은 변경되기 쉬우므로 **불변성을 유지하는 방법**을 적극적으로 사용해야 합니다.
- Spread 연산자, `Object.assign()`, `map`, `filter`, `immer`, `deepFreeze()` 등을 통해 **상태를 복사하고 수정**하는 방식으로 불변성을 지켜야 합니다.
- `Object.freeze()`는 객체를 강제로 고정할 수 있으나, 상태 복사 기반 불변성 유지에는 부적합할 수 있습니다.

---

### 📚 참고 자료

- [MDN - Immutable](https://developer.mozilla.org/ko/docs/Glossary/Immutable)
- [MDN - Object.assign()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)
- [immer 공식 문서](https://immerjs.github.io/immer/)
