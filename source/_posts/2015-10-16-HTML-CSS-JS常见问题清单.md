---
title: HTML/CSS/JS常见问题清单
date: 2015-10-16 15:31:58
tags: [HTML,CSS,JS]
permalink: html-css-js-common-questions-list
---
## CSS禁止a标签可点击 ##
```html
<a href="link.html" class="not-active">Link</a>

.not-active {
   pointer-events: none;
   cursor: default;
}
```
<!-- more -->