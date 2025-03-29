# 🌟 위클리 페이퍼 6

## ❓ 예시의 코드를 실행할 때, 콘솔에 출력될 값과 그 이유를 설명해 주세요.

```javascript
// 1번
let num = 1;

// 2번
setTimeout(() => {
  num = 2;
}, 0);

// 3번
num = 3;

// 4번
console.log(num);
```

### ✅ 콘솔 출력값: `3`

### 📌 이유:

- `setTimeout`은 비동기 함수입니다. 콜백은 **Web API 영역에서 대기한 뒤**, 호출 스택이 비워질 때 **Task Queue → Call Stack으로 이동**하여 실행됩니다.
- `setTimeout(..., 0)`은 0ms 후에 실행하라는 의미가 아니라, **최소한의 대기 시간 이후 실행**이라는 뜻입니다.
- 따라서, `setTimeout()`이 등록된 후 다음 줄인 `num = 3;`이 먼저 실행되고, 바로 `console.log(num);`이 출력됩니다.
- 그 이후 `num = 2`가 실행되지만, 이미 `console.log()`는 출력된 뒤입니다.

### 🔁 실행 순서 정리:

1. `num = 1` → 초기화
2. `setTimeout(...)` → Web API에 등록 (콜백 대기)
3. `num = 3` → 즉시 실행
4. `console.log(num)` → 3 출력
5. 콜 스택이 비면 `num = 2` → 비로소 실행됨 (출력은 없음)

## ❓ AJAX에 대해 설명해 주세요.

### ✅ 정의

- AJAX는 **Asynchronous JavaScript and XML**의 약자로,
- **페이지 전체를 새로 고치지 않고**, JavaScript를 통해 **비동기적으로 서버와 통신**하여 데이터를 주고받는 기술입니다.
- 웹 페이지의 일부만을 업데이트할 수 있어 사용자 경험(UX)을 크게 향상시킵니다.

### ✅ AJAX의 핵심 원리

1. 브라우저에서 JavaScript가 서버로 HTTP 요청을 보냄
2. 서버는 응답 데이터를 JSON/XML 등으로 반환
3. 클라이언트는 해당 데이터를 받아 DOM을 변경하여 UI를 갱신

### ✅ AJAX의 진짜 의미

- AJAX는 **특정한 API를 의미하는 것이 아니라**, **비동기 데이터 통신 기법 전체를 의미하는 상위 개념**입니다.
- AJAX의 초기 구현은 `XMLHttpRequest`였지만, 이후 `fetch`, `axios` 등 다양한 방식이 AJAX 기법을 따르며 발전해 왔습니다.

---

## 🔁 AJAX 관련 기술 비교

AJAX 요청은 다양한 방식으로 구현할 수 있습니다. 여기서는 `XMLHttpRequest`, `fetch`, `axios`를 중심으로 비교합니다.

### ✅ XMLHttpRequest

- 가장 오래된 AJAX 구현 방식
- 비동기 통신을 처음으로 구현한 브라우저 내장 객체
- 콜백 기반으로 사용되며, 코드가 복잡하고 유지보수 어려움
- 현재는 대부분 레거시 시스템에서만 사용

```js
const xhr = new XMLHttpRequest();
xhr.open('GET', 'https://api.example.com/data');
xhr.onload = function () {
  if (xhr.status === 200) {
    console.log(JSON.parse(xhr.responseText));
  }
};
xhr.send();
```

### ✅ fetch()

- 브라우저에 내장된 최신 API (ES6 이후 표준화)
- Promise 기반으로 비동기 흐름을 깔끔하게 처리 가능
- JSON 파싱도 `.json()` 메서드로 명시적으로 수행
- 에러 처리가 직관적이지 않아 주의 필요 (HTTP 오류도 `catch`가 아닌 `.then`으로 감지)

```js
fetch('https://api.example.com/data')
  .then(res => {
    if (!res.ok) throw new Error('Network Error');
    return res.json();
  })
  .then(data => console.log(data))
  .catch(err => console.error(err));
```

### ✅ axios

- 외부 라이브러리 (`npm install axios`)
- 내부적으로 `XMLHttpRequest`를 사용하지만, API는 `fetch`보다 직관적이고 편리
- 자동 JSON 변환, 에러 처리, 요청/응답 인터셉터, 요청 취소 등 다양한 기능 제공
- React, Vue, Node.js 등 다양한 환경에서 많이 사용

```js
import axios from 'axios';

async function getData() {
  try {
    const response = await axios.get('https://api.example.com/data');
    console.log(response.data);
  } catch (error) {
    console.error(error);
  }
}
```

---

## ⚖️ fetch vs axios 비교

| 항목           | fetch                          | axios                                   |
| -------------- | ------------------------------ | --------------------------------------- |
| 설치 필요 여부 | ❌ 브라우저 내장               | ✅ 별도 설치 필요 (`npm install axios`) |
| 응답 JSON 파싱 | 수동 `.json()` 호출 필요       | 자동 파싱 (`response.data`)             |
| 에러 처리      | HTTP 에러도 `.then`으로 처리됨 | `try/catch`에서 더 직관적               |
| 요청 취소      | 지원하지만 복잡함              | `CancelToken` API로 편리하게 사용 가능  |
| 지원 브라우저  | 모든 최신 브라우저             | 모든 주요 브라우저 + 구형도 일부 지원   |

---

## ✅ 마무리 정리

| 항목                 | 설명                                                           |
| -------------------- | -------------------------------------------------------------- |
| `setTimeout(..., 0)` | 최소 지연 후 Task Queue에 등록되므로 동기 코드는 먼저 실행됨   |
| AJAX                 | JavaScript를 활용해 페이지 리로드 없이 서버와 비동기 통신 가능 |
| XMLHttpRequest       | AJAX의 원형, 콜백 기반, 레거시 호환용                          |
| fetch API            | 최신 AJAX 구현 방식으로 Promise 기반 지원                      |
| axios                | 더 나은 에러 처리와 기능 확장을 제공하는 인기 있는 라이브러리  |

---

## 📚 참고하면 좋은 래퍼런스

- [Ajax와 Axios의 차이점?](https://hstory0208.tistory.com/entry/%EB%B9%84%EB%8F%99%EA%B8%B0-%ED%86%B5%EC%8B%A0-Ajax%EC%99%80-Axios%EC%9D%98-%EC%B0%A8%EC%9D%B4%EC%A0%90)
