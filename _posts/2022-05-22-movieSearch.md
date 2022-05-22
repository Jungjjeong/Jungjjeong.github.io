---
layout: single
title: "영화 검색 사이트 Project 회고"
categories: [devCourse, Project, Retrospective]
last_modified_at: 2022-05-22
excerpt: "0519-0521 영화 검색 사이트 Project 회고록"
header:
  teaser: https://user-images.githubusercontent.com/72294509/169700541-5a0439c1-98c3-4b36-bb5f-9e1b07b08dc4.png
---

![Untitled](https://user-images.githubusercontent.com/72294509/169700541-5a0439c1-98c3-4b36-bb5f-9e1b07b08dc4.png){: .align-center}

노션 클로닝 프로젝트 이후로 두번째 회고록입니다.

**KPT 회고 방법**을 사용해서 회고를 진행하려 합니다.

<br><br><br>

# [1] 영화 검색 사이트 프로젝트 회고

## [1-1] 프로젝트 소개

Vue.js와 영화 검색 API를 활용해 프로젝트를 진행합니다.

### ▶ 배포 링크

🔮 [**지영화검색 사이트**](https://musical-stroopwafel-c0b8ad.netlify.app/)

<br>

기본 요구사항은 4가지, 선택 요구사항은 6가지였습니다.

### ▶ 기본 요구사항

- 검색어를 입력해 영화를 검색할 수 있어야 합니다.
- 검색된 결과를 통해 영화의 상세 정보를 볼 수 있어야 합니다.
- 클라이언트에서 API Key가 노출되지 않아야 합니다.
- 실제 서비스로 배포하고 접근 가능한 링크를 추가해야 합니다.

<br>

### ▶ 선택 요구사항

- API 요청(Request)에 대한 로딩 애니메이션을 추가해 보세요.
- 영화 상세 검색으로 출력할 영화 포스터를 더 높은 해상도 사용해 보세요.
  - 영화 포스터 주소에 포함된 `SX300`를 `SX700`과 같이 더 큰 숫자로 수정해 요청하세요.
  - 실시간으로 이미지의 크기를 다르게 요청하는 것이 어떤 원리로 가능한지 조사해 보세요.
- 요청 주소에 HTTP가 아닌 HTTPS 프로토콜을 사용해야 하는 이유를 조사해 보세요.
- Bootstrap 등의 UI 프레임워크를 구성해 프로젝트를 최대한 예쁘게(?) 만들어 보세요.
- Open Graph 혹은 Twitter Cards로 Meta 정보를 제공해 보세요.
- NuxtJS를 활용해 Server Side Rendering(SSR)과 검색 엔진 최적화(SEO)를 도입해 보세요.<br>
  (만약 SSR에 익숙치 않다면, SPA 프로젝트를 먼저 만들어 보고 도전하세요!)

<br>

> 저는 **완벽한 기본 요구사항 구현 + 선택 요구사항 반 이상 구현**의 목표를 가지고 프로젝트를 시작했습니다.<br>Meta정보 제공, NuxtJS 활용을 제외한 모든 구현사항을 개발할 수 있었습니다.

<br>

### (1) 초기 페이지 디자인

먼저 사이트의 레이아웃, 디자인을 잡고 프로젝트를 시작했습니다.

멘토님이 말씀해주신 협업 과정을 간접적으로 경험해보고자

<span style="background-color:#FFF2D1">디자인 시안 + 색상 변수(\_variables.scss)를 먼저 정의</span>하고 개발을 시작했습니다.

![App_Component](https://user-images.githubusercontent.com/72294509/169700550-5af95078-a5f0-416b-ad38-1dbf5e94f651.png){: .align-center}

![Select](https://user-images.githubusercontent.com/72294509/169700760-1d1616fd-4abe-4e46-a3ea-62a781601abf.png){: .align-center}

![app__inner](https://user-images.githubusercontent.com/72294509/169700544-636f2ccb-d82a-4221-a7de-eb4fbfb9a3e5.png){: .align-center}

![Select-1](https://user-images.githubusercontent.com/72294509/169700762-5b81ee59-cae8-43d6-902a-a826f15f105e.png){: .align-center}

<br>

### (2) 초기 컴포넌트 구조

```
│── src
│   ├── components
│   │    │── Header.vue
│   │    │── Loading.vue
│   │    └── Movie.vue
│   ├── routes
│   │    │── index.js
│   │    │── Home.vue
│   │    │── MovieList.vue
│   │    │── MovieDetail.vue
│   │    └── NotFound.vue
│   ├── scss
│   │    └── _variables.scss
│   ├──  store
│   │    │── index.js
│   │    └── movie.js
│   ├── App.vue
│   ├── index.html
│   └── main.js
└── static
     └── favicon,svg
```

<br>

### (3) 최종 페이지 구성

- <span style="background-color:#FFF2D1">Home</span>

![https://user-images.githubusercontent.com/72294509/169679207-3d061714-cbed-4440-9a59-d46a019a368f.png](https://user-images.githubusercontent.com/72294509/169679207-3d061714-cbed-4440-9a59-d46a019a368f.png){: .align-center}

<br>

- <span style="background-color:#FFF2D1">MovieList</span>

▶ 검색된 영화가 없을 경우

![Untitled 1](https://user-images.githubusercontent.com/72294509/169700643-12480c17-5da6-4c01-8b98-977feb34ded9.png){: .align-center}

▶ 검색된 영화가 있을 경우

![Untitled 2](https://user-images.githubusercontent.com/72294509/169700524-2fd28a4c-32fd-42e3-b3e7-d0e83d90adc0.png){: .align-center}

![Untitled 3](https://user-images.githubusercontent.com/72294509/169700528-176d85a7-d865-4cea-9b8f-0b9e5ab95952.png){: .align-center}

<br>

- <span style="background-color:#FFF2D1">MovieDetail</span>

![Untitled 4](https://user-images.githubusercontent.com/72294509/169700530-82b4c340-fd26-4114-af38-c13aeee35f7d.png){: .align-center}

<br>

### [4] 최종 컴포넌트 구조

```
│── src
│   ├── components
│   │    │── Header.vue
│   │    │── Loading.vue
│   │    └── Movie.vue
│   ├── routes
│   │    │── index.js
│   │    │── Home.vue
│   │    │── MovieList.vue
│   │    │── MovieDetail.vue
│   │    └── NotFound.vue
│   ├── scss
│   │    │── _mixins.scss      ---> _mixins.scss만 추가되었습니다.
│   │    └── _variables.scss
│   ├──  store
│   │    │── index.js
│   │    └── movie.js
│   ├── App.vue
│   ├── index.html
│   └── main.js
└── static
     └── favicon,svg
```

> **초기 컴포넌트 구조, 초기 디자인과 거의 유사하게 최종 결과물을 제작**할 수 있었습니다.

<br><br>

## [1-2] KPT

### [1] KEEP

- <span style="background-color:#FFF2D1">Vuex의 중앙 집중형 저장소가 정말 편리했습니다.</span>

VanillaJS에서는 상상도 할 수 없던 반응형 데이터, 양방향 데이터 바인딩 등등

`Vue` 프래임워크가 가진 강력한 기능들과 더불어 `Vuex` 라이브러리의 저장소 기능까지

자동으로 바뀌는 데이터들과 페이지 렌더링을 보고만 있어도 배가 불렀습니다. 😊

<br>

- <span style="background-color:#FFF2D1">컴포넌트의 데이터 흐름</span>

단방향으로 흘러야 할 **데이터가 양방향으로 흐르는 반응형 웹의 특징**에 적응하기 어려웠습니다.

상위 컴포넌트에서 하위 컴포넌트로 순차적으로 데이터를 전달해주는 방식과 달리

중앙 집중형 데이터 저장소에서 데이터를 변경하고 이를 각각 다른 컴포넌트에 전달해주는 방식이 신기했습니다.

하지만 개발을 진행하며 차츰 익숙해질 수 있었습니다. 더.. 노력해 봐야죠.

<br>

- <span style="background-color:#FFF2D1">디자인</span>

디자이너와 원활한 협업이 가능한 프론트엔드 개발자가 되기 위해 `Figma`와 `SCSS`를 적극적으로 활용해보는 연습을 하고 있습니다.

디자인 시안과 변수 목록들을 받아 프론트를 제작하는 현업의 흐름과 비슷하게

`Figma`로 디자인을 먼저 한 뒤 `_variable.scss` 파일에 해당 색상 변수들을 먼저 선언하고 개발을 시작했습니다.

좋은 연습이 되었다고 생각합니다.

<br><br>

### [2] PROBLEM

- <span style="background-color:#FFF2D1">라우팅 처리</span>

개발을 진행하며 가장 애를 먹었던 문제입니다.

Home route 안에 Children으로 다른 route를 정의했습니다만,

원하지 않는 `RouterView`에도 해당 라우터가 재귀적으로 배치되는 것을 확인할 수 있었습니다.

![Untitled 5](https://user-images.githubusercontent.com/72294509/169700535-a0b74aa7-3872-472c-bc6a-08ca4e57bb81.png){: .align-center}

<br>

> MovieList 내부에 선언한 RouterView에도
> MovieList Component가 **재귀적으로 배치되는 문제**가 있었습니다.

<br>

- <span style="background-color:#FFF2D1">스크롤</span>

MovieList의 경우 **경로가 바뀌어도** **같은 컴포넌트(MovieList.vue)가 랜더링** 되기 때문에

**스크롤의 위치가 고정**되는 문제가 있었습니다.

<br>

예를 들어, 스크롤을 쭉 내려 page가 10이 되었을 때,

다른 검색어를 입력하고 검색 버튼을 눌러 새 영화 목록을 렌더링해야 하면

한번에 **page 1부터 page 10까지 API 호출**이 일어나 **영화 목록이 해당 스크롤 위치까지 불러와지는 이슈가 발생**했습니다.

이는 **page를 초기화**해줘도 동일하게 발생했습니다.

(스크롤 위치로 API를 호출하기 때문에 일어나는 문제였습니다.)

<br>

- <span style="background-color:#FFF2D1">배포 문제</span>

속도가 더 빠른 `Vercel`을 주로 사용하던 저는 이번에도 `Vercel`을 사용해서 프론트를 배포하려 했으나,

환경 변수 설정에 실패해 강의 내용대로 `Netlify`를 사용해서 배포를 진행했습니다.

<br>

`Netlify`를 사용하는데 있어서도 어려움을 겪었습니다.

먼저 ES6 방식인 `import, export`에 익숙해져 있는 저에게 CommonJS 키워드인 `require`는 너무 생소했고,

계속 import로 작성하는 저로 인해 **Syntax Error**가 발생했습니다.

~~(사소한 문제지만 이 문제 때문에 3시간 정도를 날렸습니다..)~~

```jsx
const axios = require("axios");
```

<br>

- <span style="background-color:#FFF2D1">Gird 세로 크기 문제</span>

영화의 포스터 크기가 제각각이라 납작하게 찌그러지는 Grid layout을 볼 수 있었습니다.

<br><br>

### [3] TRY

> <span style="background-color:#FFF2D1">[1] 라우팅 처리</span>

`RouterView`의 이름을 지정해 줌으로써 해결했습니다.

```jsx
{
  name: "MovieDetail",
  path: "/detail/:id",
  components: {
    modal: MovieDetail, // modal이라는 RouterView에 MovieDetail Component를 배치합니다.
  },
},
```

이는 주로 **한 페이지에서 여러개의 RouterView를 배치할 때 사용하는 방법**이지만,

저는 **재귀 배치되는 RouterView 문제를 해결하기 위해서 사용**했습니다.

<br>

> <span style="background-color:#FFF2D1">[2] 스크롤</span>

router 내부 meta 태그를 사용할 수 있었습니다.

경로 이동이 일어날 때(즉 다른 컴포넌트가 랜더링 될 때)는 `scrollBehavior` 옵션을 사용하여 돌아갈 스크롤의 위치를 명시해줄 수 있습니다.

```jsx
export default createRouter({
  history: createWebHistory(),
  scrollBehavior() {
    return { top: 0 };
  },

// ...
```

하지만 제가 겪은 문제는

같은 컴포넌트 내부에서 경로의 변경이 있을 때, **스크롤의 위치를 초기화**시켜줘야 해결할 수 있었습니다.

이는 **meta 태그로 해결**할 수 있습니다.

```jsx
name: "MovieList",
path: "/search/:searchTitle",
component: MovieList,
meta: {
  scrollToTop: true, // searchTitle이 바뀌어 들어가면 맨 위로 스크롤 위치가 초기화됩니다.
},
```

<br>

> <span style="background-color:#FFF2D1">[3] 배포 문제</span>

이는 **require 방식으로 코드를 바꾸면서 자연스럽게 해결**되었습니다..

package.json에 `type: module`도 추가해보고, 콘솔 로그도 20개 정도 찍어본 것 같은데

이렇게 쉽게 해결될 줄 몰랐습니다….

<br>

> <span style="background-color:#FFF2D1">[4] Gird 세로 크기 문제</span>

명시적으로 길이를 지정하면 끝날 문제였지만, 저는 default 이미지를 지정해 그리드의 최소 크기를 정의할 수 있었습니다.

![Untitled 6](https://user-images.githubusercontent.com/72294509/169700538-dc2467e0-6a23-4436-86d9-183ae2067154.png){: .align-center}

```html
<img
  :src="
    movie.Poster !== 'N/A'
      ? movie.Poster
      : 'https://user-images.githubusercontent.com/72294509/169549901-0c68bcba-d45b-4a94-97d8-f014b2133991.png'
  "
/>
```

<br><br>

## [1-3] 소감

거대한 프래임워크인 Vue를 사용해서 웹을 개발하는 것은 처음이었습니다.

토이 프로젝트에서는 Vue-cli server를 사용하여 이미 완성된 Bootstrap 디자인 프로젝트를 가져와 사용했었습니다.

반응형 웹의 개발 흐름을 조금이나마 배울 수 있었던 경험이었습니다.

<br>

또한 VanillaJS는 정말 기본이라는 것을 알 수 있었습니다.

Vue를 처음 사용해봤지만, 생각했던 것보다 짧은 시간 안에 구현 사항을 개발할 수 있었습니다.

기초가 탄탄하면 뭐든 도전할 수 있다고 생각합니다.

~~아직 공부할 부분은 상당히 많습니다.~~

<br>

이번 프로젝트를 진행하며 사용자의 입장에서 웹의 사용성을 극대화 시키는 고민을 많이 했습니다.

어떻게 하면 사용자가 input 창을 클릭하지 않고도 자동으로 focus가 잡히는지,

현재 어디를 클릭하고 있는지 명확하게 알 수 있을까, 가독성을 어떻게 하면 좋게 할까 등등

이전의 Notion 프로젝트보다 한 단계 성장한 제 모습을 확인할 수 있었습니다.

<br>

하지만 아직 프로젝트를 완벽하게 개발하지 못했습니다.

특히 라우터 부분에서 구현이 미흡하다고 생각합니다. ~~새로 고침 시, 이전의 검색 데이터가 사라지는 이슈가 있습니다. 하하~~

<br>

지금까지의 성장 경험을 Notion 클로닝 프로젝트에 녹여 1차 리팩토링을 진행한 뒤

Vue 프로젝트도 점진적으로 발전시켜 나갈 계획입니다.

회고록을 작성하며 생각했던 미흡한 부분들, 발전시킬 사항들을 Vue 프로젝트에 녹여보도록 하겠습니다.

<br>

앗 리팩토링을 진행하며 플랫폼의 버전도 관리해볼 수 있지 않을까요? 😎

<br><br><br>

---

### 출처

[프로그래머스](https://programmers.co.kr/)
