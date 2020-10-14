# Concentrix 响应式页面布局



<font color=red>说明：</font>

<font color=green>聚思鸿动效页面采用响应式页面布局 单位使用vw、vh</font>
<font color=green>主要使用技术包括：scrollMagic、tweenMax、swiper、canvas</font>




### 一、规范及说明

#### 1.1 SEO优化

类名和图片名：传播名-该dom层图片实际意义

```html
<!-- div背景 css中 background-image: url('p30-bgimage-accessorize.jpg') -->
<div class='p30-container'>
	<img class='p30-pic-accessorize.jpg' alt='HUAWEI 图片实际意义'>
</div>
```



#### 1.2 需要兼容的浏览器

支持chrome firefox IE11 edge 移动端

移动端宽度支持360 375 414

ipad 竖版 768×1024 1024×1366 横版 1024×768 1366×1024 ipad-pro-9.7 834×1087(竖版)



#### 1.3 使用的组件库

Swiper：https://www.swiper.com.cn/api/index.html
ScrollMagic：http://scrollmagic.io/docs/index.html
TweenMax：https://www.tweenmax.com.cn/api/tweenmax/	



------



### 二、相关技术

#### 2.1 表单序列化

活动页面中的参数通过表单序列化的方式获得

序列化得到的是一个字符串 可以对该字符串进行拼接

```javascript
data: `${$('#form').serialize()}&field_8=${field_8}`
```



#### 2.2 Srcset自适应图片

图像宽度使用相对单位 如100% 防止图片溢出

img元素的srcset属性用于浏览器根据宽、高和像素密度来加载相应的图片资源

属性格式: 图片地址 宽度描述w 像素密度描述x 多个资源之间用逗号分隔

```html
<img srcset="elva-fairy-320w.jpg 320w,
             elva-fairy-480w.jpg 480w,
             elva-fairy-800w.jpg 800w"
     sizes="(max-width: 320px) 280px,
            (max-width: 480px) 440px,
            800px"
     src="elva-fairy-800w.jpg"
     alt="Elva dressed as a fairy" />
```

srcset表示当屏幕宽度分别为320 480 800时 src指向不同的图片地址

sizes表示当屏幕宽度不大于320时 图片渲染280宽度

屏幕宽度不大于480时 图片渲染440宽度

上述条件没有匹配项时 图片渲染800宽度 注意顺序 浏览器匹配第一个之后 不会再看其他项

src表示默认渲染的图片地址 没有选项匹配时 则会渲染默认图片地址



#### 2.3 图片响应式 picture标签

```html
<picture>
	<source media="(max-width: 1023px)" srcset="img/1x/pic_s4_pc_phone.png">
    <!-- 屏幕宽度小于1023时 渲染上图 大于等于1024 分辨率1.5x时 渲染下图 -->
	<source media="(min-width: 1024px)" srcset="img/2x/pic_s4_pc_phone@2x.png 1.5x">
    <!-- 没有匹配项时 渲染下面的默认图片 -->
	<img src="img/1x/pic_s4_pc_phone.png" alt="HUAWEI nova 5i AI Quad lens">
</picture>
```



#### 2.4 @font-face自定义字体样式

```css
@font-face {
    font-family: "Manrope";
    // woff2是woff的压缩版本 体积更小 优先使用
	src: url("/content/dam/huawei-cbg-site/greate-china/cn/mkt/pdp/phones/nova5i/fonts/manrope-medium.woff2") format("woff2"),
    // woff兼容Modern Browsers
	url("/content/dam/huawei-cbg-site/greate-china/cn/mkt/pdp/phones/nova5i/fonts/manrope-medium.woff") format("woff"),
    // truetype兼容Safari、Android、iOS
	url("/content/dam/huawei-cbg-site/greate-china/cn/mkt/pdp/phones/nova5i/fonts/manrope-medium.otf") format("truetype");
    font-style: normal;
    font-weight: 500;
}
```



#### 2.5 @media媒体查询

```css
/* 屏幕宽度大于960且小于1200时执行 */
@media screen and (min-width:960px) and (max-width:1200px) {
    body{
        background:yellow;
    }
}
@media screen and (max-aspect-ratio: 1/1) {
	/* 屏幕宽高比最大为1时 为竖屏 */
}
@media screen and (min-aspect-ratio: 1/1) {
	/* 屏幕宽高比最大为1时 为横屏 */
}

@media screen and (min-width: 1200px) {
    /* pc */
}
@media (max-width: 600px), (max-aspect-ratio: 1/1) and (max-width: 1199px) {
    /* mob ipad竖版 ipad-pro竖版 */
}
@media only screen and (width: 768px) and (max-aspect-ratio: 1/1) {
    /* ipad竖版 */
}
@media only screen and (width: 1024px) and (min-aspect-ratio: 1/1) {
    /* ipad横板 */
}
@media only screen and (width: 1024px) and (max-aspect-ratio: 1/1) {
    /* ipad-pro竖版 */
}
@media only screen and (width: 1366px) and (min-aspect-ratio: 1/1) {
    /* ipad-pro横板 */
}
```



