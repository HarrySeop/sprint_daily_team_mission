# 🎨 Emotion 추가 정리

## ✅ 기본 사용법 (CSS 전용)

### 📌 예시 - 인라인 스타일 정의

```jsx
/** @jsxImportSource @emotion/react */
import { css } from '@emotion/react';

const buttonStyle = css`
  padding: 12px 20px;
  background-color: teal;
  border: none;
  border-radius: 8px;
  color: white;
  font-weight: bold;
`;

function Button({ children }) {
  return <button css={buttonStyle}>{children}</button>;
}
```

---

## 📂 스타일 분리 방식

Emotion의 `css` 스타일을 컴포넌트 외부 파일로 분리하여 재사용성과 유지보수를 높일 수 있습니다.

### 1️⃣ 스타일 파일 분리 - 예: `Button.styles.js`

```js
/** @jsxImportSource @emotion/react */
import { css } from '@emotion/react';

export const button = css`
  padding: 12px 20px;
  background-color: teal;
  color: white;
  border: none;
  border-radius: 6px;
  font-weight: bold;
`;
```

### 2️⃣ 컴포넌트에서 불러오기 - `Button.jsx`

```jsx
import * as styles from './Button.styles';

function Button({ children }) {
  return <button css={styles.button}>{children}</button>;
}
```

✅ `import * as styles` 방식은 완벽하게 작동하며, Emotion과 궁합도 매우 좋습니다.

해당 방식은 컴포넌트 전용 스타일만 불러올 때 사용하는 것이 좋습니다. (디렉토리 구조로 나뉘어 있을 때)

---

## 📛 파일명 네이밍 가이드

| 용도                        | 파일명 예시                             | 설명                   |
| --------------------------- | --------------------------------------- | ---------------------- |
| 컴포넌트용 스타일           | `ComponentName.styles.js`               | 가장 일반적인 구성     |
| 전역 스타일                 | `global.styles.js`, `theme.styles.js`   | 리셋, 테마 정의 등     |
| 컴포넌트 + 스타일 함께 쓰기 | `Component.jsx` + `Component.styles.js` | 폴더 내 분리 관리 권장 |

---

## 📦 디렉토리 구조 예시

```
/components
  └─ Button
      ├── Button.jsx
      └── Button.styles.js
```

---
