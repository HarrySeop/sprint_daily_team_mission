# 🎨 Emotion이란?

Emotion은 **CSS-in-JS 방식의 스타일링 라이브러리**로, React와 같은 컴포넌트 기반 UI 라이브러리와 함께 사용하기 좋습니다.

---

## ✅ Emotion의 특징

### 📌 핵심 키워드

- **CSS-in-JS**: 자바스크립트 코드 안에서 CSS를 작성
- **컴포넌트 단위 스타일 관리**: 컴포넌트와 스타일이 함께 유지됨
- **고성능 런타임**과 **정적 추출 지원**
- React 19, JS 환경과 호환성 우수

### 💡 주요 사용 방식

- `css` 함수 사용
- `css prop` 사용 (Emotion의 특징)

### 📦 설치

```bash
npm install @emotion/react
```

---

## 📌 `/** @jsxImportSource @emotion/react */` 주석의 역할

Emotion에서 `css prop`을 사용할 때는 JSX에서 이 prop을 인식할 수 있도록 해당 주석을 파일 상단에 작성해야 합니다.

```js
/** @jsxImportSource @emotion/react */
```

이 주석은 Emotion이 JSX 내부의 `css={...}` 구문을 자동으로 변환하기 위해 **Emotion 전용 jsx 함수로 컴파일하도록 지시**합니다.<br/>
이를 생략하면 `css prop`이 제대로 적용되지 않거나 경고가 발생할 수 있습니다.

### ❓ 만약 이 주석을 쓰지 않는다면?

1. **Babel 설정이 필요합니다**
   - Emotion의 `css prop`을 Babel 수준에서 처리하기 위한 설정을 직접 추가해야 합니다.
   - `babel-plugin-emotion`을 설치하고 `.babelrc` 또는 `babel.config.js`에 다음과 같이 설정:

```bash
npm install --save-dev babel-plugin-emotion
```

```js
// babel.config.js
module.exports = {
  presets: ['@babel/preset-react'],
  plugins: ['@emotion'],
};
```

✅ 결론: `css prop`을 사용하려면 `@emotion/react`의 jsx 기능을 명확하게 인식시켜야 하며, 가장 간단하고 권장되는 방식은 **주석을 사용하는 방법**입니다.

---

## 🧪 사용 예시

### 1. `css prop` + 외부 css 객체 활용 예시

#### 📁 styles/buttonStyle.js

```js
import { css } from '@emotion/react';

export const buttonStyle = css`
  background-color: skyblue;
  padding: 10px 20px;
  border-radius: 8px;
  font-weight: bold;
  color: white;
  border: none;
`;
```

#### 📁 components/Button.jsx

```jsx
/** @jsxImportSource @emotion/react */
import { buttonStyle } from '../styles/buttonStyle';

function Button() {
  return <button css={buttonStyle}>Styled Button</button>;
}
```

✅ 이 방식은 **스타일 재사용성**과 **구조화된 코드 관리**에 유리합니다.

---

## 💬 css prop 이란?

- Emotion에서 지원하는 **JSX 속성 형태의 스타일 적용 방법**
- 일반 HTML 속성처럼 `css={...}` 형태로 스타일을 적용
- React 컴포넌트에 바로 스타일을 덧붙일 수 있어 **간결하고 재사용성 높음**

---

## ✅ 반응형 스타일링 - facepaint 사용 예시

Emotion은 자체적으로 미디어 쿼리를 지원하지 않으므로, 반응형 스타일링에는 `facepaint` 라이브러리를 함께 사용하는 것이 좋습니다.

### 📦 설치

```bash
npm install facepaint
```

### ✅ 공통 유틸로 mq 분리

#### 📁 styles/mediaQuery.js

```js
import facepaint from 'facepaint';

const breakpoints = [480, 768, 1024];
export const mq = facepaint(breakpoints.map(bp => `@media (min-width: ${bp}px)`));
```

### ✅ 컴포넌트에서 사용 예시

#### 📁 styles/responsive.js

```js
import { css } from '@emotion/react';
import { mq } from './mediaQuery';

export const containerStyle = css`
  display: flex;
  flex-direction: column;