#### 2.6 video标签

```html
<video id="video" 
  style="object-fit:fill" 
  autoplay
  poster=""
  preload="none"
  muted
  loop="loop"
  webkit-playsinline 
  playsinline 
  x5-video-player-type="h5"
  x5-video-player-fullscreen="true"
  x-webkit-airplay="true"
>
	<source src="video/battery.mp4">
</video>

<!--
  object-fit: fill   视频内容充满整个video容器
  poster:"img.jpg"   视频封面
  autoplay： 自动播放
      auto - 当页面加载后载入整个视频
      meta - 当页面加载后只载入元数据
      none - 当页面加载后不载入视频

  muted：当设置该属性后 它规定视频的音频输出应该被静音

  webkit-playsinline playsinline:   内联播放 兼容iOS、Safari和安卓的一些浏览器
  // app要支持这种模式 webview.allowsInlineMediaPlayback = YES;

  x5-video-player-type="h5" :  启用x5内核H5播放器 兼容安卓微信内置浏览器
  x5-video-player-fullscreen="true"  全屏设置 ture和false的设置会导致布局上的不一样
  x5-video-orientation="portraint" ：声明播放器支持的方向 可选值landscape 横屏 portraint竖屏
                                     默认值portraint 无论是直播还是全屏H5一般都是竖屏播放
                                     但是这个属性需要x5-video-player-type开启H5模式
-->
```



------



### 三、scrollMagic基本用法

```javascript
// 初始化控制器 Controller用于总体的调度
let controller = new ScrollMagic.Controller({
    // 这其中的选项将会被传递到每一个场景
    globalSceneOptions: {
        reverse: true // 反向滚动时执行动画
    }
});
// 创建新的时间轴
let s3_animate2 = new TimelineLite();
// 在时间轴上添加动画效果
s3_animate3.to('做动画的元素', 1, { opacity: 1, ease: Power2.easeInOut });
// Scene场景类 用于设计具体的变换
new ScrollMagic.Scene({
    triggerElement: '#scroll', // 触发元素(一般放在要做动画元素的外部 一个空div即可)
    // 触发元素相对于窗口的位置(0-1之间的数字) 到此位置时才会触发
    // 1 => 'onEnter'  0.5 => 'onCenter'  0 => 'onLeave'
    triggerHook: 0.5,
    duration: '300%', // 效果持续距离 也可以使用百分比(一般在100%-300% 即1到3倍的屏幕高度)
    offset: 0 // 滚动到什么位置触发效果
})
    .setPin('#stick', { pushFollowers: true }) // 要固定的元素 pushFollowers将滚动距离添加至动画元素高
 	// 添加动画效果 需要插件animation.gsap.min.js
	// 引入此插件后 动画时间不再生效 只控制进度
	// 如第一个动画时间1秒 动画总时长2秒 duration为100% 则第一个动画占duration的50%
	.setTween(s3_animate3)
	.setClassToggle('选择器', '要切换的类名') // 切换选中元素类名
	.addIndicators() // 添加debug信息
    .addTo(controller); // 在控制器中注册此场景
// 注意: 为了兼容不同分辨率的设备 持续距离、触发位置可以用下例表示 如
// window.innerHeight * 3 / 4 表示在屏幕3 / 4位置处触发效果 或效果持续3 / 4屏幕
// 做动画的元素宽度不要超过屏幕宽度 否则可能造成计算错误
```

scrollMagic本身没有动画 要配合tweenMax一起使用

```javascript
let s5_animate = new TimelineLite();
// 在创建的时间轴上定义动画 s5-bg-pic向左偏移50% 放大至屏幕大小 取消圆角边框
s5_animate.to('.s5-bg-pic', 1, { x: '-50%', ease: Power2.easeInOut })
    .to('.s5-bg-pic', 1, {
    	width: '100vw',
    	height: '100vh',
    	'border-radius': 0,
    	ease: Power2.easeInOut
	});
// 定义ScrollMagic对象 加入s5_animate动画 s5-trigger离开时触发 触发期间固定section5元素
new ScrollMagic.Scene({ triggerElement: '#s5-trigger', duration: '100%', triggerHook: 0 })
	.setTween(s5_animate)
	.setPin('.section5')
	.addTo(controller);
```

scrollMagic事件

```javascript
let scene = new ScrollMagic.Scene({});
scene.on("start", function (event) {
	// start进入 end离开 progress进行中 enter leave
	console.log("进入场景");

    // 判断滚动方向
    if (event.scrollDirection == "FORWARD") {
        // 正向滚动
    } else if(event.scrollDirection == "REVERSE") {
        // 反向滚动
    }
});

// 判断滚动进度
scene.on("progress", function (event) {
    console.log(event.progress); // 0-1的数字
});
```

eg: 滚动切换背景颜色(如果采用精灵图 修改bgp可做到简单的动画)

