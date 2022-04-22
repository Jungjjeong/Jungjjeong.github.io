---
layout: single
title: "Notion cloning Project 회고"
categories: [devCourse, Project, Retrospective]
last_modified_at: 2022-04-22
excerpt: "0412-0420 Notion cloning Project 회고록"
header:
  teaser: https://user-images.githubusercontent.com/72294509/164605165-a1eb61a7-998c-42b0-9361-258dbc37127e.png
---

![Untitled](https://user-images.githubusercontent.com/72294509/164605165-a1eb61a7-998c-42b0-9361-258dbc37127e.png)

<br><br>

KPT 회고 방법으로 노션 클로닝 프로젝트 회고를 진행하려 합니다.

여기서 잠깐 <span style="background-color:#fff5b1;">KPT 회고 방법</span>이 무엇일까요?

<br><br>

# [+] KPT 회고 방법

KPT는 Keep Problem Try 세 단계로 나누어 각 단계에 따른 프로젝트 회고를 하는 것 입니다.

- **Keep** : 프로젝트 완료 후에도 간직하고 싶던 좋았던 것/ 잘했던 것
- **Problem** : 프로젝트 중 겪었던 어려움, 아쉬움으로 남는 것
- **Try**: Problem 중 해결된 사항에 대한 해결 방법/ 해결되지 않은 사항에 대한 피드백

보통 KPT 회고 방법은 4인 이상의 팀원으로 진행중인 프로젝트에서 스프린트 마지막날 또는 프로젝트 중간 중간에 해당 방법을 사용하며 협업 과정을 이끌어내는 용도로 많이 사용합니다.

하지만 저는 개인 회고에 사용해 보도록 하겠습니다. 😈

<br><br><br>

# [1] 노션 클로닝 프로젝트 회고

## [1-1] 프로젝트 소개

**바닐라 JS만을 이용해 노션을 클로닝**합니다.

크게 기본 요구사항은 4가지, 보너스 요구사항은 2가지였으며, 노션의 주요 기능과 유사했습니다.

<br>

### + 기본 요구사항

- 글 단위를 Document라고 합니다. Document는 Document 여러개를 포함할 수 있습니다.
- 화면 좌측에 Root Documents를 불러오는 API를 통해 루트 Documents를 렌더링합니다.
  - Root Document를 클릭하면 오른쪽 편집기 영역에 해당 Document의 Content를 렌더링합니다.
  - 해당 Root Document에 하위 Document가 있는 경우, 해당 Document 아래에 트리 형태로 렌더링 합니다.
  - Document Tree에서 각 Document 우측에는 + 버튼이 있습니다. 해당 버튼을 클릭하면, 클릭한 Document의 하위 Document로 새 Document를 생성하고 편집화면으로 넘깁니다.
- 편집기에는 기본적으로 저장 버튼이 없습니다. Document Save API를 이용해 지속적으로 서버에 저장되도록 합니다.
- History API를 이용해 SPA 형태로 만듭니다.
  - 루트 URL 접속 시엔 별다른 편집기 선택이 안 된 상태입니다.
  - /documents/{documentId} 로 접속시, 해당 Document 의 content를 불러와 편집기에 로딩합니다.

<br>

### + **보너스 요구사항**

- 기본적으로 편집기는 textarea 기반으로 단순한 텍스트 편집기로 시작하되, 여력이 되면 div와 contentEditable을 조합해서 좀 더 Rich한 에디터를 만들어봅니다.
- 편집기 최하단에는 현재 편집 중인 Document의 하위 Document 링크를 렌더링하도록 추가합니다.
- 편집기 내에서 다른 Document name을 적은 경우, 자동으로 해당 Document의 편집 페이지로 이동하는 링크를 거는 기능을 추가합니다.
- 그외 개선하거나 구현했으면 좋겠다는 부분이 있으면 적극적으로 구현해봅니다!

<br>

저는 <span style="background-color:#fff5b1;">완벽한 기본 요구사항 구현 + 노션과 유사한 UI 라는 목표</span>를 가지고 프로젝트를 시작했습니다.

<br><br>

### **(1) 초기 페이지 구성**

<img width="1728" alt="Untitled 1" src="https://user-images.githubusercontent.com/72294509/164605166-e184018c-b69e-410e-8a9c-d4ff5e89e300.png">

<br>

### **(2) 초기 컴포넌트 구조**

```
│── src
│   ├── components
│   │     │── PostsPage.js  // 리스트페이지
│   │     │── PostList.js   // 리스트
│   │     │── PostsEditPage.js  // 편집페이지
│   │     │── Editor.js     // 편집창
│   │     └── LinkButton.js // 이동 버튼
│   ├── utils
│   │     │── api.js
│   │     │── router.js
│   │     └── storage.js
│   ├── style.css
│   ├── App.js
│   └── main.js
└── index.html
```

<br><br>

### **[3] 최종 페이지 구성**

![Untitled 2](https://user-images.githubusercontent.com/72294509/164605167-9135997e-fe84-4278-908a-13ad4079160c.png)

<br>

### **[4] 최종 컴포넌트 구조**

```
│── src
│   ├── components
│   │     │── PostsPage.js   // 리스트 페이지
│   │     │── PostList.js    // 리스트
│   │     │── Header.js      // 헤더
│   │     │── PostsEditPage.js // 편집 페이지
│   │     │── Editor.js      // 편집창
│   │     │── Preview.js     // MD Preview
│   │     └── ToggleButton.js // 토글 버튼
│   ├── storage
│   │     └── constants.js
│   ├── utils
│   │     │── api.js
│   │     │── fetch.js
│   │     │── router.js
│   │     └── storage.js
│   ├── style.css
│   ├── App.js
│   └── main.js
└── index.html
```

![Untitled 3](https://user-images.githubusercontent.com/72294509/164605170-a0abcc3c-e640-4b9d-8071-abc58bfb45e7.png)

> **PostEditPage**에서 각 Post의 데이터를,<br>**PostsPage**에서 Post 전체 list의 데이터를 관리하고, 각각 하위 컴포넌트로 보내줍니다.

<br><br>

## [1-2] KPT

### [1] KEEP

- <span style="background-color:#fff5b1;">Component의 기본 구조와 데이터 흐름을 파악하는 법을 알 수 있었습니다.</span>

강의에서 그대로 따라치던 컴포넌트의 기본 구조가 손에 익었으며,

어떤 용도로 컴포넌트 내 함수를 생성하고 사용해야 하는지 배울 수 있었습니다.

<br>

**데이터가 필요한 곳이면 어디든 불러올 수 있게 짰던 과거의 저의 웹**들과는 달리

최대한 **서버와의 통신을 줄이고, 각 컴포넌트 간의 의존성을 낮추기 위해 데이터 흐름을 제어하는 법**을 알 수 있었습니다.

<br>

물론 완벽하게 알지는 못합니다. 하지만 **많이 배웠음에 중점을 두고 회고를 작성**합니다.

<br>

- <span style="background-color:#fff5b1;">코드를 짜는 순서를 알 수 있었습니다.</span>

**계획의 수립과 실천의 과정이 전혀 없었던 과거의 저의 모습**이 생각납니다.

물론 초기 계획과는 컴포넌트 구조 자체가 많이 달라졌지만, 큰 뼈대는 같았습니다.

계획을 세우고, 차례대로 코딩을 진행하다 보니 중간에 문제가 생기거나 추가 기능을 개발할 때에도 **수월하게 방향 전환**을 할 수 있었습니다.

<br>

- <span style="background-color:#fff5b1;">state 관리에 대해 이해할 수 있었습니다.</span>

각 컴포넌트가 가지는 **state 즉 데이터를 관리**함에 익숙해질 수 있었습니다.

state를 초기화해주고, 업데이트해주고 화면에 그리는 과정을 파악했습니다.

<br>

- <span style="background-color:#fff5b1;">개인적으로 CSS가 너무 재미있었습니다.</span>

노션에서 개발자 도구를 눌러 여러 색상을 따오고, Desktop UI에서의 리스트 크기, 버튼의 위치 등을 훔쳐왔습니다.. 하하

흰색과 검은색만 존재했던 저의 웹이 노션과 비슷해지고, 생명을 되찾는 과정이 너무 재미있었습니다.

<br>

특히 헷갈렸던 **Flex와 Grid에 대해서 어느 정도 익숙해졌다고 생각**합니다.

SCSS 문법을 적용해보고 싶었지만, 시간이 부족했습니다.

<br><br>

### [2] PROBLEM

- <span style="background-color:#fff5b1;">PostList의 같은 문서를 두번 클릭하면, Editor에 state가 업데이트 되지 않는 문제</span>

구현한 PostEditPage는 아직도 구조가 복잡하지만, 그 이전에는 더 복잡했습니다.

> 새로운 문서 생성 → { ..., id : new } → **title이나 content가 입력 되어야지만 서버에 POST, PUT**

해당 방식을 채택하고 있었기에, **id가 new일때와 아닐때를 구분**해서 setState를 진행했습니다.

따라서 같은 id를 가진 문서를 두번 클릭하면 id === post.id가 되어 setState가 발생하지 않는 문제가 있었습니다.

<br>

- <span style="background-color:#fff5b1;">onAdd와 onEdit 함수 중복 발생</span>

새로운 문서를 추가하는 onAdd와 기존의 문서를 수정하는 onEdit 함수가 중복으로 발생하는 문제가 있었습니다.

<br>

- <span style="background-color:#fff5b1;">Document 생성 시, 바로 list에 렌더링 되지 않는 문제</span>

1번 문제와 연관되는 문제입니다.

Document를 생성하면 **기본적으로 id가 new**이며, **아이디가 바로 할당되지 않는 setState 구조**였기 때문에 list에 렌더링 되지 않았습니다.

<br>

- <span style="background-color:#fff5b1;">Rich한 ContentEditor의 focus 및 <div> 문제</span>

MD를 적용해서 바로 변환해주는 Editor를 구현하려 했으나, focus가 초기화되는 문제를 겪었습니다.

또한 <div>로 감싸져 내용이 저장되어 2번의 줄바꿈 + MD 변환의 다양한 이슈가 발생했습니다.

<br><br>

### [3] TRY

- <span style="background-color:#fff5b1;">PROBLEM 01, 03</span>

> 1. PostList의 같은 문서를 두번 클릭하면, Editor에 state가 업데이트 되지 않는 문제
> 2. Document 생성 시, 바로 list에 렌더링 되지 않는 문제

먼저 새 문서 추가 버튼을 누르면 해당 문서의 내용으로 POST를 먼저 진행해 id를 할당받는 구조로 변경했습니다.

```jsx
// PostsPage의 postlist onCreateSubPost func
onCreateSubPost: async parentId => {
  const post = {
    title: 'Untitled',
    parent: parentId,
  };
  const newPost = await fetchNewPost(post);
  push(`/documents/${newPost.id}`);
},
```

새로운 문서를 초기화해주고 할당받은 id로 바로 라우팅을 해줍니다.

따라서 Document 생성 시 바로 list와 Editor의 setState가 이루어져 즉시 렌더링이 되었습니다.

<br>

또한 PostEditPage의 setState 구조를 전부 바꿨습니다.

```jsx
// PostEditPage setState func
this.setState = async (nextState) => {
  this.state = nextState;
  POST_LOCAL_SAVE_KEY = `temp-post-${this.state.id}`;

  await fetchUpdatePost(this.state);
  const post = await fetchLocalStorage();

  await listRendering();
  editor.setState(post);
  preview.setState(post);

  await this.render();
};
```

id가 미리 할당되어 PostEditPage에 전달되기 때문에, new id와 비교하는 과정을 전부 없앴습니다.

<br>

- <span style="background-color:#fff5b1;">PROBLEM 02</span>

> 2.  onAdd와 onEdit 함수 중복 발생

먼저 기존의 함수 형태를 보면 이러했습니다.

```jsx
// Postlist renderDocuments func
const renderDocuments = ({ title, id, documents }, arr) => {
    arr.push(
      `<li data-id=${id}>${title}<button class="node-post-button">+</button></li>`,
      `<li data-id=${id}>${title}<button class="node-post-button">+</button></li>`,
    );

...

// Event
$list.addEventListener('click', e => {
  const $li = e.target.closest('li');
  if ($li) {
    const { id } = $li.dataset;

    if (e.target.className === 'node-post-button') {
      onAdd(id);
    } else {
      onEdit(id);
    }
   };
};
```

onEdit을 발생시키길 원했던 li와 node-post-button의 onAdd 이벤트가 중복 발생함을 할 수 있었습니다.

이유는 li는 상위 요소이기 때문에 이벤트가 타고 올라가기 때문이었습니다.

<br>

따라서 아주 간단하게 title을 span 태그로 한번 더 감싸줌으로써 해당 이슈를 해결할 수 있었습니다.

```jsx
// Postlist renderDocuments func
const renderDocuments = ({ title, id, documents }, arr) => {
    arr.push(
      `<li data-id=${id}>${title}<button class="node-post-button">+</button></li>`,
      `<li data-id=${id}><span class="post-title">${title}</span><button class="node-post-button">+</button></li>`,
    );

```

> 다음부터는 e.preventdefault()를 사용하는 습관을 가지도록..

<br>

- <span style="background-color:#fff5b1;">PROBLEM 04</span>

> Rich한 ContentEditor의 focus 및 <div> 문제

미해결.. 문제입니다... 😂

1. focus가 정말 제멋대로 움직입니다.
   → keydown event를 사용해서 focus를 글자 끝으로 조정해보려 했으나 실패했습니다.
2. <div>로 감싸져 내용이 저장됩니다.
   → h1, h2, h3 등의 마크다운 문법을 <div>와 함께 변환해주려 했으나
   <div>가 두번이 생기고,, 없어지고,, 줄바꿈이 생기고 하는 다양한 이슈 때문에 실패했습니다.

<br>

따라서 저는 **Preview Component**를 따로 구현했습니다.

![Untitled 4](https://user-images.githubusercontent.com/72294509/164605171-9a22d21c-3db9-4183-ba98-86d32039829c.png)

이렇게 노션보다는 velog 형태로 Preview를 구현했고, 추후에 MD Editor를 따로 구현해야겠다는 목표를 세웠습니다.

참고로 Preview에서는 해당 함수를 통해 MD 문법을 html 문법으로 변경합니다.

<br>

이곳에 사실 아직 이슈가 있습니다. 😲

코드는 한 줄에 한 구역만 작성 가능하다는 이슈가 있는데, 리팩토링 때 해결할 생각입니다.

```jsx
const renderRichContent = (content) => {
  if (!content) return content;

  const richContent = content
    .split("\n")
    .map((line) => {
      if (line.indexOf("# ") === 0) {
        return `<h1>${line.substr(2)}</h1>`;
      } else if (line.indexOf("## ") === 0) {
        return `<h2>${line.substr(3)}</h2>`;
      } else if (line.indexOf("### ") === 0) {
        return `<h3>${line.substr(4)}</h3>`;
      } else if (line.indexOf("> ") === 0) {
        return `<blockquote>${line.substr(2)}</blockquote>`;
      } else if (line.indexOf("`") >= 0) {
        const codeIndex = line.indexOf("`");
        return (
          line.substring(0, codeIndex) +
          `<code>${line.substring(codeIndex + 1)}`.replace("`", "</code>")
        );
      } else if (line === "---") {
        return `<hr>`;
      }

      return `${line}`;
    })
    .join("<br>");

  return richContent;
};
```

<br><br><br>

## [1-3] 소감

노션 클로닝 프로젝트를 하면서 VanillaJS의 중요성을 다시금 깨달았습니다.

맛만 봤던 Vue와 React의 기본 구조와 Component의 state 관리가 유사하다는 생각이 들었어요.

<br>

또한 사용자의 입장에서 UX를 개선하는 것이 프론트엔드 개발자의 정말 중요한 역량이라는 것을 느낄 수 있었습니다.

focus가 하나라도 어긋나거나, 내가 지금 어디를 누르고 있는 건지 명확히 알 수 없거나, 글자의 자간과 행간이 너무 작거나 커서 가독성이 좋지 않거나..

이런 부분들도 항상 신경 쓰며 개발해야 하는 개발자가 바로 프론트엔드 개발자라는 것을 다시 한번 알 수 있었습니다.

<br>

회고를 하는 이유는, 나의 성장이라고 생각합니다.

회고를 하면서 개발 과정에 대해 다시 생각할 수 있었고, 내가 어떤 부분을 모르는지 정확히 짚을 수 있었습니다.

길은 조금 길어졌을지 몰라도, 다음에 진행하는 프로젝트에서는 조금 더 성장한 모습으로 개발하지 않을까 싶습니다.

![Untitled 5](https://user-images.githubusercontent.com/72294509/164605161-0b637e5a-e043-4c11-9aae-299ed50e340f.png){: .align-center}

자.. 이제 리팩토링 하러 가볼까요..? 🥺

<br><br><br>

---

### 출처

[https://velog.io/@cks3066/프로젝트-회고-회고-작성법](https://velog.io/@cks3066/%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8-%ED%9A%8C%EA%B3%A0-%ED%9A%8C%EA%B3%A0-%EC%9E%91%EC%84%B1%EB%B2%95)

[프로그래머스](https://programmers.co.kr/)
