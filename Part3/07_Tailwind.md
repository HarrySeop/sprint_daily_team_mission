# 15. Tailwind CSS 사용법 정리

## 📌 개요

이번 주제는 **Tailwind CSS**를 학습하고, **React + Vite** 프로젝트에 적용하는 방법과 주요 사용법을 정리한 것입니다.
Tailwind의 핵심 개념인 **모바일 우선 방식**, **상태(Pseudo-class)**, **반응형 디자인**, **애니메이션과 키프레임 커스터마이징**까지 전반적으로 다룹니다.

---

## 🛠️ 프로젝트 설정 (Vite + React + Tailwind)

### 1. 프로젝트 초기화

```bash
npm create vite@latest my-app -- --template react
cd my-app
npm install
```

### 2. Tailwind CSS 설치

```bash
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
```

### 3. Tailwind 설정 파일 수정

`tailwind.config.js`

```js
module.exports = {
  content: ['./index.html', './src/**/*.{js,ts,jsx,tsx}'],
  theme: {
    extend: {},
  },
  plugins: [],
};
```

### 4. Tailwind 지시어 CSS 작성

`src/index.css` 또는 `main.css`

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

### 5. CSS import

`main.jsx`

```js
import './index.css';
```

---

## ✅ autoprefixer란?

`autoprefixer`는 CSS 속성에 **브라우저 호환용 프리픽스**를 자동으로 붙여주는 PostCSS 플러그인입니다.

예)

```css
/* 원래 */
display: flex;

/* autoprefixer 적용 시 */
display: -webkit-box;
display: -ms-flexbox;
display: flex;
```

Tailwind는 PostCSS 기반이기 때문에, `autoprefixer`가 필수적으로 포함되어야 합니다.

---

## 📱 반응형 디자인

Tailwind는 **모바일 우선 (Mobile First)** 전략을 사용합니다.
기본 스타일은 모든 화면 크기에 적용되며, `sm:`, `md:`, `lg:`, `xl:`, `2xl:`로 점점 큰 화면을 타겟으로 오버라이딩 합니다.

### ✅ 브레이크포인트

| 이름  | 크기 (min-width 기준) |
| ----- | --------------------- |
| `sm`  | 640px                 |
| `md`  | 768px                 |
| `lg`  | 1024px                |
| `xl`  | 1280px                |
| `2xl` | 1536px                |

### ✅ 사용 예시

```html
<div class="text-sm sm:text-base md:text-lg lg:text-xl">반응형 텍스트</div>
```

- 작은 화면: `text-sm`
- 640px 이상: `text-base`
- 768px 이상: `text-lg`
- 1024px 이상: `text-xl`

> 큰 화면용 스타일을 지정하지 않으면 이전 브레이크포인트의 값이 유지됩니다.

---

## 🎨 상태(Pseudo-class) 적용 방법

Tailwind는 상태 기반 스타일을 다음과 같이 사용합니다:

```html
<button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded">Hover me</button>
```

| 상태     | 예시 클래스           |
| -------- | --------------------- |
| hover    | `hover:bg-blue-700`   |
| focus    | `focus:outline-none`  |
| active   | `active:scale-95`     |
| disabled | `disabled:opacity-50` |

---

## 🔁 애니메이션과 키프레임

### 1. 기본 애니메이션

Tailwind에서 기본 제공되는 애니메이션:

```html
<div class="animate-bounce">Bouncing</div>
```

사용 가능한 기본 애니메이션:

- `animate-spin`
- `animate-ping`
- `animate-pulse`
- `animate-bounce`

### 2. 커스텀 키프레임 등록

`tailwind.config.js`

```js
module.exports = {
  theme: {
    extend: {
      keyframes: {
        fadeIn: {
          '0%': { opacity: '0' },
          '100%': { opacity: '1' },
        },
        slideUp: {
          '0%': { transform: 'translateY(20px)', opacity: '0' },
          '100%': { transform: 'translateY(0)', opacity: '1' },
        },
      },
      animation: {
        fadeIn: 'fadeIn 1s ease-out forwards',
        slideUp: 'slideUp 0.6s ease-out forwards',
      },
    },
  },
};
```

### 3. 커스텀 애니메이션 적용

```html
<div class="animate-fadeIn">점점 나타나는 요소</div>
<div class="animate-slideUp">아래에서 올라오는 요소</div>
```

### 4. 상태와 애니메이션 조합 예시

```html
<button class="hover:animate-bounce">Hover시 bounce 애니메이션</button>
```

---

## 🔌 추천 Tailwind 플러그인 (선택 사항)

### ✅ `@tailwindcss/forms`

```bash
npm install -D @tailwindcss/forms
```

```js
plugins: [require('@tailwindcss/forms')],
```

### ✅ `tailwind-scrollbar`

```bash
npm install -D tailwind-scrollbar
```

```js
plugins: [require('tailwind-scrollbar')],
```

### ✅ `prettier-plugin-tailwindcss`

```bash
npm install -D prettier prettier-plugin-tailwindcss
```

```json
// .prettierrc
{
  "plugins": ["prettier-plugin-tailwindcss"]
}
```

---

## ✅ 마무리 체크리스트

| 항목                           | 상태 |
| ------------------------------ | ---- |
| Vite + React 프로젝트 생성     | ✅   |
| Tailwind 설치 및 설정          | ✅   |
| autoprefixer 개념 이해 및 적용 | ✅   |
| 반응형 브레이크포인트 적용     | ✅   |
| 상태(pseudo-class) 사용법 학습 | ✅   |
| 기본 및 커스텀 애니메이션 구현 | ✅   |

---

필요에 따라 반응형 컴포넌트나 실전 적용 예시도 함께 정리하시면 학습 효과가 더욱 높아집니다!
