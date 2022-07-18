---
layout: single
title: "[Web] Next.js 외부 라이브러리 사용 오류"
categories: [Web]
last_modified_at: 2022-07-18
excerpt: "포트폴리오 개발 중 오류에 맞닥뜨렸다."
header:
  teaser: https://user-images.githubusercontent.com/72294509/179460080-85c58d8e-00b9-4d03-9f7f-2caab0509566.png
---

![블로그-썸네일-003](https://user-images.githubusercontent.com/72294509/179460080-85c58d8e-00b9-4d03-9f7f-2caab0509566.png){: .align-center}

<br>

이번에 혼자 꼭 해보고자 했던 포트폴리오 웹 사이트 개발 도중, 약 36시간동안 헤멘 문제에 대해 글을 작성하고자 한다.

> 물론 말끔하게 해결하지 못했다. 찜찜한 방법으로 해결했기에, 이 문제를 겪어봤거나.. 해결책을 아시는 분들은 댓글로 알려주면 3보 1배하면서 뵈러 가겠다.. 😥

<br><br>

`Next.js`를 사용해서 개발을 진행하다가, 외부 라이브러리를 사용하고 싶은 부분이 있었다. 바로 이 부분이었다.

![Untitled](https://user-images.githubusercontent.com/72294509/179460374-0140f75f-d61e-44e6-9819-cbe565db1619.png){: .align-center}

이번에 강의에서 다룬 데이터 시각화를 이용해서 직접 구현을 할까 하다가 **D3.js 기반의 좋은 라이브러리**가 있어 이용하고자 했다.

[https://reaviz.io/?path=/story/docs-intro--page](https://reaviz.io/?path=/story/docs-intro--page)

<br><br><br>

# 문제

해당 라이브러리는 최근까지도 계속 개발이 이어져오고 있기에, 아무 문제 없겠지 했지만 `Next.js` 이..이.. 이 좋은.. 좋은 친구는 몇가지 오류를 일으켰다. 😊.. (웃어라 지영)

```tsx
import { BubbleChart, Bubble, BubbleSeries, ChartTooltip } from "reaviz";
```

<br>

이 아주 간단하고 보편적인 모듈 import에서 오류가 쭉쭉 났다.

```
1. Next.js Reference Error: document is not defined

2. require() of ES Module` error in Next.js
```

![Untitled 1](https://user-images.githubusercontent.com/72294509/179460362-c4dd7760-7297-413f-af2b-b7f4e55516dd.png){: .align-center}

<br><br><br>

# 원인

스토리북에서는 잘 작동했는데 왜 Next.js에서만 그럴까 고민했는데 역시나 예상대로 Nextjs의 장점인 `Server Side rendering` 때문이라는 생각이 들었다. 한번 더 정리하고 넘어가자.

<br>

## + NextJS의 동작 방식

Next.js는 리엑트를 SSR 방식으로 구현할 수 있도록 도와주는 프레임워크이다. 정확히는 CSR와 SSR을 적절하게 섞어 동작한다.

뭐니뭐니 해도 SSR의 가장 큰 장점은 **접속하는 페이지의 View를 사용자에게 매우 빠른 속도로 렌더링**한다는 것이다. View를 빠르게 렌더링하고, 동작을 위한 리소스들을 그 후에 로드한다. 따라서 웹의 모든 View를 클라이언트로 로드한 뒤 렌더링을 하게 되는 CSR보다 훨씬 빠르다.

Next.js가 SSR과 CSR을 적절히 섞어 사용한다는 말은 다음 동작을 의미한다. **링크, 주소를 통한 접속은 SSR방식을 이용**한다. 다만 **이벤트를 통한 페이지 접속은 CSR 방식**으로 일어난다.

예를 들어 사용자가 처음 링크를 통해 특정 웹 페이지로 최초 접속하게 되면, 서버는 사용자에게 빠르게 웹 페이지를 보여주기 위해 SSR을 하게 된다.

해당 웹 페이지 내에서 ‘게시물 페이지’ 버튼을 클릭하여 게시물 페이지로 이동하게 된다면, 이때는 이벤트를 통한 페이지 전환이 빠르게 일어나기 위해 CSR을 하게 된다.

> 이러한 NextJS 동작 방식을 이해하고 나면, 왜 문제가 일어났는지 대충 파악할 수 있다.

<br><br><br>

# 해결 방법

## 1. Next.js Reference Error: document is not defined

2번 문제는, 위의 Next.js 동작 방식으로 이해해보자면 **웹 페이지를 구성시킬 요소들이 렌더링 및 클라이언트로 로드되기 전에 document로 접근해서 View 요소를 조작하려 하니 발생한 문제**이다.

링크로 접속하거나 새로고침을 하게 된다면, SSR이 동작하여 에러가 발생한다. **왜냐 다시 document가 선언되지 않았기 때문이다.**

document가 일시적으로 다 사라진 상황에서, 그때 내가 사용할 BubbleChart 컴포넌트가 어디에 그려져야 할지 위치를 찾지 못해 나오는 에러이다.

<br>

### 해결

[Next.js Document](https://nextjs.org/docs/advanced-features/dynamic-import)를 참고해서 해결했다.

**With no SSR** 부분을 보면 dynamic을 사용할 수 있다. 이 부분에서 `ssr`, `suspense` 옵션을 설정할 수 있다.

```tsx
import S from "./AboutMePage.style";
import {
  HeaderSection,
  EducationSection,
  AwardsSection,
  ExperienceSection,
} from "./components";

const DynamicTechStack = dynamic(() => import("./components/TechStack"), {
  ssr: false,
  suspense: true,
});
```

`TechStackSection`만 따로 빼서 `dynamic`을 사용해 import해왔다.

`ssr` 옵션은 `false`, `suspense`는 `true`로 설정했다.

<br>

`ssr` 옵션을 끔으로써, **Client Side에서 동적인 로딩**을 해올 수 있다. 따라서 링크로 접속했을 때, 페이지의 나머지 요소보다, 해당 Section만 조금 늦게 렌더링 된다.

<br>

`suspense` 옵션은 해당 Section Component가 아직 로드되지 않았을 때, `suspense`의 fallback Component를 미리 보여줄 수 있다. 이는 _`import_ { Suspense } _from_ 'react';`를 해온 뒤, 해당 Component를`Suspense`로 한번 더 감싸줘야 사용할 수 있다.

```tsx
<Suspense fallback={`Loading...`}>
  // fallback 부분에 Loading Text 말고 컴포넌트를 넣어줘도 된다.
  <DynamicTechStack />
</Suspense>
```

<br><br>

## 2. require() of ES Module` error in Next.js

위의 문제를 해결하기 위해 `dynamic`과 `suspend`를 사용했는데, 이 문제는 해결되지 않았다. 이 부분은 `NextJS + typescript + CommonJS, ESM 모듈 import, require`의 문제인 것 같았다.

[카카오 스타일 기술 블로그](https://devblog.kakaostyle.com/ko/2022-04-09-1-esm-problem/)를 참고해서 이해를 할 수 있었다.

백엔드 딴에서 발생한 문제지만, 이 글을 읽으며 아 대충 모듈 내부에서 왜 문제가 일어나는지 짐작할 수 있었다.

<br>

### 해결

먼저 공식 document를 참고해서 여러 방법을 시도해봤다.

Next.js는 외부 ES Modules 라이브러리를 사용할 때 오류가 발생한다고 한다. 따라서 Next.js Version 을 11.1로 설정한 다음 next.config.js에 해당 내용을 작성해주면 오류가 사라진다.

```jsx
// next.config.js
experimental: {
  esmExternals: true;
}
```

> 하지만 왜 [12 버전](https://nextjs.org/blog/next-12)에서는 안되는 것이죠? 이전 버전과 호환도 지원한다는데 아직 완벽하게 이해가 되지 않는다.. 😨🤔

<br>

결국은 다운그레이드를 하지 않고, 약간은 우스운 방법으로 해결했다.

사용할 라이브러리의 dist 디렉토리 내부를 살펴보니, 두 개의 파일이 모두 있었다.

![Untitled 2](https://user-images.githubusercontent.com/72294509/179460366-0b2a2b7c-8107-4e31-b035-78a08340d03a.png){: .align-center}

`commonJS`, `ESM` 파일이 둘 다 존재하는데, 계속해서 `index.cjs.js`로 접근해 require을 해오니 오류가 났다. `index.esm.js`로 접근하면 참 좋을텐데..

<br>

그래서 `index.cjs.js` 파일을 삭제했다

![Untitled 3](https://user-images.githubusercontent.com/72294509/179460369-6a84ca82-9c1a-475e-b92a-fd705b8a8392.png){: .align-center}

…?

삭제하니까 너무 잘된다.. 자동으로 `index.esm.js` 파일로 접근해서 라이브러리도 잘 실행된다..

`npm run dev`도 `npm run build`도 너무 잘 된다.

<br><br>

# 반성

![Untitled 4](https://user-images.githubusercontent.com/72294509/179460372-5183010c-103a-4d67-a6f0-51c28a9e860f.png){: .align-center}

> 결론적으로 두 가지 오류 모두 해결 완료!

<br>

고민한 시간에 비해 너무 간단한 방법으로 두가지 오류를 해결할 수 있었다. 모듈 파일 자체를 건드리는 것은 좋지 않은 방법이며 올바른 해결 방법이 아니다. 설정 파일 쪽을 조금 더 건드려보고 싶었지만, 이미 `package.json, tsconfig.json, next.config.js`에서 아는 지식으로 할 수 있는 방법은 모두 적용해본 듯 하다.

더 이상 개발을 늦출 수 없어 일단은 진행해보려 한다. 🤔 그래도 이 기회에 `CommonJS`와 `ESM`, `NextJS`의 동작 방식을 한번 더 짚고 넘어갈 수 있어서 어떻게 보면 되게 좋은 시간을 보낸 것 같다.

그리고 뭐니뭐니 해도 공식 문서가 짱이다.. 만든 사람이 작성한 문서가 가장 정확하다.. 👍🏻 그리고 시간을 투자하면 투자한 만큼의 댓가를 얻을 수 있다. 어찌 저찌 문제를 해결하지 않았는가. 앞으로도 포기하지 말고 계속해서 탐구해나가는 자세를 가지도록 하자.

<br><br><br>

---

### 출처

[NextJS 공식 Document](https://nextjs.org/docs/advanced-features/dynamic-import#with-no-ssr)

[reaviz](https://reaviz.io/?path=/story/docs-intro--page)

[Kakaostyle 기술 블로그-esm-problem](https://devblog.kakaostyle.com/ko/2022-04-09-1-esm-problem/)

[egoavara Velog_typescript-npm패키지를-dual-package로-만들기](https://velog.io/@egoavara/typescript-npm%ED%8C%A8%ED%82%A4%EC%A7%80%EB%A5%BC-dual-package%EB%A1%9C-%EB%A7%8C%EB%93%A4%EA%B8%B0)

[sezzled Blog_Nextjs-Reference-Error-document-is-not-defined](https://sezzled.tistory.com/entry/%EC%97%90%EB%9F%AC%ED%95%B4%EA%B2%B0-Nextjs-Reference-Error-document-is-not-defined)

[yceffort Blog_nextjs-lesson-and-learn](https://yceffort.kr/2021/12/nextjs-lesson-and-learn)

[bytemeta_repo/reaviz/reaflow/issues/137](https://bytemeta.vip/repo/reaviz/reaflow/issues/137)
