---
title: "鼠标悬浮放大"
description: 实现鼠标悬浮时，容器放大，内容不放大的效果
toc: true
date: 2021-10-27T23:33:33+08:00
categories:
  - 开发小记
draft: false
---

## 伪元素实现

实现原理：设置容器的伪元素，使其和容器等大小，鼠标悬浮时对伪元素进行放大，需注意 [层叠上下文](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_Positioning/Understanding_z_index/The_stacking_context)

[Demo](https://codesandbox.io/s/code-snippets-s5rxe?file=/index.html)

```html
<div class="container">
  <div class="card">
    <h4>Title</h4>
    <p>Content...</p>
    <p>Content...</p>
    <p>Content...</p>
  </div>
  <div class="card">
    <h4>Title</h4>
    <p>Content...</p>
    <p>Content...</p>
    <p>Content...</p>
  </div>
</div>
```

```css
.container {
  display: flex;
}

.card {
  position: relative;
  width: 200px;
  padding: 10px;
  border-radius: 5px;
  margin-right: 10px;
  background-color: lightblue;
  cursor: pointer;
}

.card::after {
  position: absolute;
  top: 0;
  left: 0;
  content: "";
  width: 100%;
  height: 100%;
  border-radius: 5px;
  transition: all 0.5s;
  background-color: lightblue;
  /* 若不设置为负数，则伪元素会覆盖掉容器 */
  z-index: -1;
}

.card:hover {
  /* 提升容器的层级，使其放大时覆盖在后续元素的上方 */
  z-index: 1;
}

.card:hover::after {
  transform: scale(1.2);
  box-shadow: 0 0 10px gray;
}
```

这样的实现方式有个不足之处，即容器的 z-index 是在 hover 时立刻提升的，取消 hover 时 z-index 则会立刻复原，在过渡动画中会有轻微的闪动（z-index 的改变不参与 transition），导致原本放大的伪元素在取消 hover 时会立即显示在后续元素的下方。
