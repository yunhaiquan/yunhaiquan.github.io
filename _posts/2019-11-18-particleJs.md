---
layout:     post
title:      "背景粒子效果的实现"
subtitle:   "particlesJs"
date:       2019-11-18
author:     "Yhq"
header-img: "img/post-bg-js-module.jpg"
tags:
    - 前端开发
    - 实用插件类
---

## Foreword

随着html5```Canvas```元素的推出呢，现在的浏览器具备了更强大的绘制图像的功能，甚至```canvas```已经可以用来制作大型网页游戏，关于Canvas的js库也越来越多，有动画库还有图表库比如Echart等等。今天我就要给大家推荐两款非常炫酷的Canvas粒子特效，particleJs————一个轻量级的创建粒子背景的 JavaScript 库。

首先我们来看一下它的使用方式

```js
<div id="particles-js"></div>
<script src="particles.js"></script>
```
app.js
```js
particlesJS.load('particles-js', 'assets/particles.json', function() {
  console.log('callback - particles.js config loaded');
});
```
```json
