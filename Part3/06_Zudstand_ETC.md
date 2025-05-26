## ✅ Zustand란 무엇이고 어떻게 사용하나요?

### ✅ 개념 설명

**Zustand**는 React 애플리케이션에서 상태 관리를 쉽게 하기 위한 **경량 상태 관리 라이브러리**입니다.

Redux보다 설정이 간단하고, Context API 없이도 글로벌 상태를 관리할 수 있습니다. 또한 `create` 함수 기반의 사용 방식으로 **보일러플레이트 코드가 거의 없으며**, `middleware`를 통해 **로컬 스토리지 persist, devtools, immer 등과 손쉽게 연동**할 수 있습니다.

### 📌 주요 개념

- **`create`**: 상태 저장소를 생성하는 기본 함수
- **`persist`**: zustand 미들웨어 중 하나로, 로컬스토리지 등에 상태를 자동 저장
- **`getState()`**: 저장소의 현재 상태를 가져오는 메서드

### 💡 사용 예시

```ts
// store/authStore.ts
import { create } from 'zustand';
import { persist } from 'zustand/middleware';
import { useUserStore } from './userStore';

interface AuthState {
  accessToken: string | null;
  isAuthenticated: boolean;
  setAccessToken: (token: string) => void;
  clearAccessToken: () => void;
  logout: () => void;
}

export const useAuthStore = create<AuthState>()(
  persist(
    set => ({
      accessToken: null,
      isAuthenticated: false,
      setAccessToken: token => {
        set({ accessToken: token, isAuthenticated: !!token });
      },
      clearAccessToken: () => set({ accessToken: null, isAuthenticated: false }),

      logout: () => {
        set({ accessToken: null, isAuthenticated: false });
        useUserStore.getState().clearUser();
      },
    }),
    {
      name: 'auth-storage', // 로컬스토리지 키 이름
    },
  ),
);
```

### ✅ 설명 포인트

| 속성/함수명        | 설명                                                 |
| ------------------ | ---------------------------------------------------- |
| `accessToken`      | 로그인 인증 토큰을 저장하는 상태                     |
| `isAuthenticated`  | 인증 여부를 나타내는 불리언 상태                     |
| `setAccessToken`   | 토큰 저장 및 인증 상태를 true로 설정하는 함수        |
| `clearAccessToken` | 토큰 삭제 및 인증 상태를 false로 초기화하는 함수     |
| `logout`           | 상태 초기화 및 사용자 정보 초기화 처리               |
| `persist`          | 상태를 로컬 스토리지에 저장하여 새로고침 후에도 유지 |

### ✅ 정리 요약

- Zustand는 **전역 상태를 간단하게 관리**할 수 있는 React 전용 상태 관리 도구입니다.
- `create`와 `set`, `getState`로 쉽게 상태를 정의하고 사용할 수 있습니다.
- `persist` 미들웨어를 활용하면 **자동 저장 및 불러오기** 기능도 구현 가능
- Redux 대비 훨씬 **간단하고 실용적**이라 중소형 프로젝트에서 특히 유용합니다.

---

## ✅ Next.js의 `<Link>` vs React Router의 `<Link>` 차이점

### ✅ 개념 설명

Next.js와 React Router 모두 `<Link>` 컴포넌트를 사용하여 페이지 간 이동을 구현하지만, 내부 동작 방식과 사용법에는 차이가 있습니다.

| 항목      | Next.js `<Link>`                         | React Router `<Link>`                 |
| --------- | ---------------------------------------- | ------------------------------------- |
| 사용 목적 | 정적/동적 라우팅을 통한 페이지 이동      | SPA 내 라우팅 제어                    |
| 경로 처리 | `pages/` 폴더 기반 경로 자동 생성        | `Route` 컴포넌트와 수동 경로 등록     |
| 내부 구현 | `next/router` 기반, 클라이언트/서버 혼합 | `react-router-dom`의 history API 사용 |
| 필수 속성 | `href`                                   | `to`                                  |

### 💡 사용 예시

```tsx
// Next.js
import Link from 'next/link';

<Link href="/about">About</Link>;
```

```tsx
// React Router
import { Link } from 'react-router-dom';

<Link to="/about">About</Link>;
```

### ✅ 정리 요약

- Next.js의 `<Link>`는 `href`, React Router의 `<Link>`는 `to`를 사용
- Next는 파일 기반 라우팅, React Router는 수동 정의 라우팅
- 둘 다 클라이언트 사이드 네비게이션을 지원하지만, 프레임워크 구조에 따라 차이 존재

---

## ✅ 타입 선언 파일(.d.ts)이란?

### ✅ 개념 설명

`.d.ts` 파일은 **타입스크립트에서 타입 정보를 선언하는 전용 파일**입니다. 실제 코드 구현 없이 타입만 포함되며, 다음과 같은 경우에 사용됩니다:

- 자바스크립트 라이브러리를 타입스크립트에서 사용할 때
- 전역 객체 또는 커스텀 환경 변수 정의 시
- 모듈 보강(augmentation) 또는 외부 모듈 타입 선언 시

타입스크립트는 이러한 선언 파일을 통해 코드 자동완성, 타입 추론, 에러 방지를 가능하게 합니다.

### 💡 사용 예시

```ts
// types/global.d.ts
export {}; // 모듈로 인식시키기 위해 필요

declare global {
  interface Window {
    kakao: any;
  }
}
```

```ts
// 외부 라이브러리 보강
// types/custom-lodash.d.ts
declare module 'lodash' {
  export function chunk<T>(arr: T[], size: number): T[][];
}
```

### ✅ 정리 요약

- `.d.ts`는 **타입 정의 전용 파일**로, 코드 구현 없이 타입만 포함함
- 자바스크립트 라이브러리를 TS에서 사용하거나 전역 객체 정의 등에 사용
- 프로젝트 규모가 커질수록 **타입 안정성과 생산성을 크게 높여줌**