```js
let controller = new ScrollMagic.Controller();

let len = 50; // 滚动的单位距离
let scroll = document.getElementById('scroll');
for (let i = 1; i <= 6; i++) {
    // 利用循环定义数个场景
    new ScrollMagic.Scene({
        triggerElement: '#scroll',
        triggerHook: 0,
        duration: 300,
        offset: i * len, // 不同的滚动距离
        reverse: true
    })
        .setClassToggle(scroll, `step${i}`) // 给scroll对象添加类名
        .on('enter', () => { // 定义滚动到定义距离时的回调函数
        	// 可以使用setTimeout来实现延时触发效果    
        	console.log(i);
        })
        .addTo(controller);
}
```

```css
body {
  height: 2000px;

  #scroll {
    margin-top: 300px;
    height: 300px;
  }

  .step1 {
    background-color: purple;
  }
  .step2 {
    background-color: black;
  }
  .step3 {
    background-color: pink;
  }
  .step4 {
    background-color: red;
  }
  .step5 {
    background-color: blue;
  }
  .step6 {
    background-color: cyan;
  }
}
```



------



### 四、tweenMax

tweenMax是一个动画库 动画效果**尽量使用transform**   top left等性能较差

一般使用TimelineLite的方式来简写动画效果

#### 4.1 动画结构

##### 4.1.1 TweenMax()

```javascript
// TweenMax的构造函数 创建TweenMax对象
// 其中包含三个参数 分别是 动画对象 执行时间 动画效果
new TweenMax('.box', 3, {
    x: 500,
    y() { // 也可以写成回调函数
        return Math.random() * 300
    },
    alpha : 0.3,
});
```

##### 4.1.2 TweenMax.to()

```javascript
// 对多个目标进行动画 第一个参数为动画对象 也可以写成.box
TweenMax.to(['.box1', '.box2', '.box3'], 1, {
    x(index) {
        return (index + 1) * 100 // 100 200 300
    },
    alpha: 0.3,
});
```

```html
<div class="box box1"></div>
<div class="box box2"></div>
<div class="box box3"></div>
```

##### 4.1.3 TweenMax.from()

与TweenMax.to()正好相反

##### 4.1.4 TweenMax.fromTo()

```javascript
// 第三、第四个参数分别可以设置动画的起点和终点
// 比如box1从300移动到700 box2从300移动到800
TweenMax.fromTo(['.box1', '.box2', '.box3'], 4, {
    x(index) {
        return 300;
    },
    alpha: 0.3
}, {
    x(index) {
        return (index + 6) * 100;
    },
    alpha: 0.6
});
```

##### 4.1.5 TweenMax.staggerTo()

```javascript
// 此方法为多个目标制作有间隔的序列动画
let obj = {};
// 第一个参数指定动画对象 第二个参数为动画执行时间
// 第三个参数为动画的一些效果 第四个参数为动画执行的时间间隔
// 第五个参数为动画完毕后执行的回调函数
// 第六个参数为上述回调函数的参数 以数组形式传入 这里是data1和data2
// 最后一个参数是上述回调函数的执行环境this 这里指向obj
TweenMax.staggerTo(".box", 1, {rotation:360, y:200}, 0.5, function(data1, data2) {
    console.log(data1, data2, this);
}, ['data1', 'data2'], obj);
```

##### 4.1.6 TweenMax.staggerFrom()

与TweenMax.staggerTo()正好相反

##### 4.1.7 TweenMax.staggerFromTo()

```javascript
// 与TweenMax.staggerTo()基本一致 能设置动画的起点和终点
let obj = {};
TweenMax.staggerFromTo(".box", 1, { rotation: 360, y: 200 }, { rotation: 100, y: 100 }, 0.5, function (data1, data2) {
    console.log(data1, data2, this);
}, ['data1', 'data2'], obj);
```

##### 4.1.8 TweenMax.delayedCall()

```javascript
// 此方法提供在设定时间或帧数后调用函数的方法
let obj = {};
// 第一个参数 要延迟的秒数或帧数 第二个参数 要延迟执行的函数
// 第三个参数 传递给上述函数的参数 以数组形式传入
// 第四个参数 上述函数的作用域 this指向默认为空
// 最后一个参数 设定的时间是基于秒数还是帧数 默认为false=>秒数
TweenMax.delayedCall(2, function (data1, data2) {
    console.log(data1, data2, this);
}, ["param1", "param2"], obj, false);
```

##### 4.1.9 TweenMax.set()

```javascript
// TweenMax.set()立即设置目标属性值 不产生过渡动画
// 这样nbox由于设置了透视属性transformPerspective 而产生了3D效果
TweenMax.set(".nbox",{transformPerspective:300});
TweenMax.to(".box", 3, {rotationY:360, transformOrigin:"left top"});
// transformOrigin可以设置值或者是百分比 比如 100% 100%
```

```html
<div class="box"></div>
<div class="nbox box"></div>
```

也可以为一个数组设置属性

```javascript
TweenMax.set([obj1, obj2, obj3], {x:100, y:50, opacity:0});
```

#### 4.2 动画初始设置

##### 4.2.1 delay

动画开始前的延迟秒数(设置为帧数 则是帧数)

