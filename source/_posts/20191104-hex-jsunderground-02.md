---
title: JS 新手地下城 F2 「時鐘」 挑戰筆記
date: 2019-11-04 00:28:37
tags:
 - JavaScript
 - JS 新手地下城
categories:
 - 開發日記
---
> 本文為挑戰六角學院 JS 新手地下城 F2 「時鐘」的筆記。

## 作品

<a target="_blank" href="https://clhuang224.github.io/JavaScriptUnderground#02">Lynn 的 JS 新手地下城 作品列表 / 2F：時鐘</a>

## 關卡重點

* 刻出鐘面
* 刻出指針
* 讓指針轉動

<!-- more -->

## 問題解決流程

### 整體結構

拆解的方式如下：
* 背景
  * 時鐘（圓角矩形）
    * 鐘面（有邊線的圓）
      * 很多點
      * 很多星
      * 很多線
      * 很多字
    * 時針（白色矩形）
      * 一條線
    * 分針（橘色圓角矩形）
      * 一條線
      * 一個圈
    * 秒針
      * 五條線
      * 兩個圈

```html
<!-- 背景 -->
<main class="main">
        <!-- 時鐘 -->
        <div class="clock">
            <!-- 鐘面 -->
            <div class="plate"></div>
            <!-- 時針 -->
            <div class="hour-hand"><div class="dark"></div></div>
            <!-- 秒針 -->
            <div class="second-hand">
                <div class="green-circle"><div class="black-circle"></div></div>
                <div id="line1" class="line"></div>
                <div id="line2" class="line"></div>
                <div id="line3" class="line"></div>
                <div id="line4" class="line"></div>
                <div id="line5" class="line"></div>
            </div>
            <!-- 分針 -->
            <div class="minute-hand"><div class="line"></div><div class="circle"></div>
            </div>
        </div>
    </main>
```

### 時鐘

.clock 的功能除了當個圓角矩形以外，也是鐘面和三個指針用來定位的父元素。

```css
.clock {
    width: 350px;
    height: 350px;
    background-color: #3d5a45;
    border-radius: 72px;
    /* 讓鐘面置中 */
    display: flex;
    justify-content: center;
    align-items: center;
    /* 讓指針定位 */
    position: relative;
}
```

#### 鐘面

底的部分是一個有邊線的圈圈：

```css
.clock .plate {
    width: 310px;
    height: 310px;
    border: 3px solid #212f0b;
    background-color: #293B29;
    border-radius: 50%;
    position: relative;
}
```

鐘面上的點點、星星、線及文字，因為是持續旋轉的關係，用 JavaScript 產生（雖然也可以用 CSS 暴力製作）。

但在旋轉前要在 CSS 裡做好準備工作：
* 先利用絕對定位將元素放在 0 度的位置
* 設定正確的 transform-origin
  * 用 transform: rotate() 旋轉時，預設會以元素的中心點當軸心，但我需要以時鐘的圓心來旋轉

點點和線的部分比較容易：

```css
.clock .plate .dot {
    width: 2px;
    height: 2px;
    background-color: #fff;
    border-radius: 50%;
    position: absolute;
    top: 31px;
    left: 154px;
    transform-origin: 1px 124px;
}

.clock .plate .line {
    width: 1px;
    height: 24px;
    background-color: #ff7600;
    position: absolute;
    top: 20px;
    left: 154.5px;
    transform-origin: 0.5px 135px;
    font-family: Helvetica, sans-serif;
}
```

