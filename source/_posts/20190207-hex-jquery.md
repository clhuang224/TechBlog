---
title: 六角學院 jQuery 課程筆記
date: 2019-02-07 22:57:14
tags:
 - jQuery
 - CSS
categories:
 - 線上課程觀看筆記
---

本文為觀看[用 jQuery 打造互動性網頁動畫效果](https://www.udemy.com/jquery-learning/)的紀錄。

<!-- more -->

## jQuery 入門

### 簡介

jQuery是由JS寫成，兼容許多瀏覽器的函式庫。

### 用Chorme測試與偵錯

* 可以用Chrome的console直接寫jquery測試；Chrome是用V8引擎，渲染JS很快。
* 可以觀察inline css的變化

### [安裝](http://jquery.com/)

* 下載壓縮版本，節省網頁空間。
> slim版本會少一些功能。

### 載入及使用

```html
<!--載入-->
<script type="text/javascript" src="位址"></script>
```

```js
//使用（等待 HTML DOM 都載入後才執行ready裡的程式碼）
$(document).ready(function() {程式碼});
```

### [jQuery插件](https://packagecontrol.io/packages/jQuery)

## 選擇器與事件

### [Click](https://api.jquery.com/click)、[Show、Hide、Toggle](https://api.jquery.com/category/effects/basics/)

> hide()和css("display","none")的差異是，hide()會記錄原本的值，使用show()會變回原本的屬性

<p class="codepen" data-height="216" data-theme-id="0" data-default-tab="js,result" data-user="chiaya710623" data-slug-hash="YBaqov" style="height: 216px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="JQ3.13~16 練習">
  <span>See the Pen <a href="https://codepen.io/chiaya710623/pen/YBaqov/">
  JQ3.13~16 練習</a> by Lynn Huang (<a href="https://codepen.io/chiaya710623">@chiaya710623</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

### [jQuery Quick API Reference](https://oscarotero.com/jquery/)

## 更多動畫效果

### [Sliding](https://api.jquery.com/category/effects/sliding/) 和 [Fading](https://api.jquery.com/category/effects/fading/)

<p class="codepen" data-height="247" data-theme-id="0" data-default-tab="js,result" data-user="chiaya710623" data-slug-hash="wNmQMb" style="height: 247px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="JQ 3.18~19 練習 slide fade">
  <span>See the Pen <a href="https://codepen.io/chiaya710623/pen/wNmQMb/">
  JQ 3.18~19 練習 slide fade</a> by Lynn Huang (<a href="https://codepen.io/chiaya710623">@chiaya710623</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

### [Class Attibute](https://api.jquery.com/category/manipulation/class-attribute/)、[CSS Transitions](https://www.w3schools.com/css/css3_transitions.asp)

<p class="codepen" data-height="228" data-theme-id="0" data-default-tab="html,result" data-user="chiaya710623" data-slug-hash="ZwxZOx" style="height: 228px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="JQuery 4.20練習">
  <span>See the Pen <a href="https://codepen.io/chiaya710623/pen/ZwxZOx/">
  JQuery 4.20練習</a> by Lynn Huang (<a href="https://codepen.io/chiaya710623">@chiaya710623</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

### 「overflow: hidden」+「transition」組合技

<p class="codepen" data-height="265" data-theme-id="0" data-default-tab="css,result" data-user="chiaya710623" data-slug-hash="jdzRXx" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="JQ 4.21練習">
  <span>See the Pen <a href="https://codepen.io/chiaya710623/pen/jdzRXx/">
  JQ 4.21練習</a> by Lynn Huang (<a href="https://codepen.io/chiaya710623">@chiaya710623</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

### 鏈式寫法

method可以相連，會連續產生效果。

### Chrome 開發人員工具

可以用chrome開發人員工具sources的部分幫助調整動畫效果。

## CSS + jQuery 的動畫效果

### [.preventDefalt()](https://www.w3schools.com/jquery/event_preventdefault.asp)

.preventDefalt()可以取消預設事件，例如改變瀏覽器大小、點擊、滾輪、送出表單等。

> 即使寫<a herf=""\>不會連結到別的畫面，還是有跳到id錨點或瀏覽器頂部的效果，所以可以利用preventDefalt一併取消。

### [.css()](https://www.w3schools.com/jquery/jquery_css.asp)

```js
$("p").css({"background-color": "yellow", "font-size": "200%"});
```

> 可以用「""」移除css()加上去的屬性

### 下拉式選單設計

* 透過toggle類型的method，切換選單顯示與否，也可以同時切換選單的樣式。
* 在同時有多個選單時，可以用「this」等selector指定作用的對象。
* 使用setTimeout可以延遲CSS改變的時間，但內部無法直接使用「this」，所以需要存在變數中。
* 可以利用「event.stopPropagation();」和選擇html，使點擊選單以外的地方時，會將選單收起來。

<p class="codepen" data-height="255" data-theme-id="0" data-default-tab="js,result" data-user="chiaya710623" data-slug-hash="NoOdEB" style="height: 255px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="jQuery 5.27 下拉式選單">
  <span>See the Pen <a href="https://codepen.io/chiaya710623/pen/NoOdEB/">
  jQuery 5.27 下拉式選單</a> by Lynn Huang (<a href="https://codepen.io/chiaya710623">@chiaya710623</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

### [.delay()](https://api.jquery.com/delay/)

.delay()能延遲後續的動畫效果。

* show()之類的要寫秒數
* removeClass等無法使用，可以用[setTimeout()](https://www.w3schools.com/jsref/met_win_settimeout.asp)

> 同時用toggleClass跟jQuery的動畫可能會有一些衝突

<p class="codepen" data-height="264" data-theme-id="0" data-default-tab="js,result" data-user="chiaya710623" data-slug-hash="omQGLd" style="height: 264px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="jQuery 5.28 delay練習">
  <span>See the Pen <a href="https://codepen.io/chiaya710623/pen/omQGLd/">
  jQuery 5.28 delay練習</a> by Lynn Huang (<a href="https://codepen.io/chiaya710623">@chiaya710623</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

### 字體切換功能：使用css()

<p class="codepen" data-height="276" data-theme-id="0" data-default-tab="js,result" data-user="chiaya710623" data-slug-hash="JxezGw" style="height: 276px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="5.29 字體切換功能">
  <span>See the Pen <a href="https://codepen.io/chiaya710623/pen/JxezGw/">
  5.29 字體切換功能</a> by Lynn Huang (<a href="https://codepen.io/chiaya710623">@chiaya710623</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

> 用range的數值調整字體大小

<p class="codepen" data-height="251" data-theme-id="0" data-default-tab="js,result" data-user="chiaya710623" data-slug-hash="ErOMXG" style="height: 251px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="用range調整字體大小">
  <span>See the Pen <a href="https://codepen.io/chiaya710623/pen/ErOMXG/">
  用range調整字體大小</a> by Lynn Huang (<a href="https://codepen.io/chiaya710623">@chiaya710623</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

### 可關閉廣告設計

<p class="codepen" data-height="235" data-theme-id="0" data-default-tab="js,result" data-user="chiaya710623" data-slug-hash="dawdze" style="height: 235px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="jQuerfy 5.30關閉廣告">
  <span>See the Pen <a href="https://codepen.io/chiaya710623/pen/dawdze/">
  jQuerfy 5.30關閉廣告</a> by Lynn Huang (<a href="https://codepen.io/chiaya710623">@chiaya710623</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

### [.stop()](https://api.jquery.com/stop/)

> 如果按很多下動畫，但只按一次stop()，就只會停止當下的動畫，而繼續跑後面的動畫；
> 使用.stop(true).jumpToEnd(true)可以直接跳到整個動畫最後而停止。

<p class="codepen" data-height="329" data-theme-id="0" data-default-tab="js,result" data-user="chiaya710623" data-slug-hash="daLOgR" style="height: 329px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="jQuery 5.31 .stop()">
  <span>See the Pen <a href="https://codepen.io/chiaya710623/pen/daLOgR/">
  jQuery 5.31 .stop()</a> by Lynn Huang (<a href="https://codepen.io/chiaya710623">@chiaya710623</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

### off-Canvas Menu

<p class="codepen" data-height="265" data-theme-id="0" data-default-tab="js,result" data-user="chiaya710623" data-slug-hash="RvXRGV" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="jQuery 5.32 用offCanvas製作選單">
  <span>See the Pen <a href="https://codepen.io/chiaya710623/pen/RvXRGV/">
  jQuery 5.32 用offCanvas製作選單</a> by Lynn Huang (<a href="https://codepen.io/chiaya710623">@chiaya710623</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

### [Animate.css](https://daneden.github.io/animate.css/)

* 元素含有animated和動畫名稱的類別時會發生動畫
* 利用事件加上類別名稱產生動畫
* 單純加上事件而不消除的話，會只能點擊一次，可以透過兩種方法解決：
  * 利用官方寫好的[function animateCss(element, animationName, callback)](https://github.com/daneden/animate.css)
  * 利用.one('webkitAnimationEnd oanimationend msAnimationEnd animationend', function(event) { /* ... */ });，設定動畫結束時移除類別

<p class="codepen" data-height="249" data-theme-id="0" data-default-tab="js,result" data-user="chiaya710623" data-slug-hash="PVMzVB" style="height: 249px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="jQuery 5.33 Animate.css">
  <span>See the Pen <a href="https://codepen.io/chiaya710623/pen/PVMzVB/">
  jQuery 5.33 Animate.css</a> by Lynn Huang (<a href="https://codepen.io/chiaya710623">@chiaya710623</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

## 操控網頁元素

### [Tree Traversal](https://api.jquery.com/category/traversing/tree-traversal/)

* this：我（觸發函式的元素）
* 「.~~()」※沒有參數
  * .offsetParent()：從長輩中找positioned（position為relative、absolute或fixed）且最近的一個人
* 「.~~(FILTER)」
  * .parents()：所有長輩
  * .parent()：父母
  * .siblings()：所有兄弟姊妹
  * .prevAll()：所有兄姊
  * .prev()：前一個兄姊
  * .next()：後一個弟妹
  * .nextAll()：所有弟妹
  * .children()：兒子女兒
  * .find() ：子孫後代們
  * .closest()：從我開始往長輩中找符合選擇器且最近的一個人
* 「.~~Until(不包含的人, FILTER)」
  * .parentsUntil()：所有長輩但不包含括號的人跟再往上的長輩
  * .prevUntil()：所有兄姊但不包含括號的人跟更大的兄姊
  * .nextUntil()：所有弟妹但不包含括號的人跟更小的弟妹

### 利用Tree Traversal設計選單

<p class="codepen" data-height="243" data-theme-id="0" data-default-tab="js,result" data-user="chiaya710623" data-slug-hash="PLYgWB" style="height: 243px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="jQuery 6.38 QA選單練習">
  <span>See the Pen <a href="https://codepen.io/chiaya710623/pen/PLYgWB/">
  jQuery 6.38 QA選單練習</a> by Lynn Huang (<a href="https://codepen.io/chiaya710623">@chiaya710623</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

### [DOM Insertion, Inside](https://api.jquery.com/category/manipulation/dom-insertion-inside/)

* .html()：取得html內容
* .html(程式碼)：以文字或function的回傳值放入html內容
* .text()：取得文字內容（xml也可以用）
* .text(程式碼)：以文字或function的回傳值放入文字

<p class="codepen" data-height="389" data-theme-id="0" data-default-tab="js,result" data-user="chiaya710623" data-slug-hash="ZPEMJz" style="height: 389px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="jQuery 6.40 DOM Insertion, Inside">
  <span>See the Pen <a href="https://codepen.io/chiaya710623/pen/ZPEMJz/">
  jQuery 6.40 DOM Insertion, Inside</a> by Lynn Huang (<a href="https://codepen.io/chiaya710623">@chiaya710623</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

### .on()

監聽特定區塊的事件，才能產生效果。

<p class="codepen" data-height="291" data-theme-id="0" data-default-tab="js,result" data-user="chiaya710623" data-slug-hash="VRYjdY" style="height: 291px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="jQuery 6.41 on()">
  <span>See the Pen <a href="https://codepen.io/chiaya710623/pen/VRYjdY/">
  jQuery 6.41 on()</a> by Lynn Huang (<a href="https://codepen.io/chiaya710623">@chiaya710623</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

## 常用小技巧

### [General Attributes](http://api.jquery.com/category/manipulation/general-attributes/)

.attr()：取得或放入屬性

### [DOM Removal](https://api.jquery.com/category/manipulation/dom-removal/)

.remove()：移除html標籤

> 真正從畫面中消失，而非隱藏，方便管理畫面中的資料

<p class="codepen" data-height="252" data-theme-id="0" data-default-tab="js,result" data-user="chiaya710623" data-slug-hash="rRabpX" style="height: 252px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="jQuery 7.45 remove()">
  <span>See the Pen <a href="https://codepen.io/chiaya710623/pen/rRabpX/">
  jQuery 7.45 remove()</a> by Lynn Huang (<a href="https://codepen.io/chiaya710623">@chiaya710623</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

### [.scrollTop()](https://api.jquery.com/scrollTop/)

<p class="codepen" data-height="249" data-theme-id="0" data-default-tab="js,result" data-user="chiaya710623" data-slug-hash="zbxXmG" style="height: 249px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="7.46 Top anchor滾動效果">
  <span>See the Pen <a href="https://codepen.io/chiaya710623/pen/zbxXmG/">
  7.46 Top anchor滾動效果</a> by Lynn Huang (<a href="https://codepen.io/chiaya710623">@chiaya710623</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

### [Font Awesome](https://fontawesome.com/)

> 用「[[attribute$=value]](https://www.w3schools.com/cssref/css_selectors.asp)」之類的選擇器，指定特定連結

<p class="codepen" data-height="282" data-theme-id="0" data-default-tab="js,result" data-user="chiaya710623" data-slug-hash="GeJRvg" style="height: 282px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="jQuery 7.47 Font Awesome ">
  <span>See the Pen <a href="https://codepen.io/chiaya710623/pen/GeJRvg/">
  jQuery 7.47 Font Awesome </a> by Lynn Huang (<a href="https://codepen.io/chiaya710623">@chiaya710623</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

## 第三方Plugin

### [Lightbox](https://lokeshdhakar.com/projects/lightbox2/)

* 商品圖等等經常是使用小圖，讓使用者可以看大圖。
* 透過官網裡提供的設定方法，或自己修改css，更改成想要的樣式；要暴力壓過[權重](http://cssspecificity.com/)可以使用!important。

<p class="codepen" data-height="265" data-theme-id="0" data-default-tab="js,result" data-user="chiaya710623" data-slug-hash="pYJebZ" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="jQuery 7.49~50 Lightbox">
  <span>See the Pen <a href="https://codepen.io/chiaya710623/pen/pYJebZ/">
  jQuery 7.49~50 Lightbox</a> by Lynn Huang (<a href="https://codepen.io/chiaya710623">@chiaya710623</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

### [Swiper](http://idangero.us/swiper/)

> 上線的網頁用min的版本比較節省資源

<p class="codepen" data-height="316" data-theme-id="0" data-default-tab="js,result" data-user="chiaya710623" data-slug-hash="pYjeya" style="height: 316px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="jQuery 8.51 Swiper">
  <span>See the Pen <a href="https://codepen.io/chiaya710623/pen/pYjeya/">
  jQuery 8.51 Swiper</a> by Lynn Huang (<a href="https://codepen.io/chiaya710623">@chiaya710623</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

### Lightbox + Swiper 組合技

* 引用jQuery的順序：jQuery本人→第三方插件→自己寫的jQuery
* 引用CSS的順序：Reset→自己寫的CSS→第三方插件
* 利用on:......使Swiper生成後，移除複本，避免被Lightbox計算到

<p class="codepen" data-height="265" data-theme-id="0" data-default-tab="js,result" data-user="chiaya710623" data-slug-hash="oVYXER" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="8.55 Lightbox + Swiper">
  <span>See the Pen <a href="https://codepen.io/chiaya710623/pen/oVYXER/">
  8.55 Lightbox + Swiper</a> by Lynn Huang (<a href="https://codepen.io/chiaya710623">@chiaya710623</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

## 如何更精進

### 類別管理撇步

一般的CSS類別名稱就直接取；jQuery中要用到的類別名稱可以加個jq-前綴。

## 證書

![證書](https://udemy-certificate.s3.amazonaws.com/image/UC-DL9XOIXW.jpg?l=null)
