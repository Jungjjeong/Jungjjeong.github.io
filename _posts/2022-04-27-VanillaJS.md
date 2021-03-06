---
layout: single
title: "VanillaJS 세션 마무리"
categories: [devCourse, TIL]
last_modified_at: 2022-04-27
excerpt: "VanillaJS 세션 마무리 소감을 적어보았어요."
header:
  teaser: https://user-images.githubusercontent.com/72294509/165543933-c6cd9305-943b-4991-8742-b66133d425e6.png
---

![header](https://user-images.githubusercontent.com/72294509/165543933-c6cd9305-943b-4991-8742-b66133d425e6.png)

눈 감았다 뜨니 데브코스는 한 달이 지났고, Vanilla JS 과정도 끝이 났다!

> 지금까지 강의 진행해주신 로토님 정말 감사드립니다. 😊

이 글은 소감문(?) 같은 느낌으로 작성해보려 한다.

<br><br><br>

---

기존에 내가 만들어 본 웹들을 보면 코드가 정말 난해하다.

당연할 수 밖에 없다. **설계 → 뼈대 → 코딩 → 리팩터링** 의 과정이 아닌

> 자 일단 짜보자!

![Untitled](https://user-images.githubusercontent.com/72294509/165543999-d9a38203-818c-40b9-b348-81ba10fbb0bd.png)

> 어..근데 왜 안돼..?

![Untitled 1](https://user-images.githubusercontent.com/72294509/165543988-f2f3a41a-acb0-49bd-a508-e6415ed2e098.png)

항상 시작은 이러했으니 제대로 개발이 진행이 될 수가 없었다.

<br>

컴포넌트의 구조? 난 몰라 일단 한 파일에 코드 다 넣고 보자! 아 그.. 라우팅만 분리해볼까..?

문제가 생기면 일단 모든 코드에 `console.log`를 찍고 보면 돼~

개발 진행 안돼? 그럼 깃허브에 코드 검색해봐.. 오픈소스 하나 받아오자.

<br>

라는 어마무시한 마인드들을 가지고 개발을 이어나갔었다.

그래서 나는 포트폴리오에 지금까지 짜온 내 코드들을 적지 못할 것 같다.. 💔 흑흑

<br><br>

# [1] 학습 요약

이번 VanillaJS 강의를 듣고 <span style="background-color:#FFF2D1">알게 된 점</span>을 정리해본다.

<br>

## 1. 독립적인 컴포넌트

일단 <span style="background-color:#FFF2D1">컴포넌트</span>의 개념부터 새로 알았다.

왜 컴포넌트가 <span style="background-color:#FFF2D1">독립적이어야 하는지</span>도 알게 되었다.

<br>

> → 이슈를 해결하기 위해 파일의 모든 곳에 `console.log`를 찍어야 했던 내 고질적인 어려움의 원인을 알았다.

<br>

데이터의 흐름은 컴포넌트 간에 한 방향으로 흐르는 것이 좋다.

어디서 <span style="background-color:#FFF2D1">이슈가 발생했는지 파악</span>이 쉬우며, 해당 컴포넌트가 다루는 <span style="background-color:#FFF2D1">상태가 명확</span>해진다.

<br>

이러한 방식으로 컴포넌트를 설계하고 구현하면

후에 새로운 컴포넌트를 붙이거나 기능을 확장할 때도 용이하다.

<br>

## 2. 상태 관리

컴포넌트가 가지고 있는 `상태`란, 쉽게 말해 `데이터`이다.

화면의 구조를 중심으로 UI를 표현하는 것이 아닌, 상태 즉 **데이터의 흐름을 중심**으로 UI를 표현하는 법에 대해 알 수 있었다.

```jsx
export default function Jiyoung() {
  this.state = [];
  const state = [];
}
```

그리고 갑자기 다른 이야기지만 나는 이 둘의 차이도 몰랐다. (외부에서 접근이 가능한가 하지 못하는가)

상태를 다루며 스코프에 대해서 한번 더 공부할 수 있었다.

<br>

```jsx
export default function TodoList({ $target, initialState, 다양한 함수들 }){
	const $list = document.createElement('ul');
	$target.appendChild($list);

	this.state = initialState;

	this.setState = nextState => {
		this.state = nextState;
		this.render();
	}

	this.render = () => {
		...
	}

	this.render()
}
```

컴포넌트의 뼈대 구조를 손으로 익히고 응용할 줄 알게 되었다.

state를 업데이트 해주는 함수와 render를 따로 다루면 코드를 수정할 때 너무 편했다.

<br>

## 3. 기능 개발이 끝이 아니야.

<span style="background-color:#FFF2D1">기획서에 적혀있지 않은 수많은 UI 개발은 프론트엔드 개발자의 역량</span>이었다.

<br>

에디터의 focus를 어디에 맞춰야 하는지,

키보드의 다양한 키 (ESC, ENTER 등)을 누르면 어떤 동작을 하게 만들 것인지,

부드러운 렌더링을 위해 낙관적 업데이트를 해줄 것인지,

서버의 부하를 어떻게 줄일 것인지,

화면의 요소가 어느 정도 스크롤이 내려갔을 때, 다음 요소를 보여줄 것 인지 등등

<br>

등등의 과정을 로토님이 고민하실 때, 나도 함께 고민하며

아.. 이렇게 <span style="background-color:#FFF2D1">UX를 끊임없이 고민해야 하는 개발자</span>가 바로 프론트엔드 개발자구나 알 수 있었다.

<br>

## 4. App과 main을 왜 분리할까

실습 초기부터 궁금했다.

특히 깃허브의 다양한 오픈소스를 보면 전부 이러한 컴포넌트 구조를 가졌다.

하지만 실습을 따라가는 데 급급하여 궁금해하기만 했다. 드디어 전날 강의에서 알 수 있었다.

- 한 화면에서 App 컴포넌트를 여러 개 생성할 수 있다.
- 컴포넌트를 선언하는 쪽과 실제로 초기화해서 실행하는 쪽을 분리하기 위함이다.<br>
  → 선언과 실행을 분리시키지 않은 스크립트가 되면, 코드를 보기 굉장히 난해하다.<br>
  → 유닛 테스트를 짤 때도, 해당 방식이 훨 편하다.

아하 이래서 로토님이 스크립트를 여러개 달아놓으셨다가 나중에 App으로 감싸고, main으로 감싸셨구나..

마치 정체된 고속도로가 뻥 뚫리듯 시원하게 궁금증을 해결할 수 있었다.

<br>

## 5. 코드 가독성

가독성이 좋은 코드는 코드의 의미, 기능을 파악하기 쉽다.

따라서 <span style="background-color:#FFF2D1">가독성이 좋으면 협업하기 좋은 코드라 할 수 있다고 생각</span>한다. (물론 부가적인 조건이 많겠지만)

이번 노션 코드리뷰도 그렇고, 강의를 들으며 가독성 관련해서 많이 배울 수 있었다.

- 삼향 연산자 사용
- 적절한 줄바꿈
- 함수는 적절하게 항상 분리하기
- 길어지는 조건문은 switch 사용

이 중 삼향 연산자는 정말 익숙치 않아서 ? 뒤에가 true인지 :뒤에가 true 인지 정말 헷갈렸다.

지금은 손에 익었다! 앞으로 적극적으로 사용하도록 하자.

<br><br>

---

# [2] 소감

개발 참 재미있다.

내가 작성한 코드가 화면의 어떠한 영역에 띄워지고, 동작하는 순간 순간이 짜릿하다.

대규모의 프로젝트를 진행하게 되면 (물론 걱정부터 하겠지만) 정말 재밌게 참여할 수 있을 것 같다.

<br>

사실 요즘 조금 슬럼프였다.

취업은 할 수 있을까 나는 개발자로써 성장할 수 있을까 등등의 수많은 고민을 했다.

앞으로 해보고 싶은 프로젝트를 생각해 보기도 하고, 내가 진짜 어떤 길을 걷고 싶은지 되돌아보기도 했다.

~~그래서 포스팅도 잘 못하긴 했어요 하하~~

![Untitled 2](https://user-images.githubusercontent.com/72294509/165543993-49078955-1892-4740-a828-7a0eb3624bd2.png)

정말 고민이 많았지만, 새로 배우는 재미에 버텼던 것 같다. 🤗

내가 해보고 싶은 프로젝트에서 배운 것들을 적극적으로 활용할 수 있도록 계속 복습해야겠다.

<br>

앞으로 뷰와 리엑트 과정이 남았는데, **사실 리엑트가 조금 두렵다. 😓**

2020년 겨울에 리엑트로 프로젝트를 진행하다가 너무 어려워서 VanillaJS로 급하게 변경한 이력이 있다.

<br>

그래도 나는 지금 성장했으니까.. 할 수 있..을 것 이라 생각하고 열심히 공부해 보려 한다.

<br>

이상 간단한 VanillaJS 세션 회고와 소감을 적어보았다.

<span style="background-color:#FFF2D1">모든 예비 개발자분들 화이팅 !</span>
