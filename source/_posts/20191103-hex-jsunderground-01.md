---
title: JS 新手地下城 F1 「九九乘法表」 挑戰筆記
date: 2019-11-03 15:26:24
tags:
 - JavaScript
 - JS 新手地下城
categories:
 - 開發日記
---

> 本文為挑戰六角學院 JS 新手地下城 F1 「九九乘法表」的筆記。

## 作品

<a target="_blank" href="https://clhuang224.github.io/JavaScriptUnderground#01">Lynn 的 JS 新手地下城 作品列表 / 1F：九九乘法表</a>

## 關卡重點

* 刻出標題區塊
* 刻出白色區塊
* 外迴圈跑出所有白色區塊
* 內迴圈放入每個白色區塊的內容
* RWD

<!-- more -->

## 問題解決流程

### main 和 footer

我的習慣是先整理大區塊，因此先把 main 和 footer 的部分寫好。

```html
<main class="main">
</main>
<footer class="footer">
    <span class="copyright">Copyright © 2019 HexSchool. All rights reserved.</span>
</footer>
```

```css
.main {
    max-width: 1280px;          /* 設計稿的尺寸 */
    min-width: 375px;           /* 讓視窗縮到最小也不會扁到比 375px 還小 */
    background-color: #F0F0F0;
    padding: 60px 70px;         /* 一次從外設定 padding 比較符合設計稿的長相 */
    display: flex;
    flex-wrap: wrap;            /* 讓白色框框可以換行 */
    justify-content: center;    /* 讓白色框框垂直置中 */
    width: 100%;                /* 用了 flex ，寬度就不會撐滿，所以要設定 */
}

.footer {
    max-width: 1280px;
    min-width: 375px;
    background-color: #2EB738;
    padding: 8px 85px;          /* 讓文字和白色框框最右邊對齊 */
    display: flex;
    justify-content: flex-end;
    width: 100%;
}

.footer .copyright {
    color: #F0F0F0;
    font-size: 16px;
    line-height: 24px;
}
```

### 標題區塊

標題區塊從上到下有邊框、文字、邊框三個區塊，這三個區塊裡面又有一些小區塊。
為了排版方便，整個標題區塊和白色框框的大小一致。

```html
<div class="title">
    <div class="border">
        <span class="cross">×</span>
        <hr class="line">
        <span class="cross">×</span>
    </div>
    <h1 class="chinese">九九乘法表<span class="english">MULTIPLICATION CHART</span></h1>
    <div class="border">
        <!-- 同上 -->
    </div>
</div>
```

```css
.main .title {
    display: flex;
    flex-direction: column;
    align-items: center;            /* 垂直置中 */
    justify-content: space-between; /* 讓上下邊框撐到最開 */
    height: 366px;
    width: 350px;
    margin: 20px 15px;
}

.main .title .border {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 28px;
}

.main .title .border .cross {
    font-size: 24px;
    line-height: 24px;
    padding: 0;
    margin: 0;
}

.main .title .border .line {
    width: 280px;
    border: #2EB738 1px solid;
    margin: 0px 21px;
    padding: 0;
}

.main .title .chinese {
    font-family: 'Noto Sans TC', sans-serif;
    font-size: 56px;
    line-height: 78px;
    display: flex;
    flex-direction: column;
}

.main .title .english {
    font-size: 24px;
    line-height: 28px;
}
```

### 白色區塊

白色區塊的重點是弧形邊框、陰影和內外的間距。

```css
.main .card {
    width: 350px;
    height: 366px;
    border-radius: 100px 0 30px 0;  /* 弧形邊框 */
    background-color: #fff;
    box-shadow: #D8D8D8 0 3px 10px; /* 陰影 */
    margin: 20px 15px;              /* 外間距 */
    padding: 60px 40px;             /* 內間距 */
}
```

### 外迴圈

有了白色框框的樣式之後，就用 JavaScript 重複放入 main 中。
在 main 裡面已經有標題區塊，所以可以用 appendChild 的方式放在標題區塊後面。

```js
let main = document.querySelector('.main');

// i : 被乘數（左邊的數字）
// 用 2~9 跟設計稿相符，增加可讀性
for(let i=2;i<=9;i++)
{
    // 新增白色區塊節點，class 為 card
    let card = document.createElement('div');
    card.classList.add('card');

    // 在白色區塊裡放入 2~9 的數字標題
    card.innerHTML = `<div class="number">${i}</div>`;

    // 將白色區塊放入 main 中
    main.appendChild(card);
}
```

### 內迴圈

外迴圈中已經放入每一個白色框框的數字標題，接下來再在內迴圈放入乘以 1~9 的式子。

```js
// i : 被乘數（左邊的數字）
for(let i=2;i<=9;i++)
{
    // 新增白色框框節點
    let card = document.createElement('div');
    card.classList.add('card');

    // 放入數字標題
    card.innerHTML = `<div class="number">${i}</div>`;

    //// 內迴圈
    // j : 乘數（右邊的數字）
    for(let j=1; j<=9; j++)
    {
        // 新增式子節點
        let formula = document.createElement('div');
        formula.classList.add('formula');
        // 文字內容為 被乘數 × 乘數 ＝ 乘積
        formula.textContent = `${i} × ${j} ＝ ${i*j}`;
        card.appendChild(formula);
    }
    main.appendChild(card);
}
```

排版白色框框裡的內容。
重點是左右的式子要對齊，所以右上角的三個式子的高度要和數字標題一樣，且區塊的垂直距離要平均。
另外數字標題的白色邊框，可以用 text-stroke ；但是在 Chrome 縮放畫面時會破圖，所以我改用陰影疊加的方式。

```css
.main .card {
    align-content: space-between;      /* 水平距離平均且貼壁 */
    justify-content: space-between;    /* 垂直距離平均且貼壁 */
    flex-direction: column;
    flex-wrap: wrap;                   /* 讓式子可以換行到右欄 */
}
.main .card .number {
    width: 120px;
    font-weight: bold;
    font-size: 128px;
    line-height: 123px;                /* 和右側三個式子的高度相同 */
    text-shadow: 1.5px 1.5px #FFF,     /* 白色邊框 */
                 4px 3px #F0F0F0;      /* 灰色陰影 */
    display: flex;
    justify-content: center;
    align-items: center;
}

.main .card .formula {
    width: 120px;
    height: 33px;
    display: flex;
    justify-content: flex-start;
    align-items: center;
    font-size: 24px;
    line-height: 24px;
    margin: 4px 0px;
}
```

### RWD

最後隨喜好做一些 RWD 。
我是讓白色區塊因為視窗不夠大而換行時，讓 main 縮小，以免白色區塊已經換行了，灰色部分卻空一大塊。

```css

@media only screen and (max-width: 1279px) {
    /* main 和 footer 縮成兩欄大小 */
    .main, .footer {
        max-width: 915px;
    }
}

@media only screen and (max-width: 914px) {
    /* main 和 footer 縮成一欄大小 */
    .main, .footer {
        max-width: 535px;
    }
    .footer {
        justify-content: center;
    }
}

@media only screen and (max-width: 534px) {
    /* 減少一些內外距，騰出空間 */
    .main, .footer {
        max-width: auto;
        padding: 8px 0px;
    }
    .main .card {
        margin: 20px 0px;
        padding: 60px 20px;
        width: 346px;
    }
    .footer .copyright {
        font-size: 12px;
    }
}
```

這樣就完成了！
![完成圖](01.png)
