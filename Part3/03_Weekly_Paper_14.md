# 🛠️ 위클리페이퍼 - React vs Next.js (SSR & Hydration)

## 🇶 리액트만 사용할 때와 비교해 Next.js를 사용하는 이유에 대해 설명해 주세요.

### 📌 React만 사용할 경우

- CSR(Client Side Rendering)이 기본
- 라우팅, 코드 스플리팅, 데이터 패칭 등을 직접 구성해야 함
- SEO(Search Engine Optimization)에 취약 (초기 HTML이 비어 있음)
- 초기 로딩 속도가 상대적으로 느림 (JS 로딩 후 렌더링)

### ✅ Next.js의 장점

| 기능                    | React                           | Next.js                                      |
| ----------------------- | ------------------------------- | -------------------------------------------- |
| 라우팅                  | 수동 설정 (react-router-dom 등) | 파일 기반 자동 라우팅 (`pages` 디렉토리)     |
| 서버 사이드 렌더링(SSR) | 직접 구현 필요                  | ✅ 내장 지원                                 |
| 정적 사이트 생성(SSG)   | ✖️                              | ✅ `getStaticProps` / `getStaticPaths`       |
| SEO                     | ❌ 불리함                       | ✅ 초기 HTML 포함 가능 → SEO 친화적          |
| 성능 최적화             | ✖️ 수동 튜닝 필요               | ✅ 이미지 최적화, 자동 코드 스플리팅 등      |
| 풀스택 개발             | ✖️ 별도 API 서버 필요           | ✅ API Routes 기능으로 백엔드 코드 작성 가능 |
| 파일 기반 라우팅        | ❌                              | ✅ `pages/index.tsx` → `/` 자동 매핑         |

### 🤔 우리 팀의 선택: **React 단독 사용**

우리 팀은 SSR(서버 사이드 렌더링) 기능이 필요 없는 **CSR 기반 SPA** 프로젝트이기 때문에 Next.js의 주요 기능이 **필수적이지 않다**고 판단했습니다.

- API는 별도 백엔드에서 처리 예정
- 페이지 수가 많지 않아 라우팅 구성도 복잡하지 않음
- SEO 요구사항이 없으며, 로그인 기반의 내부 서비스
- 초기 설정과 번들 커스터마이징에 대한 제어가 React + Vite 조합이 더 편리함

> ✅ 따라서, **React + Vite** 조합으로 더 가볍고 유연하게 구성하는 것이 적합한 선택입니다.

---

## 🇶 Next.js에서 SSR을 실행하는 과정과 hydration에 대해 설명해 주세요.

### ✅ SSR (Server Side Rendering) 동작 과정

1. 사용자가 브라우저에서 URL을 입력하거나 페이지 이동 시도
2. Next.js 서버가 해당 요청을 받고 `getServerSideProps()`를 실행
3. 데이터 fetch 이후 HTML을 서버에서 먼저 생성
4. 서버가 완성된 HTML을 클라이언트에 전달
5. 브라우저는 HTML을 화면에 렌더링하고 JS 파일도 다운로드
6. JS가 로드되면 React가 기존 HTML에 이벤트를 연결하고 **hydrate** (수분 공급)

### ✅ Hydration이란?

- 서버에서 생성된 HTML을 브라우저에서 React가 takeover하는 과정
- HTML은 이미 렌더링되어 있어 사용자에게 빠르게 표시됨
- JS가 로딩되면 React가 기존 DOM 구조와 virtual DOM을 연결 (차이 확인)
- 이로써 동적 인터랙션이 가능해짐 (ex: 클릭, 입력 등)

---

## ✅ SSG (Static Site Generation) 설명

### 🔍 정적 사이트 생성이란?

- **빌드 시점**에 HTML을 미리 생성하여 배포하는 방식
- 정적 자산은 CDN을 통해 빠르게 서빙되며 SSR보다 서버 부하가 없음

### ✅ 사용 방식

- Next.js에서는 `getStaticProps()`와 `getStaticPaths()`를 통해 구현

```ts
export async function getStaticProps() {
  const data = await fetchData();
  return { props: { data } };
}
```

### ✅ React에서 SSG를 하는 방법

React 단독으로는 Next.js처럼 정적 페이지를 자동으로 생성하지 않지만, 다음과 같은 도구를 활용하여 SSG를 구현할 수 있습니다:

#### 1. **Vite + @vitejs/plugin-react + Static Export**

- `vite-plugin-ssg` 또는 `vite-plugin-static-site` 같은 도구 사용
- 빌드시 HTML을 미리 생성할 수 있음

#### 2. **Gatsby 사용** (React 기반 정적 사이트 생성기)

- React 생태계 안에서 정적 페이지 생성에 특화된 프레임워크
- 빌드시 각 페이지를 미리 HTML로 출력

#### 예시: `vite-plugin-ssg`

```ts
// main.tsx
import { renderToString } from 'react-dom/server';
import App from './App';

export function render() {
  return renderToString(<App />);
}
```

```ts
// vite.config.ts
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';
import ssr from 'vite-plugin-ssg';

export default defineConfig({
  plugins: [react(), ssr()],
});
```

> ✅ React에서의 SSG는 Next.js처럼 내장된 기능은 아니지만, **추가 설정과 도구**를 통해 충분히 구현이 가능합니다.

### ✅ SSG 장점

- 빠른 응답 속도 (CDN 캐싱 가능)
- 트래픽이 많아도 서버 부하 없음
- 복잡한 서버 연산 필요 없는 콘텐츠에 적합 (블로그, 문서 등)

### ❗주의사항

- 콘텐츠 업데이트 시 재배포 필요
- 동적 데이터에는 부적합 (로그인, 개인화 등)

---

## 🔍 CSR vs SSR vs SSG 비교

| 항목           | CSR  | SSR                | SSG             |
| -------------- | ---- | ------------------ | --------------- |
| 초기 응답 속도 | 느림 | 빠름               | 매우 빠름       |
| SEO 대응       | 약함 | 강함               | 강함            |
| 서버 부하      | 없음 | 요청마다 HTML 생성 | 없음 (CDN 활용) |
| 실시간성       | 가능 | 가능               | ✖️ 불가         |

> 📌 우리 팀은 실시간성, 로그인 기반, 빠른 개발 환경에 초점을 맞추고 있기 때문에 CSR + 클라이언트 중심 렌더링이 가장 적절합니다.