##### 4.2.2 ease

过渡效果的速度曲线 如Bounce.easeOut

##### 4.2.3 play paused

控制动画播放和暂停

paused默认为false 如果为true 则动画刚开始会暂停

```javascript
TweenMax.set(".nbox",{transformPerspective:300});
let tween = TweenMax.to(".box", 3, {rotationY:360, transformOrigin:"left top", paused: true, ease: Bounce.easeOut});
let btn = document.getElementById('btn');
btn.onclick = function() {
    tween.play();
}
```

##### 4.2.4 immediateRender

```javascript
// 此属性控制动画是否立即渲染 如from()的运动对象是立即渲染的 不想立即渲染 可以将此属性设置为false
TweenMax.from('.green', 3, {
    x: 500,
    delay:3,
});
TweenMax.from('.orange', 3, { // 此物块没有立即渲染
    x: 500,
    delay:3,
    immediateRender: false,
});
```

##### 4.2.5 overwrite

```javascript
// 用来控制同一个对象上有多个动画时的覆盖效果
TweenMax.to('.box', 6, {x: 700,y:100,});
TweenMax.to('.box', 3, {x: 200,overwrite:"none"});
// 1.none/false 不处理 前3秒x200 y100 后3秒x700 y100
// 2.all/true 覆盖所有 x200
// 3.auto 仅覆盖重复 默认
// 4.concurrent 同时发生 支覆盖同时发生的 不覆盖没启动的
// 5.allOnStart
// 6.preexisting
```

##### 4.2.6 useFrames

当设置为true时 对这个TweenMax对象的时间计算方式以帧为单位

```javascript
// 通过此方式设置帧速 帧速不能大于60
TweenMax.ticker.fps(60);
TweenMax.to('.box', 60, { x: 200, useFrames: true });
```

##### 4.2.7 lazy

延迟渲染可以提高性能 不进行延迟渲染 可以设置lazy: true

##### 4.2.8 autoCSS

自动识别css属性 css包装器 默认为true 如怀疑css被解析成其他属性时 可使此值为false并手动设置css: {}

```javascript
TweenMax.to('.box', 3, {
    autoCSS:false,   
    css:{
        x: 500,
        alpha : 0.3,
    }
});
```

##### 4.2.9 callbackScope

用于所有回调范围

##### 4.2.10 repeat

动画重复的次数 repeat为1时 动画一共执行2次 无限循环时 repeat = -1

##### 4.2.11 repeatDelay

每次重复之间的秒数或帧数

##### 4.2.12 yoyo

设置为true时 动画将往复运行 默认值为false **与repeat搭配使用**

##### 4.2.13 yoyoEase

定义动画返回时的缓动效果 如Bounce.easeOut **与yoyo: ture 搭配使用**

##### 4.2.14 startAt

设置动画开始时 某属性的值

```javascript
// mc从200运动到500处
TweenMax.to(mc, 2, {x:500, startAt:{x:200}});
```

##### 4.2.15 cycle

可以在stagger动画中设置属性组

```javascript
// 所有通过".box"选中的元素 都会循环执行下面的属性数组
myTween = TweenMax.staggerTo(".box", 1, {
    cycle: {
        //为目标循环设置一下属性数组
        backgroundColor: ["red", "green", "#00f"],
        //通过function返回属性数组
        y(index, target) {
            return (index + 1) * 20;
        }
    }
}, 0.5);
```

#### 4.3 动画事件

##### 4.3.1 onComplete

动画结束时触发的回调函数

```javascript
TweenMax.to('.box', 2, {
    x: 500,
    onComplete: function() {
        ...
    }
});
```

##### 4.3.2 onCompleteParams

传递给onComplete函数的参数数组

```javascript
let panel = document.getElementById("panel");
new TweenMax('.box', 3, {
    x: 500,
    onComplete: myFunction,
    // 传递动画对象本身 可以使用{self}
    onCompleteParams: ["{self}", "param2 value"],
});
function myFunction(param1, param2) {
    console.log(param1);
    panel.innerHTML = param2;
}
```

##### 4.3.3 onCompleteScope

传递给onComplete函数的作用域  **onCompleteScope: object**

##### 4.3.4 onReverseComplete

反向播放动画完成时执行此回调函数 如tween.reverse();

yoyo返回原点时不会触发onReverseComplete 因为yoyo只是把repeat反了个方向

```javascript
let tween = new TweenMax('.box', 3, {
    x: 500,
    onReverseComplete: function() {
    	panel.innerHTML = '返回完毕';
    }
});
let panel = document.getElementById("panel");
reverseBtn = document.getElementById("reverseBtn");
reverseBtn.onclick = function() {
    // 执行reverse时 会触发onReverseComplete函数
    tween.reverse();
}
```

##### 4.3.5 onReverseCompleteParams

传递给onReverseComplete函数的参数数组

如果想传递动画对象本身 可以使用{self}  **onReverseCompleteParams: ["{self}", "param2"]**

##### 4.3.6 onReverseCompleteScope

onReverseComplete的作用域 **onReverseCompleteScope: Object**

