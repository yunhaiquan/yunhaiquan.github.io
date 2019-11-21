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
##### 下面这张图片有木有很熟悉，其实下边这个背景粒子效果就可以通过particle.js实现的。
![avatar](https://img1.dxycdn.com/2019/1120/285/3380684252546350091-2.png)
#### 根据上边的效果我们自己先做个小demo：
```js
<html>
 
<head>
  <meta charset="UTF-8">
  <title></title>
  <style type="text/css">
    * {
        margin: 0;
        padding: 0;
    }
    #myCanvas {
        background-color: transparent;
    }
  </style>
</head>
<body>
  <canvas id="myCanvas"></canvas>
</body>
<script type="text/javascript">
    let body = document.getElementsByTagName('body')[0]
    let canvas = document.getElementById("myCanvas");
    canvas.width = document.documentElement.clientWidth;
    canvas.height = document.documentElement.clientHeight;
    let ctx = canvas.getContext("2d");
    //创建小球的构造函数
    function Ball() {
      this.x = randomNum(3, canvas.width - 3);
      this.y = randomNum(3, canvas.height - 3);
      this.r = randomNum(1, 3);
      this.color = randomColor();
      this.speedX = randomNum(-3, 3) * 0.2;
      this.speedY = randomNum(-3, 3) * 0.2;
    }
    Ball.prototype = {
    //绘制小球
      draw () {
      ctx.beginPath();
      ctx.globalAlpha = 1;
      ctx.fillStyle = this.color;
      ctx.arc(this.x, this.y, this.r, 0, Math.PI * 2);
      ctx.fill();
      },
      //小球移动
      move () {
        this.x += this.speedX;
        this.y += this.speedY;
        //为了合理性,设置极限值
        if (this.x <= 3 || this.x > canvas.width - 3) {
            this.speedX *= -1;
        }
        if (this.y <= 3 || this.y >= canvas.height - 3) {
            this.speedY *= -1;
        }
      }
    }
    //存储所有的小球
    var balls = [];
    //创建200个小球
    for (var i = 0; i < 150; i++) {
      var ball = new Ball();
      balls.push(ball);
    }
    main()
    function main() {
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      //鼠标移动绘制线				
      mouseLine();
      //小球与小球之间自动画线
      drawLine();
      //使用关键帧动画，不断的绘制和清除
      window.requestAnimationFrame(main);
    }
    //添加鼠标移动事件
    //记录鼠标移动时的mouseX坐标
    var mouseX;
    var mouseY;
    body.onmousemove = function (e) {
      var ev = event || e;
      mouseX = ev.offsetX;
      mouseY = ev.offsetY;
      console.log(mouseX, mouseY)
    }
    //判断是否划线
    function drawLine() {
      for (var i = 0; i < balls.length; i++) {
        balls[i].draw();
        balls[i].move();
        for (var j = 0; j < balls.length; j++) {
          if (i !== j) {
          if (Math.sqrt(Math.pow((balls[i].x - balls[j].x), 2) + Math.pow((balls[i].y - balls[j].y), 2)) < 80) {
              // 使用 beginPath() 绘制路径的起始点， 使用 moveTo()移动画笔， 使用 stroke() 方法真正地画线。
              ctx.beginPath();
              ctx.moveTo(balls[i].x, balls[i].y);
              ctx.lineTo(balls[j].x, balls[j].y);
              ctx.strokeStyle = "white";
              ctx.globalAlpha = 0.2;
              ctx.stroke();
            }
          }
        }
      }
    }
    // 使用鼠标移动划线
    function mouseLine() {
      for (var i = 0; i < balls.length; i++) {
        if (Math.sqrt(Math.pow((balls[i].x - mouseX), 2) + Math.pow((balls[i].y - mouseY), 2)) < 80) {
          ctx.beginPath();
          ctx.moveTo(balls[i].x, balls[i].y);
          ctx.lineTo(mouseX, mouseY);
          ctx.strokeStyle = "white";
          ctx.globalAlpha = 0.8;
          ctx.stroke();
        }
      }
    }
    //随机函数
    function randomNum(m, n) {
      return Math.floor(Math.random() * (n - m + 1) + m);
    }
    //随机颜色
    function randomColor() {
      return "rgb(" + randomNum(0, 255) + "," + randomNum(0, 255) + "," + randomNum(0, 255) + ")";
    }
</script>
 
</html>

```
效果
![avatar](https://img1.dxycdn.com/2019/1121/713/3380799591745673625-2.png)


###插件的简单使用

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