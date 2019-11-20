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

随着html5```Canvas```元素的推出呢，现在的浏览器具备了更强大的绘制图像的功能，甚至```canvas```已经可以用来制作大型网页游戏，关于Canvas的js库也越来越多，有动画库还有图表库比如Echart等等。今天我就要给大家推荐一款非常炫酷的Canvas粒子特效，particleJs——一个轻量级的创建粒子背景的 JavaScript 库。

#### 先热下身实现一个简单的小球动画：
```js
<canvas id="myCanvas" width="600" height="400" style="background:#f1f1f1;"></canvas>
```
```js
let myCanvas = document.getElementById('myCanvas');
myCanvas.width = 600;
myCanvas.height = 400;
let ctx = myCanvas.getContext('2d');
  // 把圆心坐标抽成变量
let _x = 30;
let _y = 30;
// 为画布创建定时器, 一秒50帧
setInterval(function() {
  // 清屏
  ctx.clearRect(0, 0, 600, 400);
  ctx.beginPath();
  // 画圆
  ctx.arc(_x, _y, 15, 0, 2 * Math.PI);
  ctx.fillStyle = '#f60';
  ctx.fill();
  // 画完后改变圆心坐标变量
  _x += 6;
  _y += 4;
  if (_y > 400) {
    _x = 30;
    _y = 30;
  }
}, 20)
```
效果如下
![avatar](https://img1.dxycdn.com/2019/1120/844/3380695209008156952-2.gif)
### 多个球的运动
```js
<body>
  <canvas id="myCanvas" width="600" height="400" style="background: #f1f1f1;"></canvas>
  <script type="text/javascript">
    // 获取画布节点
    let myCanvas = document.getElementById('myCanvas');
    // 设置画布的宽高
    myCanvas.width = 600;
    myCanvas.height = 400;
    // 获取画布的上下文对象
    let ctx = myCanvas.getContext('2d');
    // 小球的构造函数, 传坐标, 半径和速度形参
    function Ball(x, y, r, speed) {
      this.x = x;
      this.y = y;
      this.r = r;
      this.speed = speed;
      // new出来的时候直接调用下面写的渲染方法把小球放到画布上
      this.render();
    }
    // 渲染小球的方法
    Ball.prototype.render = function() {
      ctx.beginPath();
      ctx.arc(this.x, this.y, this.r, 0, 2 * Math.PI);
      ctx.fillStyle = 'skyblue';
      ctx.fill();
    }
    // 更新小球实例的字段的方法
    Ball.prototype.update = function() {
      this.x += this.speed * 3;
      this.y += this.speed * 2;
      if (this.y > 400) {
        this.x = 30;
        this.y = 30;
      }
    }

    // 创建出来的小球实例放在这个数组里
    let ballArr = [];
    // 每个小球实例的实参都可以不同
    ballArr[0] = new Ball(30, 30, 10, 2);
    ballArr[1] = new Ball(30, 70, 15, 1.5);
    ballArr[2] = new Ball(30, 110, 20, 1);

    // 画布唯一定时器, 每秒50帧, 每一帧都先清屏
    setInterval(function() {
      ctx.clearRect(0, 0, myCanvas.width, myCanvas.height);
      for (var i = 0; i < ballArr.length; i++) {
        // 每个小球的实例调用自己的update方法来改变自己的坐标字段
        ballArr[i].update();
        // 每个小球的实例调用自己的render方法来绘制改变坐标后的小球
        ballArr[i].render();
      }
    }, 20);
  </script>
</body>
```
效果
![avatar](https://img1.dxycdn.com/2019/1120/900/3380699274194521961-2.gif)
##### 下面这张图片有木有很熟悉，其实下边这个背景粒子效果就可以通过particle.js实现的。
![avatar](https://img1.dxycdn.com/2019/1120/285/3380684252546350091-2.png)

#### 引入方式
```js
<div id="particles-js"></div>

<script src="particles.js"></script>
```
#### 重要配置文件
app.js
```js
particlesJS.load('particles-js', 'assets/particles.json', function() {
  console.log('callback - particles.js config loaded');
});
```
```js
{
  "particles": {
    "number": {
      "value": 80,
      "density": {
        "enable": true,
        "value_area": 800
      }
    },
    "color": {
      "value": "#ffffff"
    },
    "shape": {
      "type": "circle",
      "stroke": {
        "width": 0,
        "color": "#000000"
      },
      "polygon": {
        "nb_sides": 5
      },
      "image": {
        "src": "img/github.svg",
        "width": 100,
        "height": 100
      }
    },
    "opacity": {
      "value": 0.5,
      "random": false,
      "anim": {
        "enable": false,
        "speed": 1,
        "opacity_min": 0.1,
        "sync": false
      }
    },
    "size": {
      "value": 10,
      "random": true,
      "anim": {
        "enable": false,
        "speed": 80,
        "size_min": 0.1,
        "sync": false
      }
    },
    "line_linked": {
      "enable": true,
      "distance": 300,
      "color": "#ffffff",
      "opacity": 0.4,
      "width": 2
    },
    "move": {
      "enable": true,
      "speed": 12,
      "direction": "none",
      "random": false,
      "straight": false,
      "out_mode": "out",
      "bounce": false,
      "attract": {
        "enable": false,
        "rotateX": 600,
        "rotateY": 1200
      }
    }
  },
  "interactivity": {
    "detect_on": "canvas",
    "events": {
      "onhover": {
        "enable": false,
        "mode": "repulse"
      },
      "onclick": {
        "enable": true,
        "mode": "push"
      },
      "resize": true
    },
    "modes": {
      "grab": {
        "distance": 800,
        "line_linked": {
          "opacity": 1
        }
      },
      "bubble": {
        "distance": 800,
        "size": 80,
        "duration": 2,
        "opacity": 0.8,
        "speed": 3
      },
      "repulse": {
        "distance": 400,
        "duration": 0.4
      },
      "push": {
        "particles_nb": 4
      },
      "remove": {
        "particles_nb": 2
      }
    }
  },
  "retina_detect": true
}

```
### `Options`

key | option type / notes | example
----|---------|------
`particles.number.value` | number | `40`
`particles.number.density.enable （粒子稀密程度）` | boolean | `true` / `false` 
`particles.number.density.value_area (每一个粒子所占用的空间)` | number | `800`
`particles.color.value` | HEX (string) <br /> RGB (object) <br /> HSL (object) <br /> array selection (HEX) <br /> random (string) | `"#b61924"` <br /> `{r:182, g:25, b:36}` <br />  `{h:356, s:76, l:41}` <br /> `["#b61924", "#333333", "999999"]` <br /> `"random"`
`particles.shape.type` | string <br /> array selection | `"circle"` <br /> `"edge"` <br /> `"triangle"` <br /> `"polygon"` <br /> `"star"` <br /> `"image"` <br /> `["circle", "triangle", "image"]`
`particles.shape.stroke.width` | number | `2`
`particles.shape.stroke.color` | HEX (string) | `"#222222"`
`particles.shape.polygon.nb_slides` | number | `5`
`particles.shape.image.src` | path link <br /> svg / png / gif / jpg | `"assets/img/yop.svg"` <br /> `"http://mywebsite.com/assets/img/yop.png"`
`particles.shape.image.width` | number <br />(for aspect ratio) | `100`
`particles.shape.image.height` | number <br />(for aspect ratio) | `100`
`particles.opacity.value` | number (0 to 1) | `0.75`
`particles.opacity.random` | boolean | `true` / `false` 
`particles.opacity.anim.enable` | boolean | `true` / `false` 
`particles.opacity.anim.speed` | number | `3`
`particles.opacity.anim.opacity_min` | number (0 to 1) | `0.25`
`particles.opacity.anim.sync` | boolean | `true` / `false`
`particles.size.value` | number | `20`
`particles.size.random` | boolean | `true` / `false` 
`particles.size.anim.enable` | boolean | `true` / `false` 
`particles.size.anim.speed` | number | `3`
`particles.size.anim.size_min` | number | `0.25`
`particles.size.anim.sync` | boolean | `true` / `false`
`particles.line_linked.enable` | boolean | `true` / `false`
`particles.line_linked.distance` | number | `150`
`particles.line_linked.color` | HEX (string) | `#ffffff`
`particles.line_linked.opacity` | number (0 to 1) | `0.5`
`particles.line_linked.width` | number | `1.5`
`particles.move.enable` | boolean | `true` / `false`
`particles.move.speed` | number | `4`
`particles.move.direction` | string | `"none"` <br /> `"top"` <br /> `"top-right"` <br /> `"right"` <br /> `"bottom-right"` <br /> `"bottom"` <br /> `"bottom-left"` <br /> `"left"` <br /> `"top-left"`
`particles.move.random` | boolean | `true` / `false`
`particles.move.straight` | boolean | `true` / `false`
`particles.move.out_mode` | string <br /> (out of canvas) | `"out"` <br /> `"bounce"`
`particles.move.bounce` | boolean <br /> (between particles) | `true` / `false`
`particles.move.attract.enable` | boolean | `true` / `false`
`particles.move.attract.rotateX` | number | `3000`
`particles.move.attract.rotateY` | number | `1500`
`interactivity.detect_on` | string | `"canvas", "window"`
`interactivity.events.onhover.enable` | boolean | `true` / `false`
`interactivity.events.onhover.mode` | string <br /> array selection | `"grab"` <br /> `"bubble"` <br /> `"repulse"` <br /> `["grab", "bubble"]`
`interactivity.events.onclick.enable` | boolean | `true` / `false`
`interactivity.events.onclick.mode` | string <br /> array selection | `"push"` <br /> `"remove"` <br /> `"bubble"` <br /> `"repulse"` <br /> `["push", "repulse"]`
`interactivity.events.resize` | boolean | `true` / `false`
`interactivity.events.modes.grab.distance` | number | `100`
`interactivity.events.modes.grab.line_linked.opacity` | number (0 to 1) | `0.75`
`interactivity.events.modes.bubble.distance` | number | `100`
`interactivity.events.modes.bubble.size` | number | `40`
`interactivity.events.modes.bubble.duration` | number <br /> (second) | `0.4`
`interactivity.events.modes.repulse.distance` | number | `200`
`interactivity.events.modes.repulse.duration` | number <br /> (second) | `1.2`
`interactivity.events.modes.push.particles_nb` | number | `4`
`interactivity.events.modes.push.particles_nb` | number | `4`
`retina_detect` | boolean | `true` / `false`

-------------------------------

 通过以上配置我们发挥自己的想象实现各种各样的效果。当然我们也可以直接访问[http://vincentgarreau.com/particles.js/](http://vincentgarreau.com/particles.js ).在线调试效果 
#####参考
github项目地址[https://github.com/VincentGarreau/particles.js/](https://github.com/VincentGarreau/particles.js/ )