##### 4.3.7 onStart

动画开始执行时的回调函数 可能会多次执行 因为动画可以重复执行

##### 4.3.8 onStartParams、onStartScope

传递给onStart函数的参数数组(可以使用{self}来传递动画对象自身) 和作用域 

##### 4.3.9 onUpdate

当动画发生改变时(动画进行中的每一帧) 会**不停的触发此事件**

##### 4.3.10 onUpdateParams、onUpdateScope

传递给onUpdate函数的参数数组(可以使用{self}来传递动画对象自身) 和作用域 

##### 4.3.11 onOverwrite

```javascript
var myTween = TweenMax.to([".green",".orange"], 3, {
    x: 300,
    alpha: 0.2,
    onOverwrite:myFunction
})
var myTween2 = TweenMax.to([".orange",".grey"], 5, {
    x: 500,
    y: 100,
})

function myFunction(overwrittenTween,overwritingTween,target,overwrittenProperties){
    console.log(overwrittenTween); // 被覆盖的动画
    console.log(overwritingTween); // 覆盖的动画
    console.log(target); // 动画目标(只有"auto"模式才会传递此参数)
    // 一个属性数组 包含了被覆盖的动画属性(只有"auto"模式才会传递此参数) 例如：["x","y","opacity"]
    console.log(overwrittenProperties); 
}
```

##### 4.3.12 onRepeat

动画每次重复时 执行此函数

##### 4.3.13 onRepeatParams、onRepeatScope

传递给onRepeat函数的参数数组(可以使用{self}来传递动画对象自身) 和作用域 

##### 4.3.14 .eventCallback()

```javascript
let myAnimation = new TweenLite(mc, 1, {
    x:100,
    onComplete: myFunction,
    onCompleteParams: ["param1","param2"]
});
// 上下两种设置方式相同
myAnimation.eventCallback("onComplete", myFunction, ["param1","param2"]);
// 每个动画函数的每个回调类型只能有一个(一个onComplete, 一个onUpdate, 一个onStart) 新的会覆盖旧的
// 可通过以下方式删除定义过的动画
myAnimation.eventCallback("onUpdate", null);
```

#### 4.4 动画播放组件

一般情况下 会返回self 方便链式调用

##### 4.4.1 .play()

```javascript
// 1.动画开始播放时间 不传此参数代表从当前位置开始播放
// 2.如果true(默认值) 当播放头移动到from参数中定义的新位置 不会触发任何事件或回调
myAnimation.play(2, false); // false时不阻止期间的函数或事件
```

##### 4.4.2 .pause()

```javascript
// 跳转到第2秒并暂停动画 第二个参数为false时 不会抑制事件和函数触发
myAnimation.pause(2, false); // 不传参数只暂停
```

##### 4.4.3 .paused()

```javascript
let paused = myAnimation.paused(); // 获取暂停状态 该状态指示动画是否已暂停
myAnimation.paused(true); // 设置暂停状态(类似pause())
myAnimation.paused(!myAnimation.paused()); // 切换暂停/非暂停状态
```

##### 4.4.4 .restart()

重新开始动画/重头开始

```javascript
// 略去delay马上重新开始
myAnimation.restart();
 
// 包含delay重新开始 并且不抑制事件触发
myAnimation.restart(true, false);
```

##### 4.4.5 .resume()

```javascript
//恢复播放
myAnimation.resume();
 
//跳到第2秒并恢复播放
myAnimation.resume(2);
 
//跳到第2秒并恢复播放 并且不抑制事件触发
myAnimation.resume(2, false);
```

##### 4.4.6 .reverse()

```javascript
// 当前位置反向播放
myAnimation.reverse();
 
// 2秒位置反向播放
myAnimation.reverse(2);
 
// 2秒位置反向播放 不抑制事件触发
myAnimation.reverse(2, false);
 
// 动画末端反向播放
myAnimation.reverse(0);
  
// 动画末端往回1秒位置反向播放
myAnimation.reverse(-1);
 
// 切换方向(如前进方向 则反向播放 如反向 则向前播放)
if (myAnimation.reversed()) {
    myAnimation.play();
} else {
    myAnimation.reverse();
}
```

##### 4.4.7 .reversed()

```javascript
let rev = myAnimation.reversed(); // 获取反方向状态
myAnimation.reversed(true); // 设置反方向
myAnimation.reversed(!myAnimation.reversed()); // 切换方向
```

##### 4.4.8 .seek()

不改变状态度(播放、暂停、方向)的情况下 直接跳转到某个时间点

```javascript
// 跳转到第2秒
myAnimation.seek(2);

// 跳转到第2秒 并且不阻止期间的函数、事件
myAnimation.seek(2, false);
```

##### 4.4.9 .timeScale()

获取/设定动画速度 默认为1 0.5为慢速 2为快速

如果设置则返回此动画实例便于链式调用 如不设置则返回时间调节比例

#### 4.5 动画属性调整

一般情况下 返回self 方便链式调用

##### 4.5.1 .duration()

获取/设置动画持续时间

