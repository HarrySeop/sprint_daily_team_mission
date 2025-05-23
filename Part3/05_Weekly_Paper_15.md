## ✅ [CORS 에러란 무엇이고 어떻게 해결할 수 있나요?]

### ❓ 질문

> CORS 에러에 대해 설명하고, 어떻게 해결하면 될지 설명해 주세요.

### ✅ 개념 설명

**CORS(Cross-Origin Resource Sharing)** 는 웹 브라우저가 **다른 출처(origin)의 리소스를 요청하는 것을 제어하는 메커니즘**입니다.

웹 보안 정책 중 하나인 **SOP(Same-Origin Policy, 동일 출처 정책)** 는 기본적으로 브라우저가 **다른 출처의 리소스에 접근하는 것을 제한**합니다.

- **출처(origin)** = 프로토콜 + 호스트 + 포트
- 예: `https://example.com:443`

SOP 덕분에 악의적인 사이트에서 사용자의 세션 정보나 데이터를 탈취하지 못하게 막을 수 있지만, **프론트엔드와 백엔드 서버가 분리된 환경에서는 정상적인 요청조차 차단될 수 있습니다.**

이를 해결하기 위한 방법이 바로 **CORS**입니다. 서버가 브라우저에 `Access-Control-Allow-Origin` 등의 헤더를 응답에 포함해주면, 브라우저는 다른 출처 간의 요청을 허용합니다.

#### 📌 Credential 포함 요청이란?

사용자가 로그인한 상태처럼 **쿠키, 인증 헤더(Authorization), TLS 인증서 등**이 포함된 요청은 보안상 더 엄격하게 다뤄집니다.

이런 경우 서버는 다음 조건을 충족해야 합니다:

- `Access-Control-Allow-Credentials: true`를 포함
- `Access-Control-Allow-Origin`은 `*`이 아닌, **정확한 출처 문자열이어야 함** (예: `https://myapp.com`)

### 💡 사용 예시

```ts
// 클라이언트에서 쿠키를 포함한 fetch 요청
fetch('https://api.example.com/data', {
  method: 'GET',
  credentials: 'include', // 쿠키 포함
})
  .then(res => res.json())
  .catch(console.error);

// Express 서버에서 CORS 허용
import express from 'express';
import cors from 'cors';

const app = express();

app.use(
  cors({
    origin: 'https://myfrontend.com',
    credentials: true, // 쿠키 허용
  }),
);
```

### 🛠 해결 방법

| 상황                                     | 해결 방법                                                      |
| ---------------------------------------- | -------------------------------------------------------------- |
| 개발 환경에서 CORS 에러 발생             | 백엔드 서버에서 `cors` 설정 추가 (origin, credentials 등 명시) |
| 브라우저에서 OPTIONS preflight 요청 막힘 | 서버가 `OPTIONS` 메서드 요청에 대한 응답도 처리하도록 설정     |
| React 앱에서 백엔드 API 요청             | 로컬 개발 시 `vite.config.ts` 또는 `proxy` 설정으로 우회 가능  |

### ✅ 정리 요약

- CORS는 브라우저 보안을 위한 **출처 간 요청 제한 정책을 완화**하는 방식
- SOP로 인해 다른 출처의 요청이 막힐 수 있으며, 이를 해결하려면 서버 설정이 필요
- **쿠키/인증 포함 요청**은 추가 설정(`credentials`)이 반드시 필요

---

## ✅ [SEO란 무엇이고 어떻게 개선할 수 있나요?]

### ❓ 질문

> SEO가 무엇인지 설명하고, 개선을 위해 어떤 작업을 할 수 있을지 설명해 주세요.

### ✅ 개념 설명

**SEO(Search Engine Optimization)** 는 웹사이트를 **검색 엔진에 더 잘 노출되도록 최적화**하는 작업입니다.

검색 엔진(구글, 네이버 등)은 웹사이트의 구조, 콘텐츠, 메타 정보 등을 기반으로 검색 순위를 결정합니다. **SEO가 잘 되어 있으면 검색 결과 상위에 노출되어 트래픽과 인지도를 높일 수 있습니다.**

React와 같은 SPA(싱글 페이지 애플리케이션)는 자바스크립트로 콘텐츠를 렌더링하므로 **검색 엔진에 노출이 어렵습니다**. 이때 SSR(서버 사이드 렌더링), SSG(정적 사이트 생성) 등이 효과적입니다.

### 💡 사용 예시 (Next.js 기반 SSR)

```tsx
import Head from 'next/head';

export default function Home() {
  return (
    <>
      <Head>
        <title>StarSync - 팬과 아티스트를 잇는 플랫폼</title>
        <meta name="description" content="StarSync는 팬덤 기반 크라우드 펀딩 플랫폼입니다." />
        <meta property="og:title" content="StarSync" />
        <meta property="og:image" content="https://starsync.wiki/thumbnail.png" />
      </Head>
      <main>...</main>
    </>
  );
}
```

### 📈 SEO 개선 체크리스트

| 항목             | 설명                                                       |
| ---------------- | ---------------------------------------------------------- |
| 메타 태그 설정   | title, description, og:image, twitter card 등              |
| Semantic HTML    | `<header>`, `<main>`, `<article>`, `<footer>` 등 태그 사용 |
| SSR/SSG 적용     | React 앱에서 Next.js, Astro 등 활용                        |
| sitemap.xml 제공 | 검색 엔진에 페이지 구조 알려주는 지도                      |
| robots.txt       | 크롤링 허용/제한 경로 설정                                 |
| 접근성 고려      | 이미지 `alt`, heading 구조 등도 SEO에 영향                 |

### ✅ 정리 요약

- SEO는 검색엔진에 잘 노출되기 위한 **콘텐츠 구조화 및 정보 제공 최적화** 작업
- SSR, 메타 태그, 시맨틱 태그, 접근성, 링크 구조 등 다양한 요소가 영향을 미침
- React 앱에서는 Next.js 등 **렌더링 방식을 고려한 설계**가 필요함
