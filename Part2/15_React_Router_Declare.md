# 🚦 React Router Declare 모드 정리

## ✅ Declare 모드란?

React Router에서 **Declare(선언형) 모드**는 `Route`, `Routes`, `BrowserRouter` 등의 컴포넌트를 **JSX 문법 내에서 선언적으로 작성하여 라우팅을 정의**하는 방식입니다.

React Router v6부터 이 선언형 방식이 더 간결해지고 명확하게 정비되었습니다.

---

## 🔧 기본 구성 예시

```jsx
// App.jsx
import { Routes, Route } from 'react-router-dom';
import Home from './pages/Home';
import About from './pages/About';

function App() {
  return (
    <Routes>
      <Route path="/" element={<Home />} />
      <Route path="/about" element={<About />} />
    </Routes>
  );
}

export default App;
```

```jsx
// main.jsx 또는 index.jsx
import React from 'react';
import ReactDOM from 'react-dom/client';
import { BrowserRouter } from 'react-router-dom';
import App from './App';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <React.StrictMode>
    <BrowserRouter>
      <App />
    </BrowserRouter>
  </React.StrictMode>,
);
```

📌 `BrowserRouter`는 보통 **앱의 진입점(index.js or main.jsx)** 에서 선언합니다. <br/>
전역적으로 라우팅을 관리하기 때문에 가장 바깥에서 감싸야 합니다.

---

## ✅ 특징 및 장점

| 항목              | 설명                                                |
| ----------------- | --------------------------------------------------- |
| 선언형 방식       | JSX 내부에 라우팅 정의 포함 → 가독성 및 구조적 장점 |
| 계층 구조 표현    | 중첩 라우팅 작성이 쉬움 (중첩 `<Route>`)            |
| 동적 라우트 지원  | `:id`와 같은 URL 파라미터 지원                      |
| React 철학에 부합 | 선언형 UI 작성과 일관된 방식                        |

---

## 🧱 중첩 라우팅이란?

중첩 라우팅은 **부모 라우트 내부에서 자식 라우트를 구성하는 방식**입니다. <br/>
페이지 안에서 공통 레이아웃(ex. 사이드바, 헤더)을 유지하면서 자식 컴포넌트가 교체되는 구조를 만들 수 있습니다.

---

## 📦 중첩 라우팅 예시

### 디렉토리 구조 예시

```
src/
├─ pages/
│  ├─ DashboardLayout.jsx
│  ├─ StatsPage.jsx
│  └─ SettingsPage.jsx
├─ App.jsx
```

### App.jsx

```jsx
import { Routes, Route } from 'react-router-dom';
import DashboardLayout from './pages/DashboardLayout';
import StatsPage from './pages/StatsPage';
import SettingsPage from './pages/SettingsPage';

function App() {
  return (
    <Routes>
      <Route path="/dashboard" element={<DashboardLayout />}>
        <Route path="stats" element={<StatsPage />} />
        <Route path="settings" element={<SettingsPage />} />
      </Route>
    </Routes>
  );
}

export default App;
```

### DashboardLayout.jsx

```jsx
import { Outlet, Link } from 'react-router-dom';

function DashboardLayout() {
  return (
    <div>
      <h1>Dashboard</h1>
      <nav>
        <Link to="stats">Stats</Link> | <Link to="settings">Settings</Link>
      </nav>
      <hr />
      <Outlet /> {/* 자식 라우트가 여기에 렌더링됨 */}
    </div>
  );
}

export default DashboardLayout;
```

📌 `<Outlet />`은 중첩 라우트의 **자식 컴포넌트를 렌더링하는 자리**입니다. 중첩된 `<Route>`가 일치할 경우 해당 위치에 렌더링됩니다.

### 동작 흐름

- `/dashboard` → DashboardLayout만 보임 (Outlet 비어 있음)
- `/dashboard/stats` → StatsPage가 Outlet 자리에 렌더링됨
- `/dashboard/settings` → SettingsPage가 Outlet 자리에 렌더링됨

---

## 💡 언제 사용하나요?

- 모든 React 프로젝트에서 권장되는 방식 (특히 `v6` 이후)
- 라우팅 구조를 **JSX에서 명확하게 관리하고 싶을 때**
- 컴포넌트 기반 레이아웃, 공통 UI, 중첩 구조가 필요할 때

---
