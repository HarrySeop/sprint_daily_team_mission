# 🌟 위클리 페이퍼 3

## 🇶 `var`, `let`, `const`의 중복 선언 허용, 스코프, 호이스팅 비교

### 📌 중복 선언 허용 여부

| 키워드  | 중복 선언 허용 여부     |
| ------- | ----------------------- |
| `var`   | ✅ 가능                 |
| `let`   | ❌ 불가능 (SyntaxError) |
| `const` | ❌ 불가능 (SyntaxError) |

```javascript
var a = 1;
var a = 2; // 가능

let b = 1;
let b = 2; // 오류 (SyntaxError: Identifier 'b' has already been declared)

const c = 1;
const c = 2; // 오류 (SyntaxError: Identifier 'c' has already been declared)
```

### 📌 스코프(Scope)

| 키워드  | 스코프                      |
| ------- | --------------------------- |
| `var`   | 함수 스코프(Function Scope) |
| `let`   | 블록 스코프(Block Scope)    |
| `const` | 블록 스코프(Block Scope)    |

```javascript
function test() {
  if (true) {
    var x = 10;
    let y = 20;
    const z = 30;
  }
  console.log(x); // 10 (var는 함수 스코프)
  console.log(y); // 오류 (ReferenceError: y is not defined)
  console.log(z); // 오류 (ReferenceError: z is not defined)
}
```

### 📌 호이스팅(Hoisting)

| 키워드  | 호이스팅 여부 | 초기화 여부                |
| ------- | ------------- | -------------------------- |
| `var`   | ✅ 호이스팅됨 | `undefined`로 초기화       |
| `let`   | ✅ 호이스팅됨 | 초기화되지 않음 (TDZ 발생) |
| `const` | ✅ 호이스팅됨 | 초기화되지 않음 (TDZ 발생) |

```javascript
console.log(a); // undefined
var a = 10;

console.log(b); // 오류 (ReferenceError: Cannot access 'b' before initialization)
let b = 20;

console.log(c); // 오류 (ReferenceError: Cannot access 'c' before initialization)
const c = 30;
```

✅ **결론**:

- `var`는 **함수 스코프**를 가지며, 중복 선언이 가능하고 `undefined`로 초기화됨.
- `let`, `const`는 **블록 스코프**를 가지며, 중복 선언이 불가능하고 **호이스팅되지만 TDZ(Temporal Dead Zone)** 가 발생함.
- `const`는 재할당이 불가능하며 선언과 동시에 초기화해야 함.

---

## 🇶 웹 브라우저 요청 흐름

### 📌 웹 브라우저에서 URL을 입력하고 엔터를 누르면 어떤 일이 일어날까?

1️⃣ **웹 브라우저가 URL을 해석**

- 예: `https://www.google.com/search?q=hello&hl=ko`
- 해석된 정보:
  - `scheme`: `https`
  - `host`: `www.google.com`
  - `port`: `443` (생략, https의 기본 포트)
  - `path`: `/search`
  - `query`: `q=hello&hl=ko`

2️⃣ **DNS를 통해 IP 주소 조회**

- 웹 브라우저는 **DNS(Domain Name System)** 에 `www.google.com`의 IP 주소를 요청
- DNS 서버가 해당 IP 주소를 응답

3️⃣ **서버에 HTTP 요청 전송**

- 브라우저는 얻은 IP 주소와 포트 번호를 사용하여 서버에 HTTP 요청을 보냄
- 요청 예시:
  ```http
  GET /search?q=hello&hl=ko HTTP/1.1
  Host: www.google.com
  ```
- 요청은 **출발지 IP 주소, 목적지 IP 주소, 출발지 포트, 목적지 포트** 를 포함하여 전송됨

4️⃣ **서버가 요청을 처리하고 응답 반환**

- 서버는 요청을 받아서 응답 생성
- 응답 예시:

  ```http
  HTTP/1.1 200 OK
  Content-Type: text/html; charset=UTF-8
  Content-Length: 1234

  <!DOCTYPE html>
  <html>
    <body>
      ...
    </body>
  </html>
  ```

5️⃣ **웹 브라우저가 응답을 받아 화면에 표시**

- HTML을 파싱하여 **DOM(Document Object Model)** 생성
- CSS를 파싱하여 **CSSOM(CSS Object Model)** 생성
- 두 개를 합쳐 **렌더 트리(Render Tree) 생성**
- 화면의 레이아웃을 계산하고 **페인팅(Painting)** 수행

---

## ✅ 정리

- **URI(Uniform Resource Identifier)** 는 리소스를 식별하기 위한 통합된 방법
- **URL(Uniform Resource Locator)** 은 리소스의 위치를 나타냄
- 브라우저는 URL을 해석한 후, **DNS에 IP 주소를 요청**하여 해당 서버에 HTTP 요청을 보냄
- 서버는 요청을 처리하고 적절한 응답을 반환하며, 브라우저는 이를 렌더링하여 사용자에게 표시

### 참고자료

[방예혁 - 모든 개발자들을 위한 HTTP 웹 기본 지식 정리 자료](https://github.com/YehyeokBang/TIL/blob/main/HTTP/%EB%AA%A8%EB%93%A0%20%EA%B0%9C%EB%B0%9C%EC%9E%90%EB%A5%BC%20%EC%9C%84%ED%95%9C%20HTTP%20%EC%9B%B9%20%EA%B8%B0%EB%B3%B8%20%EC%A7%80%EC%8B%9D/2.uri-webbrowser.md)
