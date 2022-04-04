---
layout: single
title: "[Web] MVC Pattern Practice"
categories: [Web, Frontend]
last_modified_at: 2021-09-20
excerpt: "MVC Pattern 실습"
---

![notion-005](https://user-images.githubusercontent.com/72294509/156121276-300d14e0-0353-44b8-9149-10fbf560bd24.png)

# MVC Pattern

> 개념만 배우고 한번도 적용해본 적 없는 MVC 페턴. 직접 코드에 적용시켜 보고자 글을 작성합니다.

![https://user-images.githubusercontent.com/72294509/146886750-ba789870-aea4-4a94-82b7-363e0dd7e638.jpeg](https://user-images.githubusercontent.com/72294509/146886750-ba789870-aea4-4a94-82b7-363e0dd7e638.jpeg)

말이 됩니까..

4학년이 이제서야 MVC 페턴 실습을 하고 있다는 것이..!
<br>

## MVC가 뭘까

우선 MVC란 Model, View, Controller의 세 영역이다.

즉, 각 영역의 코드 결합도를 최소화 시키는 것을 목적으로 하는 개발 페턴

1. 사용자는 application과 상호작용
2. controller의 event handler 작동
3. Model에서 데이터 전송 → controller → 결과 : View
4. View는 결과 rendering

→ 결론적으로 페턴의 주요 목적은 **모델(기능)과 뷰(렌더링)의 코드 결합도를 최소화** 시키는 것
<br>

## 역할 정의

**Model** : 어플리케이션의 비지니스 로직, 사용되는 데이터를 다루는 영역

**View** : 최종 사용자에게 보여줄 화면 렌더링 담당

**Controller**: Model과 View 영역간의 코드 결합도를 줄여주는 영역 (중요!!)
<br>

## 실습 코드

- 이번 우아한 테크코스 프리코스를 진행하며 조금이나마 익숙해질 수 있었던 Class형으로 MVC를 구현해봤다.
- Model.js

```jsx
class Item {
  constructor(content) {
    this.content = content;
    this.finished = false;
  }
}

export default class Model {
  constructor() {
    this.list = [];
  }

  addItem(content) {
    this.item = new Item(content);
    this.list.push(this.item);
  }

  removeItem(itemIndex) {
    this.list.splice(itemIndex, 1);
  }

  checkItem(itemIndex) {
    const currentItem = this.list[itemIndex];
    currentItem.finished = !currentItem.finished;
  }
}
```

- View.js

```jsx
export default class View {
  constructor(list) {
    this.toDoList = document.querySelector(".to-do-list");
    this.finishedList = document.querySelector(".finished-list");
    this.list = list;
  }

  showList(list) {
    this.finishedList.innerHTML = "";
    this.toDoList.innerHTML = "";

    list.forEach((item, i) => {
      if (!item.finished) {
        console.log(item);
        const toDoItemHTML =
          '<li class="to-do-list__item" id="item-' +
          i +
          '">' +
          '<div class="item__content">' +
          item.content +
          "</div>" +
          '<div class="item__action">' +
          '<i class="fa fa-trash"></i>' +
          '<input type="checkbox">' +
          "</div>" +
          "</li>";
        this.toDoList.insertAdjacentHTML("afterbegin", toDoItemHTML);
        console.log(item.content);
        return;
      }

      const finishedItemHTML =
        '<li class="to-do-list__item" id="item-' +
        i +
        '">' +
        '<div class="item__content">' +
        item.content +
        "</div>" +
        '<div class="item__action">' +
        '<i class="fa fa-trash"></i>' +
        '<input type="checkbox" checked>' +
        "</div>" +
        "</li>";
      this.finishedList.insertAdjacentHTML("afterbegin", finishedItemHTML);
    });
  }
}
```

- Controller.js

```jsx
import Model from "./Model.js";
import View from "./View.js";

export default class Controller {
  constructor() {
    this.model = new Model();
    this.view = new View();
    this.form = document.forms["list-form"];
    this.addInput = this.form["add-item__input"];
    this.searchInput = this.form["search-item__input"];
    this.section = document.querySelector("section");
    this.setfunction();
  }

  setfunction() {
    this.addItem();
    this.searchItem();
    this.removeItem();
    this.checkItem();
  }

  addItem() {
    this.form.addEventListener("submit", (e) => {
      e.preventDefault();

      const addVal = this.addInput.value;
      console.log(addVal);

      this.model.addItem(addVal);
      console.log(this.model.list);
      this.view.showList(this.model.list);

      this.form.reset();
    });
  }

  searchItem() {
    this.searchInput.addEventListener("input", (e) => {
      e.preventDefault();
      const searchVal = this.searchInput.value;

      const filterList = this.model.list.filter((item) => {
        return item.content.indexOf(searchVal) > -1;
      });

      this.view.showList(filterList);
    });
  }

  removeItem() {
    this.section.addEventListener("click", (e) => {
      if (e.target.tagName !== "I") return;
      console.log("remove");
      const itemId = e.target.parentNode.parentNode.id;
      const itemIndex = itemId.split("-")[1];

      this.model.removeItem(itemIndex);

      this.view.showList(this.model.list);
    });
  }

  checkItem() {
    this.section.addEventListener("change", (e) => {
      console.log("check");
      if (e.target.tagName !== "INPUT") return;

      const itemId = e.target.parentNode.parentNode.id;
      const itemIndex = itemId.split("-")[1];

      this.model.checkItem(itemIndex);

      this.view.showList(this.model.list);
    });
  }
}
```

- index.js

```jsx
import Controller from "./Controller.js";
import Model from "./Model.js";
import View from "./View.js";

new Model();
new View();
new Controller();
```

- index.html (기술 블로그 MVC 실습에서 가져왔다.)

```html
<!DOCTYPE html>
<html lang="ko">
  <head>
    <meta charset="UTF-8" />
    <title>mvc pattern practice</title>
    <link rel="stylesheet" href="style.css" />
  </head>
  <body>
    <div class="list-container">
      <header>
        <form id="list-form">
          <div class="list-form__add-item">
            <input type="text" id="add-item__input" required autofocus />
            <input type="submit" value="✚" />
          </div>

          <div class="list-form__search-item">
            <input type="search" id="search-item__input" />
            <label for="search-item__input">
              <i class="fa fa-search"></i>
            </label>
          </div>
        </form>
      </header>

      <section>
        <ul class="to-do-list"></ul>
        <hr />
        <ul class="finished-list"></ul>
      </section>

      <div class="text-center">
        <a href="https://github.com/EastSun5566" target="_blank">
          <i class="fa fa-github" aria-hidden="true"></i>
        </a>
      </div>
    </div>
    <script type="module" src="src/index.js"></script>
  </body>
</html>
```

- style.css

→ 사실 style은 크게 중요하지 않지만, index.html을 가져올 때 같이 가져왔다.

```css
@import url(//fonts.googleapis.com/earlyaccess/notosanstc.css);

* {
  /*  border: 1px solid #000; */
  position: relative;
  box-sizing: border-box;
}

:root {
  --color-primary: #d3cce3;
  --color-secondary: chocolate;
  --color-font-dark: darkred;
  --color-font-light: #eee;
}

body {
  margin: 0;
  min-height: 100vh;
  background: radial-gradient(circle, #e9e4f0, var(--color-primary));
  color: var(--color-font-dark);
  letter-spacing: 2px;
  font-family: "Noto Sans TC", sans-serif;

  display: flex;
  justify-content: center;
  align-items: center;
}

/* list container */
.list-container {
  width: 60%;
  padding: 16px 0;
}

@media (max-width: 500px) {
  .list-container {
    width: 100%;
  }
}

/* form */
#list-form {
  display: flex;
  justify-content: space-around;
}

.list-form__add-item,
.list-form__search-item {
  width: 45%;
}

input {
  background-color: transparent;
  border: 2px solid var(--color-secondary);
  border-radius: 100px;
  padding: 16px;

  text-indent: 8px;
  color: var(--color-font-dark);
  letter-spacing: 2px;

  transition: 0.5s;
}

input:focus {
  outline: none;
}

/* add input */
input[type="text"],
input[type="search"] {
  width: 100%;
}

#add-item__input:focus,
#search-item__input:focus {
  background-color: var(--color-secondary);
  color: var(--color-font-light);
}

input[type="text"] {
  padding-right: 40px;
}

input[type="submit"] {
  border: none;
  font-size: 16px;

  position: absolute;
  right: 0;
  top: 50%;
  transform: translateY(-50%);

  transition: 0.5s;
  cursor: pointer;
}

/* serch input */
input[type="search"] {
  text-indent: 24px;
}

label[for="search-item__input"] {
  position: absolute;
  left: 16px;
  top: 50%;
  transform: translateY(-50%);

  transition: 0.5s;
  cursor: pointer;
}

#add-item__input:focus + input[type="submit"],
#search-item__input:focus + label[for="search-item__input"] {
  color: var(--color-font-light);
}

/* list */
ul {
  list-style: none;
  padding: 0;
}

/* to do list */
.to-do-list:empty::before {
  content: "Nothing to do...";
  display: block;
  text-align: center;

  font-size: 12px;
  font-weight: lighter;
}

/* finished list */
.finished-list:empty::before {
  content: "Nothing finished...";
  display: block;
  text-align: center;

  font-size: 12px;
  font-weight: lighter;
}

li {
  border: 1.5px solid transparent;
  border-radius: 100px;
  margin: 8px;
  padding: 8px;

  display: flex;
  justify-content: space-between;
  align-items: center;
  font-weight: bold;

  transition: 0.5s;
  /*  animation: fadeIn 0.5s both; */
}

@keyframes fadeIn {
  0% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}

li:hover {
  /*  border-color: var(--color-secondary); */
}

.item__content {
  max-width: 80%;
  overflow: hidden;
  text-overflow: ellipsis;
}

/* item action */
.item__action {
  width: 20%;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

/* check input */
input[type="checkbox"] {
  appearance: none;
  padding: 8px;

  transition: 0.3s;
  cursor: pointer;
}

input[type="checkbox"]:checked,
input[type="checkbox"]:hover {
  background-color: var(--color-secondary);
}

input[type="checkbox"]:checked::after {
  content: "✔";
  position: absolute;
  top: 50%;
  left: 35%;
  transform: translate(-50%, -50%);

  color: #eee;
  font-weight: bold;
}

.fa-trash {
  display: block;
  cursor: pointer;
  background-color: aqua;

  transition: 0.3s 0.1s;
}

.fa-trash::after {
  content: "";
  position: absolute;
  right: -10px;
  width: 3px;
  height: 100%;

  background-color: var(--color-secondary);
  z-index: -1;

  transition: 0.3s;
}

.fa-trash:hover {
  color: var(--color-font-light);
}

.fa-trash:hover::after {
  width: 200%;
}

/* hr */
hr {
  width: 80%;
  border: 1px solid var(--color-secondary);
  opacity: 0.5;
}

.text-center {
  text-align: center;
}
```

아직 갈길은 멀다.. 왜냐 MVC 패턴에도 많은 문제가 있다.

**MVVM, MVP 등 여러가지 페턴에 대해 공부할 것**

조금 더 효율적인 디렉토리 파일 구성을 위해 더 열심히 공부할 것.
