---
layout: single
title: "[Web] Storybook 이란?"
categories: [Web]
last_modified_at: 2022-05-27
excerpt: "Storybook with React"
header:
  teaser: https://user-images.githubusercontent.com/72294509/170736014-c86f8d0f-2861-4765-ab38-fef3673cc55d.png
---

![Untitled](https://user-images.githubusercontent.com/72294509/170736014-c86f8d0f-2861-4765-ab38-fef3673cc55d.png){: .align-center}

<br><br><br>

# [1] Storybook

`storybook`은 **UI 컴포넌트를 모아서 보여주고 문서화하는 오픈소스 툴이다.**

즉 `React, Angular, Vue` 등의 **분리된 UI 컴포넌트를 체계적이고 효율적으로 구축할 수 있는 개발 도구**이다.

<br><br>

## [1-1] Storybook 설치

> 리엑트 기준으로 설명합니다. 🤗

<br>

나는 새로 생성한 `react-create-app` 프로젝트 내부에서 해당 명령어로 `storybook`을 설치했다.

루트 디렉토리에 설치해도 상관 없다고 한다.

```bash
npx -p @storybook/cli sb init
```

<br>

설치해보면 두 개의 디렉터리가 생성된다.

- .**storybook** : Storybook 설정 파일 포함
- **src/stories**: Storybook 예제 컴포넌트들

<br><br>

설치가 완료된 후, 해당 명령어로 `storybook` **서버를 가동**할 수 있다.

해당 서버는 포트번포 6006으로 열린다.

```bash
npm run storybook
```

![Untitled 1](https://user-images.githubusercontent.com/72294509/170736017-4852dad3-3e47-4969-a41b-c2babbc32dab.png){: .align-center}

![Untitled 2](https://user-images.githubusercontent.com/72294509/170736021-c8704ce9-86bf-443b-8100-cdf5bf684ee2.png){: .align-center}

<br>

> 참고로 나는 **몇개의 실습을 진행해본 후 정리글을 작성**하기 때문에
> 초기 설정과는 다르게 몇개의 컴포넌트가 더 포함되어 있다. 😮

<br><br>

## [1-1] Story 작성

`Story`는 `Storybook`을 구성하는 기본 구성 단위이다.

컴포넌트는 기본적으로 하나 이상의 `Story`로 구성된다.

`<컴포넌트 이름>.stories.js` 파일 안에 `Story`를 작성할 수 있다.

<br>

주로 **컴포넌트 파일과 같은 디렉토리 안에 작성**한다.

![Untitled 3](https://user-images.githubusercontent.com/72294509/170736024-4fca9447-011e-4291-a93c-c7e0aaa3722d.png){: .align-center}

<br>

하지만 예제 컴포넌트들이 담긴 `src/stories` **디렉토리 내부에 작성하는 경우**도 있다.

이는 협업을 진행하는 팀마다 다를 것이라 예상한다. 🤔

![Untitled 4](https://user-images.githubusercontent.com/72294509/170736027-9b87b98e-7fe1-4862-949a-4e10ef0e3a27.png){: .align-center}

<br>

```jsx
// Box.stories.js
import React from "react";
import Box from ".";

export default {
  title: "Example/Box",
  component: Box,
  argTypes: {
    width: { control: "number" },
    height: { control: "number" },
    backgroundColor: { control: "color" },
  },
};

const Template = (args) => <Box {...args} />;

export const Default = Template.bind({});
```

![Untitled 5](https://user-images.githubusercontent.com/72294509/170735996-011243ce-cdf1-41c3-8594-23c6bd9231ed.png){: .align-center}

<br>

컴포넌트 스토리 포맷의 세부 내용은 다음과 같다.

<br>

| 프로퍼티           | 설명                                                                                                                                                                                                                                                         |
| :----------------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **title**          | Storybook 사이드바에 표시될 이름이다. <br>`title: "Example/Box"`로 작성된 경우, Example 그룹의 Box 스토리로 표시된다.                                                                                                                                        |
| **component**      | 컴포넌트                                                                                                                                                                                                                                                     |
| **args**           | 모든 스토리에 공통으로 전달될 props이다. <br>`args: { width: 100 }` 로 작성된 경우, 모든 스토리 공통으로 컴포넌트에게 width props를 전달한다.                                                                                                                |
| **argTypes**       | 각 스토리 args의 행동 방식을 설정할 수 있다. <br> `argTypes: { width: { control: ‘number’ } }` 일 경우, controls에서 입력한 숫자를 컴포넌트의 props로 전달하겠다는 의미를 가진다. <br>위의 예시에서는 width, height, backgroundColor의 행동 방식을 설정했다. |
| **decorators**     | 스토리를 래핑하는 추가 렌더링 기능을 작성한다.                                                                                                                                                                                                               |
| **parameters**     | 스토리에 대한 정적 메타 데이터를 정의한다.                                                                                                                                                                                                                   |
| **excludeStories** | 렌더링 제외 설정                                                                                                                                                                                                                                             |

<br>

[storybook 공식 document](https://storybook.js.org/docs/react/writing-stories/introduction)를 참고하면 더 많은 스토리 작성 방식을 알 수 있다.

<br><br>

## [1-2] 간단한 Input Component

emotion의 styled를 사용해서 간단하게 Input 컴포넌트를 작성하고, 해당 Story를 작성해보자.

```
src
├── components
│   └── Input
│       ├── index.js       # => Component
│       └── Input.stories.js # => Story
...
```

```jsx
// Input/index.js
import styled from "@emotion/styled";

const Input = styled.input`
  display: block;
  padding: 4px 6px;
  width: 100%;
  height: 28px;
  font-size: 14px;
  border-radius: 4px;
  border: 2px solid #333;
  background-color: white;
  box-sizing: border-box;
`;

export default Input;
```

```jsx
// Input/Input.stories.js
import Input from ".";

export default {
  title: "Component/Input",
  component: Input,
  argTypes: {
    onChange: { action: "onChange" },
  },
};

const StoryTemplate = (args) => <Input {...args} />;

export const Default = StoryTemplate.bind({});

export const IdInput = StoryTemplate.bind({});
IdInput.args = {
  id: "userId",
  placeholder: "이메일 주소를 입력하세요. ",
};

export const PWInput = StoryTemplate.bind({});
PWInput.args = {
  id: "password",
  type: "password",
  placeholder: "비밀번호를 입력하세요.",
};
```

![Untitled 6](https://user-images.githubusercontent.com/72294509/170736003-ef75e48d-7f54-4317-bad0-fc17129409bd.png){: .align-center}

![Untitled 7](https://user-images.githubusercontent.com/72294509/170736004-ebd7f98a-7359-4055-841f-d1441c451f92.png){: .align-center}

![Untitled 8](https://user-images.githubusercontent.com/72294509/170736009-f4328798-747f-40d7-9232-82071033a8c6.png){: .align-center}

<br>

다음과 같이 `Storybook`에 스토리가 추가된 것을 확인할 수 있다.

<br><br>

## [1-3] 정리

결국 `Storybook`은 **프로젝트에서 개발하는 메인 어플리케이션과 별개로 따로 구동이 가능한 웹사이트**이다.

컴포넌트의 구조와 형태를 쉽게 파악할 수 있어 많은 개발자들이 사용하고 있다.

위 글은 `Storybook`의 기능 중 기본이며, **공식 웹사이트를 읽어보면 더욱 강력하고 다양한 기능**을 사용할 수 있다.

<br><br><br>

---

### 출처

[https://storybook.js.org/](https://storybook.js.org/)

[https://poiemaweb.com/storybook](https://poiemaweb.com/storybook)

[https://www.daleseo.com/storybook/](https://www.daleseo.com/storybook/)