```javascript
var currentDuration = myAnimation.duration(); // 获取持续时间
myAnimation.duration(2); // 设置持续时间

// 链式调用例子
myAnimation.duration(2).repeat(3).timeScale(2).play(0.5);
```

##### 4.5.2 .totalDuration()

获取或设定全部重复的动画的持续时间

如一次动画持续时间为6秒 repeat3次 则totalDuration值为18秒

##### 4.5.3 .time()

获取/设置当前动画时间(当次动画时间)

##### 4.5.4 .totalTime()

获取/设置动画总时间(动画到目前为止总共执行时间)

##### 4.5.5 .progress()

获取/设置**单次动画的进程(0-1)** 即动画进行到什么程度

##### 4.5.6 .totalProgress()

获取/设置动画的总进程(0-1)

##### 4.5.7 .delay()

获取/设置动画的延迟时间

##### 4.5.8 .invalidate()

刷新任何内部记录的开始/结束值

重新启动动画而**不恢复到以前记录的任何起始值**

```javascript
var t = TweenLite.to(".green", 1, {x:"+=100"});

$("#restart").click(function(){
  t.restart(); // 记录起始值 会一直从记录的起始值触发动画
})

$("#invalidate").click(function(){
  t.invalidate(); // 清除记录的起始值
  t.restart(); // 由于清除了记录的起始值 会将动画执行结束的位置记录为每次restart的起始值
})
```

##### 4.5.9 .isActive()

检测动画是否处于活动状态

```javascript
btn.onclick = () => {
    // 当动画停止时 再reverse
    if (!tween.isActive()) {
        // 反向播放动画
        tween.reversed(!tween.reversed());
    }
};
```

##### 4.5.10 .updateTo()

```javascript
let myTween = TweenMax.to(".box", 3, {x:500, y:100 ,opacity:0.2, rotation:360})
resetBtn = document.getElementById("resetBtn");
resetBtn.onclick = function() {
    // 改变属性后动画时长重新计算3秒 即从上个动画结束后 再花费三秒来执行新的动画
    myTween.updateTo({css: {x:300, y:0, opacity:0.2, rotation:360}}, true);
}
```

##### 4.5.11 .startTime()

获取/设定动画在其父时间轴上的开始时间

##### 4.5.12 .endTime()

获取动画在父时间轴上的结束时间

```javascript
let tl = new TimelineLite(); // 创建新的时间轴
let tween = TweenMax.to(".box", 1, {x:100}); // 创建一个1秒钟的动画
tl.add(tween, 0.5); // 将动画插入时间轴0.5秒处
document.getElementById("endTime1").innerHTML=tween.endTime(); // 1.5秒结束
tween.timeScale(2); // 设置动画双倍速度
document.getElementById("endTime2").innerHTML=tween.endTime(); // 加速后1秒结束 0.5+1/2
```

##### 4.5.13 .repeat()

获取或者设定动画在第一次完成后的重复次数

##### 4.5.14 .repeatDelay()

获取或者设置每次重复动画之间的间隔时间(秒)

##### 4.5.15 .yoyo()

获取或设置补间动画的yoyo的状态
如需使用yoyo 还要设置repeat(动画重复次数)

#### 4.6 实例属性

##### 4.6.1 .data

可用于存储需要的数据

```javascript
myTween = new TweenMax('.box', 3, {
    x: 500,
});
myTween.data={data1:'value1',data2:'value2',}
console.log(myTween.data.data1);
```

##### 4.6.2 TweenLite.defaultEase

更改TweenLite默认的缓动方式 默认的缓动方式是Power1.easeOut

```javascript
TweenLite.defaultEase = Bounce.easeOut
new TweenLite('.box', 3, {
    x: 500,
});
```

其余实例属性见文档

<https://www.tweenmax.com.cn/api/tweenmax/TweenMax.ticker>

#### 4.7 实例方法

实例方法见文档



------



### 五、TimelineMax

可创建时间轴(timeline)作为动画或其他时间轴的容器 这使得整个动画控制和精确管理时间变得简单

**屏幕触发：**一般使用play() pause()来控制动画效果

**滚轮控制：**一般使用setTween来控制动画效果

```javascript
var tl = new TimelineLite();
tl.add(TweenLite.to(element, 1, {left:100})); // 将一个动画添加到时间轴
tl.add(TweenLite.to(element, 1, {top:50})); // 将一个动画添加到时间轴末端 即与前一个动画接续
tl.add(TweenLite.to(element, 1, {opacity:0})); // 将一个动画添加到时间轴末端 即与前一个动画接续
 
// 控制时间轴
tl.pause();
tl.resume();
tl.seek(1.5);
tl.reverse();
```

或者使用简单的to()方法和链式调用使其更加简洁

```javascript
let tl = new TimelineLite();
tl.to(element, 1, {left:100}).to(element, 1, {top:50}).to(element, 1, {opacity:0});
```

如此便可随意调整任何动画 而不必担心延迟时间会发生混乱 增加第一个动画的持续时间 一切会自动调整