星星的部分，由於不規則，這一關的設計稿也只有提供各個部件的 SVG 或圖檔，所以無法直接抓規格。
我找到這篇文章：[運用 clip-path 的純 CSS 形狀變換](https://www.oxxostudio.tw/articles/201503/css-clip-path.html)，利用裡面提供的 [Clippy](https://bennettfeely.com/clippy/) 這個網站，拉出差不多的星星圖案。

```css

.clock .plate .star {
    width: 8px;
    height: 8px;
    background-color: #ff7600;
    /* 用 Clippy 拉出的圖形 */
    clip-path: polygon(50% 0, 66% 33%, 100% 50%, 66% 66%, 50% 100%, 33% 66%, 0 50%, 33% 33%);
    position: absolute;
    top: 28px;
    left: 151px;
    transform-origin: 4px 127px;
}
```

接著就可以用 JavaScript 一邊旋轉，一邊放入新的元素：

```js
// 找到鐘面元素
let plate = document.querySelector('.clock .plate');
// i 代表鐘面上有 72 個位置需要放東西
for (let i = 1; i <= 72; i++) {

    // 新增節點
    let node = document.createElement('div');

    // 旋轉 （一次要旋轉的量 * 第幾格）
    let degree = 360 / 72 * i;
    node.style.transform = `rotate(${degree}deg)`;

    // 線每六格產生一次
    if (i % 6 == 0) {
        node.classList.add('line');
    }
    // 星星每三格產生一次（六的倍數時會產生線，不會走進這個 else if）
    else if (i % 3 == 0) {
        node.classList.add('star');
    }
    // 剩下的部分都是點點
    else {
        node.classList.add('dot');
    }
    plate.appendChild(node);
}
```

再來還有文字的部分。
文字很明顯是跟線對齊，所以可以放在線的元素裡面；由於放線的時候，字會跟著線一起轉，所以要逆向轉回來。

```js
// 線每六格產生一次
    if (i % 6 == 0) {
        node.classList.add('line');
        // 數字會跟著線一起轉，所以要轉回來
        node.innerHTML = `
        <span class="pm" style="transform:rotate(${-degree}deg);">${i / 6 + 12}</span>
        <span class="am" style="transform:rotate(${-degree}deg);">${i / 6}</span>`;
    }
```

再利用絕對定位，把文字放到線的上下位置。

```css
.clock .plate .line .pm {
    width: 12px;
    height: 12px;
    display: flex;
    justify-content: center;
    align-items: center;
    color: #FFF;
    font-size: 10px;
    position: absolute;
    transform-origin: 6px 6px;
    left: -5.5px;
    top: -14px;
}

.clock .plate .line .am {
    width: 12px;
    height: 12px;
    display: flex;
    justify-content: center;
    align-items: center;
    color: #FFF;
    font-size: 10px;
    position: absolute;
    transform-origin: 6px 6px;
    left: -5.5px;
    bottom: -14px;
}
```

這樣鐘面的部分就完成了。

#### 指針

指針的部分也是先個別作出外型，然後設定好位置和軸心。
時針和分針的部分比較容易，只是內部包含一些小裝飾；其中時針的深色部分原本應該是透明的效果，但我讓時針在最底層，用與背景同樣的顏色，就看不出差異。

```css
.clock .hour-hand {
    width: 8px;
    height: 72px;
    background-color: #FFF;
    border-radius: 0 0 4px 4px;
    position: absolute;
    left: 171px;
    top: 107px;
    transform-origin: 4px 68px;
}

/* 時針的深色部分 */
.clock .hour-hand .dark {
    background-color: #293B29;
    width: 2px;
    height: 24px;
    margin: 3px;
}

.clock .minute-hand {
    width: 8px;
    height: 96px;
    border-radius: 4px;
    background-color: #ff7600;
    position: absolute;
    left: 171px;
    top: 83px;
    transform-origin: 4px 92px;
    display: flex;
    justify-content: center;
    align-content: flex-end;
    flex-wrap: wrap;
}

/* 分針的白線 */
.clock .minute-hand .line {
    width: 1px;
    height: 32px;
    background-color: #FFF;
}

/* 分針的白圈 */
.clock .minute-hand .circle {
    margin: 0px 2px 2px 2px;
    width: 4px;
    height: 4px;
    border-radius: 50%;
    background-color: #FFF;
}

```

秒針的長相就有點複雜，其中圓的部分還好，可以從 SVG 看出尺寸；但是 path 的部分想看懂的話，可能要作到天荒地老。

```xml
<g xmlns="http://www.w3.org/2000/svg" transform="translate(646.618 400) rotate(180)">
    <g class="a" transform="translate(636 274)">
        <circle class="d" cx="4" cy="4" r="4"/><circle class="e" cx="4" cy="4" r="3.5"/>
    </g>
    <g class="b" transform="translate(638 276)">
        <circle class="d" cx="2" cy="2" r="2"/><circle class="e" cx="2" cy="2" r="1.5"/>
    </g>
    <path class="c" d="M0,3.61V50l6,8L-6,75l6,7v40" transform="translate(640 278)"/>
</g>
```

於是我決定隨興製作（？），線的部分只要有接起來，長得像就好了。

```css
.clock .second-hand {
    width: 14px;
    height: 126px;
    position: absolute;
    left: 168px;
    top: 50px;
    transform-origin: 7px 125px;
}

.clock .second-hand .green-circle {
    width: 8px;
    height: 8px;
    border-radius: 50%;
    background-color: #b1ff00;
    display: flex;
    justify-content: center;
    align-items: center;
    position: absolute;
    left: 3px;
}

.clock .second-hand .green-circle .black-circle {
    width: 4px;
    height: 4px;
    border-radius: 50%;
    border: 1px solid #293b29;
}

.clock .second-hand .line {
    width: 2px;
    border-radius: 1px;
    background-color: #b1ff00;
    position: absolute;
    left: 6px;
    transform-origin: 1px 1px;
}

/* 以下部分是憑感覺 */

.clock .second-hand #line1 {
    height: 41px;
    top: 7px;
    border-radius: 0px 0px 1px 1px;
}

.clock .second-hand #line2 {
    height: 10px;
    top: 47px;
    transform: rotate(-45deg);
}

.clock .second-hand #line3 {
    height: 20px;
    top: 53px;
    transform: translateX(6px) rotate(45deg);
}

.clock .second-hand #line4 {
    height: 12px;
    top: 66px;
    transform: translateX(-7px) rotate(-45deg);
}

.clock .second-hand #line5 {
    height: 53px;
    top: 73px;
    border-radius: 1px 1px 0px 0px;
}
```

### 時間

最後才是處理時間的部分，只要抓到現在的時間，再計算角度，讓指針隨著時間旋轉即可（ CSS 部分已經設定過軸心）。
其中要注意的是大單位要加上小單位的量才是指針的正確位置（例如 11:30 時的時針應該在 11 和 12 之間）。

```js
let hourHand = document.querySelector('.clock .hour-hand');
let minuteHand = document.querySelector('.clock .minute-hand');
let secondHand = document.querySelector('.clock .second-hand');
setInterval(function () {
    let currentTime = new Date();
    // 指針指向的位置要加上小單位的量
    let second = currentTime.getSeconds();
    let minute = currentTime.getMinutes() + second / 60;
    let hour = currentTime.getHours() + minute / 60;
    hourHand.style.transform = `rotate(${(hour % 12) / 12 * 360}deg)`;
    minuteHand.style.transform = `rotate(${minute / 60 * 360}deg)`;
    secondHand.style.transform = `rotate(${second / 60 * 360}deg)`;
}, 1000);
```

完成。

![完成圖](01.png)
