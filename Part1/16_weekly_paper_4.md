# 🌟 위클리 페이퍼 4

## 💡 자바스크립트 this에 대해 설명해 주세요.

[코어 자바스크립트 스터디 - 3장 this - 박지섭](https://github.com/FE-Study-Journey/Core-JS-Study/blob/main/03_this/%EB%B0%95%EC%A7%80%EC%84%AD.md)<br/>
여기 읽어주시면 됩니다!

## 💡 렉시컬 스코프(Lexical scope)에 대해 설명해 주세요.

### 📌 렉시컬 스코프란?

- **함수가 선언된 위치**에 따라 스코프가 결정됨
- 실행될 때가 아니라 **정의된 위치를 기준으로 스코프가 결정됨**

### 📌 예제

```javascript
function outer() {
  let outerVar = 'I am outer';

  function inner() {
    console.log(outerVar); // "I am outer"
  }
  inner();
}
outer();
```

✅ **결론**: 내부 함수 `inner`는 선언된 위치를 기준으로 `outerVar`를 접근 가능함.

### 참고자료

[코어 자바스크립트 스터디 - 2장 실행 컨텍스트 - 박지섭](https://github.com/FE-Study-Journey/Core-JS-Study/blob/main/03_this/%EB%B0%95%EC%A7%80%EC%84%AD.md)