#### 5.1 时间轴初始化及动画管理

基础使用示例

```javascript
// 创建一个重复3次 每次1秒间隔的时间轴 当时间轴结束时执行myFunction()
var tl = new TimelineMax({repeat: 3, repeatDelay: 1, onComplete: myFunction});
// 添加一个动画
tl.add(TweenLite.to(element, 1, {left: 200, top: 100}));
        
// 在时间轴末尾添加另一个动画
tl.add(TweenLite.to(element, 0.5, {opacity:0}));
 
// 通过.to()方法添加一个动画，添加于时间轴末尾后0.5秒处的
tl.to(element, 1, {rotation: 30}, "+=0.5");
         
// 反向播放时间轴
tl.reverse();
// 在三秒钟处插入一个标签
tl.addLabel("spin", 3);
// 在标签处插入一个动画
tl.add(new TweenLite(element, 2, {rotation: "+=360"}), "spin");
    
// 跳转到标签处开始播放
tl.play("spin");
// 在时间轴中插入另一个时间轴
var nested = new TimelineMax();
nested.to(element2, 1, {left: 200}));
tl.add(nested);
```

##### 5.1.1 TimelineMax()

创建一个时间轴实例

```javascript
let myTimeline = new TimelineMax();
// 添加动画至时间轴 时间轴长为6秒
myTimeline.add(TweenLite.to('.green', 6, {x: 600})); 
// 添加动画至时间轴绝对位置1秒处
myTimeline.add(TweenLite.to('.orange', 2, {x: 200}), 1); 
// 添加动画至时间轴末尾(6秒处) 时间轴长变成9秒
myTimeline.add(TweenLite.to('.grey', 3, {x: 300}));  
```

一般方法都会返回此时间轴实例 以便进行链式调用

##### 5.1.2 .add()

向时间轴添加动画、其他时间轴、回调函数或标签(或它们的数组)

```javascript
// 将一个动画添加到时间轴的末尾
tl.add(TweenLite.to(element, 2, {left: 100}));
 
// 将一个函数添加到1.5秒处
tl.add(func, 1.5); 
 
// 将一个标签添加至时间轴结束后2秒
tl.add("myLabel", "+=2");
 
// 将另一个时间轴添加至"myLabel"标签处
tl.add(otherTimeline, "myLabel"); 
 
// 将一个动画数组添加至标签"myLabel"后2秒处
tl.add([tween1, tween2, tween3], "myLabel+=2"); 
 
// 将一个动画数组添加至时间轴结尾后2秒处，每个动画都在前一个动画结束0.5秒后开始
tl.add([tween1, tween2, tween3], "+=2", "sequence", 0.5);
// 1.value 添加的动画、时间轴、回调函数或标签(或它们的数组)
// 2.position 添加的时间 以秒为单位表示绝对时间 += -=表示相对时间 例如+=2表示时间轴结束后2秒
//   还可以使用标签 例如"myLabel+=2"表示添加至myLabel后2秒处 标签不存在 则添加至末尾
// 3.align 排序方式 sequence：排队添加 即一个完成后再到下一个
//	 start：全部同时开始 并忽略delay	normal：全部同时开始 不忽略delay
// 4.stagger 序列时间间隔/帧 仅当value是数组时 需要设置 默认为0
//	 如为0.5秒 如align是normal 则第一个完成后再添加第二个 如align是start 则每隔0.5秒添加一个
```

##### 5.1.3 .addCallback()

在特定位置插入回调函数

```javascript
let panel = document.getElementById("panel");
let myFunction = function() {
  panel.innerHTML = '函数触发了！';
};

let tm = new TimelineMax();
tm.to(".box", 5, {x: 500})
  .addCallback(myFunction, "-=4");

console.log(tm.getChildren());
```

##### 5.1.4 .addLabel()

```javascript
let tl = new TimelineLite(); // 创建新的时间轴
// 在时间轴上加入动画
tl.to("#green", 1, {x: 750})
  // 在时间轴结束1秒后的位置 加入blueGreenSpin标签
  .addLabel("blueGreenSpin", "+=1")
  // 给blueGreenSpin标签添加动画
  .to("#blue", 2, {x:750, rotation:360}, "blueGreenSpin")
  // 在blueGreenSpin标签插入位置后0.5秒加入动画
  .to("#orange", 2, {x:750, rotation:360}, "blueGreenSpin+=0.5");
```

##### 5.1.5 .addPause()

在时间轴上添加一个暂停点

```javascript
// 在2秒处添加一个暂停点
timeline.addPause(2);
 
// 在"yourLabel"标签处添加一个暂停点
timeline.addPause("yourLabel");
 
// 在"yourLabel"标签3秒后添加一个暂停点 暂停时触发yourFunction函数
timeline.addPause("yourLabel+=3", yourFunction);

// 在时间轴4秒处添加暂停点 暂停时触发yourFunction函数 并将参数"param1"和"param2"以及作用域传递给该函数
timeline.addPause(4, yourFunction, ["param1", "param2"], this);
```

##### 5.1.6.remove()

