# 🌟 CSS `position` 속성과 특징

> [HTML/CSS] 위클리 페이퍼<br/> 04. position의 속성들과 각각의 특징을 설명해 주세요. <br/> ~~위클리 페이퍼 주제가 바뀐 것 같지만, 정리해보았습니다.~~

## ✅ `position` 속성 개요

### 📌 `position` 속성이란?

- `position` 속성은 요소의 배치 방법을 결정합니다.
- 기본값은 `static`이며, 다른 값들은 특정한 기준을 기반으로 요소를 이동시킬 수 있습니다.
- 주요 값: `static`, `relative`, `absolute`, `fixed`, `sticky`

---

## ✅ `position` 속성의 종류 및 특징

### 📌 1. `static` (기본값)

- 기본적인 문서 흐름에 따라 배치됩니다.
- `top`, `left`, `right`, `bottom` 속성이 적용되지 않습니다.

```css
.element {
  position: static; /* 기본값 */
}
```

✅ **언제 사용할까?**

- 요소를 기본적인 문서 흐름에 맞춰 배치할 때 사용

---

### 📌 2. `relative` (상대 위치)

- **자신의 원래 위치**를 기준으로 `top`, `left`, `right`, `bottom` 값을 이용해 이동할 수 있습니다.
- 원래 위치가 유지되므로 **주변 요소에 영향을 주지 않음**

```css
.element {
  position: relative;
  top: 20px; /* 기존 위치에서 아래로 20px 이동 */
  left: 10px; /* 기존 위치에서 오른쪽으로 10px 이동 */
}
```

✅ **언제 사용할까?**

- 특정 요소를 원래 위치에서 조금만 조정하고 싶을 때 사용
- 부모 요소의 기준점을 잡기 위해 사용 (자식 요소의 `absolute` 배치를 위해)

---

### 📌 3. `absolute` (절대 위치)

- **가장 가까운 `position: relative`가 설정된 부모 요소**를 기준으로 배치됩니다.
- 만약 부모 요소 중 `relative`가 없다면 **`<html>` 요소를 기준**으로 위치가 결정됩니다.
- `top`, `left`, `right`, `bottom` 값을 사용하여 자유롭게 위치 조정 가능

```css
.container {
  position: relative; /* 기준 요소 */
}
.element {
  position: absolute;
  top: 50px; /* 부모 요소 기준으로 50px 아래 이동 */
  right: 20px; /* 부모 요소 기준으로 20px 왼쪽으로 이동 */
}
```

✅ **언제 사용할까?**

- 특정한 부모 요소를 기준으로 배치하고 싶을 때
- 일반적인 문서 흐름에서 벗어나 독립적인 배치를 원할 때

---

### 📌 4. `fixed` (고정 위치)

- **브라우저의 뷰포트(화면)** 를 기준으로 요소가 배치됩니다.
- 스크롤을 내려도 요소가 같은 위치에 고정되어 있음
- `top`, `left`, `right`, `bottom` 속성을 이용해 배치 가능
- 다른 요소들과 겹치는 경우 `margin`을 활용하여 적절한 간격을 조정할 수 있음

```css
.element {
  position: fixed;
  top: 10px;
  right: 10px;
  margin: 20px; /* 다른 요소와 겹치지 않도록 여백 추가 */
}
```

✅ **언제 사용할까?**

- 네비게이션 바, 돌아가기 버튼, 고정된 광고 배너 등에 사용
- `fixed` 요소가 다른 요소와 겹칠 경우 `margin`을 활용하여 간격을 조정하는 것이 좋음

---

### 📌 5. `sticky` (스크롤 따라가다 고정됨)

- `relative`처럼 동작하다가 특정 지점에서 `fixed`처럼 고정됨
- `top`, `left`, `right`, `bottom` 속성으로 고정 위치 지정
- 부모 요소의 영역을 벗어나지 않음

```css
.element {
  position: sticky;
  top: 0;
}
```

✅ **언제 사용할까?**

- 테이블 헤더, 페이지 내 특정 섹션을 스크롤에 맞춰 고정할 때

---

## ✅ `position` 속성 비교

| 속성       | 기준점                      | 스크롤 시 위치 변화 | 사용 예시                |
| ---------- | --------------------------- | ------------------- | ------------------------ |
| `static`   | 기본 문서 흐름              | 이동 없음           | 기본 배치                |
| `relative` | 자기 원래 위치              | 이동 없음           | 기존 위치에서 조정       |
| `absolute` | 가장 가까운 `relative` 부모 | 이동 없음           | 특정 부모 요소 기준 배치 |
| `fixed`    | 뷰포트(화면)                | 항상 고정           | 네비게이션 바, 버튼      |
| `sticky`   | 스크롤 위치에 따라 변동     | 특정 지점에서 고정  | 테이블 헤더, 섹션 제목   |

---

## ✅ 마무리

- `static`: 기본값, 문서 흐름에 따름
- `relative`: 기존 위치를 기준으로 이동
- `absolute`: `relative` 부모 기준으로 배치 (없으면 `html` 기준)
- `fixed`: 뷰포트 기준으로 고정 (겹침 방지를 위해 `margin` 조정 가능)
- `sticky`: 스크롤 시 특정 위치에 고정됨

### 참고자료

[MDN - CSS Position](https://developer.mozilla.org/ko/docs/Web/CSS/position)
