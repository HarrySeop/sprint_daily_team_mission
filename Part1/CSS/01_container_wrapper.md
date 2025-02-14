# 🌟 Container vs Wrapper 쉽게 이해하기

> 스프린트 첫 시작 데일리 팀 미션이기도 하고, 비전공자분들도 있어서 어려운 부분보단 이론을 설명해 보려고 합니다.

스프린트 1일 차 강의를 수강하셨다면, `<div>` 태그에 직접 `style`을 적용하는 예제를 보셨을 겁니다. <br/>
다만, 첫번째 **\<div>** 에는 width값을 지정하고, 두번째 **\<div>** 에는 다른 속성값을 지정해서 이해하기 어려웠다고 생각해요.<br/>

~~절대로 제가 처음 CSS를 배울 때 어려워했어서 해당 주제를 선택한 것은 안 비밀입니다.~~

그래서 2일차는 `CSS 핵심 개념` 파트를 수강하기 때문에 `Container(컨테이너)`와 `Wrapper(랩퍼)`의 차이를 쉽게 설명해볼게요. 😊

## ✅ Container와 Wrapper의 차이

웹 페이지는 여러개의 Box로 구성 되어있습니다.

> 박스, 그리드 등등 여러 속성을 배우겠지만 제일 작게보면 박스 크기로 영역을 가지고 있습니다.

이때 어떤 요소를 감싸는지에 따라 `Container`와 `Wrapper`로 구분할 수 있습니다.

| 개념      | 역할                         | 설명                                      |
| --------- | ---------------------------- | ----------------------------------------- |
| Container | 여러 요소를 담는 큰 틀       | display: flex/grid를 사용해 정렬하는 역할 |
| Wrapper   | 특정 요소를 감싸는 작은 상자 | 개별 요소를 감싸며 스타일을 적용          |

**쉽게 생각하면?**

- `container` = 여러 요소를 정리하는 큰 틀 🏠
- `wrapper` = 특정 요소를 감싸는 작은 상자 📦

## ✅ Container와 Wrapper의 적용 예시

```html
<!-- 전체 리스트를 감싸는 큰 틀 (Container) -->
<ul class="items-container">
  <!-- 개별 아이템을 감싸는 작은 틀 (Wrapper) -->
  <li class="item-wrapper">
    <!-- 실제 콘텐츠 (Item) -->
    <div class="item">Item 1</div>
  </li>
  <li class="item-wrapper">
    <div class="item">Item 2</div>
  </li>
  <li class="item-wrapper">
    <div class="item">Item 3</div>
  </li>
</ul>
```

오늘이 발렌타인 데이니까 초콜릿으로 예시를 들면 아래와 같습니다. <br/>
<img src="https://i.namu.wiki/i/Lfwp0UUHS2fsFJnN9SbYfzki2NYd3QDWtJsnzQHAWkUyohTiku5jQI-Lz7N_hZxTRV0Kg-R5pFNy7nJ4mcd3xA.webp" style="width: 300px">
<img src="https://www.ferrerorocher.com/kr/sites/ferrerorocher20_kr/files/2022-11/t4_0.png?t=1732264293" style="width: 300px; height: 170px" > <br/>

- (첫번째 이미지 오른쪽)페로로 로쉐 초콜릿 -> `Item`
- (첫번째 이미지 왼쪽)개별 포장된 페로로 로쉐 -> `Wrapper`
- (두번째 이미지) 5개입 되어있는 기프트팩 -> `Container`

## ✅ Container와 Wrapper라는 키워드를 왜 알아야 하나요?

왜케 키워드에 집착하냐고 할 수 있습니다. <br/>
프로그래밍은 혼자 하는 것이 아니고 여러명과 같이 하는 것이다 보니 조직간의 스타일 가이드, 커밋컨벤션등 여러 규칙이 존재합니다.

```css
/* 큰 틀 - Container */
.items-container {
  display: flex;
  justify-content: space-between;
  padding: 20px;
  background-color: #f0f0f0;
}

/* 작은 틀 - Wrapper */
.item-wrapper {
  padding: 10px;
  border: 1px solid #ccc;
}

/* 실제 콘텐츠 */
.item {
  background-color: lightblue;
  padding: 20px;
  text-align: center;
}
```

변수명만 보고도 `아? 이건 이거겠구나`라는 생각이 들게 만드는 것이 클린코딩의 첫번째 과제라고 생각합니다. <br/>
~~저는 변수명 짓는게 제일 어렵습니다.~~

하지만 사람들마다 Container와 Wrapper를 사용하는 방식과 관점이 달라서 해당 주제를 맹목적으로 믿으면 안됩니다.<br/>
(저도 예전에는 Wrapper와 Container를 반대로 사용했습니다. - 팀 컨벤션에 맞췄기 때문입니다.)<br/>
그래도 개인적인 연습과 프로젝트를 할 때는 본인이 설명할 수 있는 근거에 맞는 변수명과 코드를 작성하는 것이 좋다고 생각합니다. <br/>

### 출처

[Container vs Wrapper - 스택오버플로우](https://stackoverflow.com/questions/4059163/css-language-speak-container-vs-wrapper)