从时间轴上移除一个动画、时间轴、函数或标签(或者他们的数组)

##### 5.1.7 .removeCallBack()

```javascript
// 移除3秒处函数
myTimeline.removeCallback(myFunction, 3);
// 移除标签处函数
myTimeline.removeCallback(myFunction, "myLabel");
// 移除全部myFunction函数
myTimeline.removeCallback(myFunction, null);
```

##### 5.1.8 .removeLabel()

从时间轴中删除标记 也可以使用remove()方法来移除

##### 5.1.9 .removePause()

删除通过TimelineMax.addPause() 添加到时间轴的暂停点

```javascript
let tl = new TimelineMax();
tl.to(obj, 1, {x:100})
  .addPause() // 在1秒处插入暂停
  .to(obj, 1, {opacity:0});

// 移除暂停
tl.removePause(1);
```

##### 5.1.10 .to()

添加一个动画到时间轴

```javascript
myTimeline.add(TweenLite.to(element, 1, {left:100, opacity:0.5}));
// 上下代码执行效果一致
myTimeline.to(element, 1, {left: 100, opacity: 0.5});
```

```javascript
tl.to(element, 1, {left: 100});  // 添加至时间轴末尾
tl.to(element, 1, {left: 100}, 2);  // 添加至2秒处(绝对位置)
tl.to(element, 1, {left: 100}, "+=2");  // 添加至时间轴末尾后2秒处(相对位置)
tl.to(element, 1, {left: 100}, "myLabel");  // 添加至标记处
tl.to(element, 1, {left: 100}, "myLabel+=2");  // 添加至标记后2秒处
```

##### 5.1.11 .from()

添加一个TweenLite.from() 动画到时间轴 相当于add(TweenLite.from(...))

```javascript
myTimeline.add(TweenLite.from(element, 1, {left: 100, opacity: 0.5}));
// 上下代码执行效果一致
myTimeline.from(element, 1, {left: 100, opacity: 0.5});
```

##### 5.1.12 .fromTo()

```javascript
tl.fromTo(element, 1, {left: 0}, {left: 100});  // 添加至时间轴末尾
tl.fromTo(element, 1, {left: 0}, {left: 100}, 2);  // 添加至2秒处(绝对位置)
tl.fromTo(element, 1, {left: 0}, {left: 100}, "+=2");  // 添加至时间轴末尾后2秒处(相对位置)
tl.fromTo(element, 1, {left: 0}, {left: 100}, "myLabel");  // 添加至标记处
tl.fromTo(element, 1, {left: 0}, {left: 100}, "myLabel+=2");  // 添加至标记后2秒处
```

##### 5.1.13 .staggerTo()

为一组目标设定相同的终点变化属性 但错开一定时间 创建成一个间隔均匀的动画序列

```javascript
let textFields = [tf1, tf2, tf3, tf4, tf5];
myTimeline.staggerTo(textFields, 1, {top: "+=150", ease: CubicIn.ease}, 0.2);
```

```javascript
.staggerTo(targets: Array, duration: Number, vars: Object, stagger: Number, position: *, onCompleteAll: Function, onCompleteAllParams: Array, onCompleteScope: *);
// targets: 动画的对象或数组
// duration: 每个动画持续的秒数/帧数
// vars: 动画参数(CSS属性、延迟、重复次数等)
// stagger: 每个动画的开始时间或间隔
// position: 插入排序动画的位置 默认"+=0"
// onCompleteAll: 整个动画完成时触发的函数
// onCompleteAllParams: 传递给onCompleteAll函数的参数
// onCompleteScope: onCompleteAll函数的作用域
```

虽然stagger限定了动画目标使用相同的属性(如x: 100, rotation: 90) 也可使用如下方式

```javascript
// 可以使用cycle来设定一个属性组
cycle: {x :[100, -100], rotation: [30,60,90]}
// 还可使用function关键词
x: function() {
    return Math.random() * 200;
}
```

##### 5.1.14 .staggerFrom()

参数与.staggerTo()基本一致

```javascript
let textFields = [tf1, tf2, tf3, tf4, tf5];
// 上下代码效果一致(一段文字段落按0.2秒间隔向上运动)
myTimeline.staggerFrom(textFields, 1, {top: "+=150"}, 0.2);
```

##### 5.1.15 .staggerFromTo()

```javascript
let textFields = [tf1, tf2, tf3, tf4, tf5];
// 上下代码效果一致
myTimeline.staggerFromTo(textFields, 1, {opacity: 1}, {opacity: 0}, 0.2);
```

##### 5.1.16 .set()

将零持续时间的动画添加到时间轴的末尾

```javascript
myTimeline.add(TweenLite.to(element, 0, {left: 100, opacity: 0.5, immediateRender: false}));
// 上下代码效果一致
myTimeline.set(element, {left: 100, opacity: 0.5});
```

其余用法可参考官方文档



------



### 其他

#### 1.dir属性

```html
<element dir="ltr|rtl">
```

ltr为默认从左向右 rtl为从右向左 在css中可使用**html[dir=rtl]**作选择器
