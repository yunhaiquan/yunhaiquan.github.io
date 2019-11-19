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
|key	|option type / notes|	example|
| ------------- | ------------- | ------------- |
particles.number.value| number | 40 |
particles.number.density.enable | boolean | true / false |
particles.number.density.enable	|boolean|	true / false
particles.number.density.value_area|	number|	800
particles.color.value	HEX (string)|RGB (object)HSL(object)array selection (HEX)random (string)	|"#b61924" {r:182, g:25, b:36} {h:356, s:76, l:41} ["#b61924", "#333333", "999999"] "random"
particles.shape.type|	string array selection	|"circle" "edge" "triangle" "polygon" "star" "image" ["circle", "triangle", "image"] 
particles.shape.stroke.width	|number	|2
particles.shape.stroke.color|	HEX (string)|	"#222222"
particles.shape.polygon.nb_slides|	number|	5
particles.shape.image.src|	path link|svg / png / gif / jpg	"assets/img/yop.svg" "http://mywebsite.com/assets/img/yop.png"
particles.shape.image.width	|number (for aspect ratio)|	100
particles.shape.image.height|	number(for aspect ratio)|	100
particles.opacity.value	|number (0 to 1)|	0.75
particles.opacity.random|	boolean	|true / false
particles.opacity.anim.enable	|boolean	true / false
particles.opacity.anim.speed|	number|	3
particles.opacity.anim.opacity_min|number (0 to 1)|	0.25
particles.opacity.anim.sync	|boolean	|true / false
particles.size.value|	number|	20
particles.size.random	|boolean|	true / false
particles.size.anim.enable|	boolean|	true / false
particles.size.anim.speed	|number	|3
particles.size.anim.size_min|	number|	0.25
particles.size.anim.sync|	boolean	|true / false
particles.line_linked.enable|	boolean|	true / false
particles.line_linked.distance|	number|	150
particles.line_linked.color	|HEX (string)|	#ffffff
particles.line_linked.opacity	|number (0 to 1)|	0.5
particles.line_linked.width|	number|	1.5
particles.move.enable	|boolean	|true / false
particles.move.speed	|number|	4
particles.move.direction	|string|	"none""top" "top-right" "right" "bottom-right" "bottom" "bottom-left" "left" "top-left"
particles.move.random	|boolean|	true / false
particles.move.straight	|boolean|	true / false
particles.move.out_mode	|string(out of canvas)|	"out" "bounce"
particles.move.bounce	|boolean(between particles)|	true / false
particles.move.attract.enable	|boolean	|true / false
particles.move.attract.rotateX|	number|	3000
particles.move.attract.rotateY|	number|	1500
interactivity.detect_on	|string	|"canvas", "window"
interactivity.events.onhover.enable	|boolean|	true / false
interactivity.events.onhover.mode	|string|array selection	"grab" "bubble" "repulse" ["grab", "bubble"]
interactivity.events.onclick.enable|	boolean	|true / false
interactivity.events.onclick.mode|	stringarray selection	|"push" "remove" "bubble" "repulse" ["push", "repulse"]
interactivity.events.resize|boolean	|true / false
interactivity.events.modes.grab.distance|	number|	100
interactivity.events.modes.grab.line_linked.opacity	|number (0 to 1)|	0.75
interactivity.events.modes.bubble.distance|	number|	100
interactivity.events.modes.bubble.size|	number|	40
interactivity.events.modes.bubble.duration|	number(second)|0.4
interactivity.events.modes.repulse.distance|	number|	200
interactivity.events.modes.repulse.duration|	number(second)|	1.2
interactivity.events.modes.push.particles_nb|	number|	4
interactivity.events.modes.push.particles_nb|	number|	4
retina_detect	|boolean|	true / false