`;

export const responsiveText = mq({
  fontSize: ['14px', '18px', '22px'],
  color: ['blue', 'green', 'red'],
});

export const responsiveBox = mq({
  width: ['100%', '80%', '60%'],
  backgroundColor: ['#eee', '#ddd', '#ccc'],
  padding: ['10px', '20px', '30px'],
});
```

#### 📁 components/ResponsiveBox.jsx

```jsx
/** @jsxImportSource @emotion/react */
import { containerStyle, responsiveText, responsiveBox } from '../styles/responsive';

function ResponsiveBox() {
  return (
    <div css={containerStyle}>
      <div css={responsiveBox}>
        <p css={responsiveText}>반응형 텍스트와 박스입니다.</p>
      </div>
    </div>
  );
}
```

✅ `facepaint`를 사용하면 **브레이크포인트별로 속성을 배열 형태로 선언**하여 반응형 스타일을 간결하게 작성할 수 있습니다.

✅ `mq`는 각 컴포넌트 파일에서 import해서 사용하는 방식이 일반적이며, **공통 모듈로 분리하여 재사용**합니다.

---

## 📚 인라인 스타일 vs CSS 객체 분리

### 📌 Emotion 인라인 스타일 작성 방식

```jsx
/** @jsxImportSource @emotion/react */
import { css } from '@emotion/react';

function InlineExample() {
  return (
    <div
      css={css`
        color: navy;
        font-size: 20px;
        padding: 8px;
      `}>
      Emotion 인라인 스타일 예시
    </div>
  );
}
```

### 📌 CSS 객체 분리 (Emotion 기준)

```js
// styles/textStyle.js
import { css } from '@emotion/react';

export const textStyle = css`
  color: navy;
  font-size: 20px;
  padding: 8px;
`;
```

```jsx
/** @jsxImportSource @emotion/react */
import { textStyle } from '../styles/textStyle';

function SeparatedExample() {
  return <div css={textStyle}>분리된 스타일 예시</div>;
}
```

### 차이점 비교

| 항목                   | Emotion 인라인 스타일 | CSS 객체 분리 (Emotion 등) |
| ---------------------- | --------------------- | -------------------------- |
| 코드 재사용성          | 낮음                  | 높음                       |
| 조건부 스타일링        | 가능                  | 가능                       |
| 미디어 쿼리, keyframes | 가능                  | 가능                       |
| 자동 클래스 해시 생성  | 있음                  | 있음                       |
| 스타일 분리 및 관리    | 어려움                | 쉬움 (모듈화 가능)         |

---

## ✅ 결론: Emotion을 선택하는 이유

- **React 프로젝트에 최적화된 CSS-in-JS 솔루션**
- 최신 문법, Hook과의 호환성, 타입 지원 등 실무 활용에 강함
- 작은 컴포넌트부터 복잡한 UI까지 적용 가능하며 유지보수가 용이
- `@emotion/styled` 없이도 **`css`와 `css prop`만으로 완전한 스타일링이 가능**
- **반응형도 facepaint와 함께 자연스럽게 처리 가능**
- `mq` 유틸을 공통 분리하여 사용하면, 반복 없이 깔끔하게 반응형을 구성할 수 있음

### 조심할 부분

#### Use the css prop or @emotion/styled, but not both

> While the css prop and `styled` can peacefully coexist, it's better to pick one approach and use it consistently across your codebase. Whether you choose the css prop or `styled` is a matter of personal preference. (If you're curious, the css prop is more popular among the maintainers of Emotion.)

> (구글 번역) css prop과 css prop은 `styled` 평화롭게 공존할 수 있지만, 하나의 접근 방식을 선택하여 코드베이스 전체에서 일관되게 사용하는 것이 좋습니다. css prop을 선택하든 `styled` 개인적 선호도에 따라 다릅니다. (궁금하시다면, css prop은 Emotion 유지 관리자들 사이에서 더 인기가 많습니다.)

---

### 🔗 참고 자료

- [Emotion 공식 문서](https://emotion.sh/docs/introduction)
- [Emotion 공식 문서 - Use the css prop or @emotion/styled, but not both](https://emotion.sh/docs/best-practices#use-the-css-prop-or-emotionstyled-but-not-both)
- [Emotion으로 React 컴포넌트 스타일하기](https://www.daleseo.com/emotion)
- [[React] 프로젝트에서의 Emotion 사용](https://velog.io/@page1597/React-Emotion-%EC%82%AC%EC%9A%A9)
