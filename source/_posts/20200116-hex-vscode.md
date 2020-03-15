---
title: 六角學院 Visual Studio Code 課程學習筆記
date: 2020-01-16 14:23:35
tags:
 - VSCode
categories:
 - 線上課程觀看筆記
---

本文為觀看六角學院「 [VSCode 開發功能大解密](https://courses.hexschool.com/courses/711502)」的紀錄。

<!-- more -->

## VSCode 是什麼

Visual Studio Code 是由微軟開發的開源文字編輯器，並包含很多強大的功能，輔助使用者編輯各種檔案。

> 官網： [Visual Studio Code](https://code.visualstudio.com/)

## 基本環境介紹

VSCode 內建 Emmet 等功能，也有許多快捷鍵，方便使用。

常用的指令例如:
* Ctrl+Shift+P, F1 ：顯示所有命令
* Alt+↑/↓ ：位移整行程式碼
* Ctrl+Enter ：在下方新增一行
* Ctrl+Shift+Enter ：在上方新增一行
* Ctrl+/ ：開關註解
* Alt+Z ：切換自動換行
* Ctrl+P ：搜尋檔案
* Ctrl+D ：找到相同的片段
* Shift+Alt+F ：排版
* Ctrl+\ ：分割視窗
* Ctrl+` ：開啟終端機

> 官方快捷鍵列表： [Keyboard shortcuts for Windows](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)
> Emmet snippests 列表： [Cheat Sheet](https://docs.emmet.io/cheat-sheet/)

## 設定

用 Ctrl+, 或左下角的齒輪處可以找到設定。
![設定選項](01.png)

可以用圖形介面或 JSON 格式改動設定；也分成全域或工作區域設定。
例如使用 Live server 的話，就會在工作區資料夾底下出現 .vscode/setting.json ，裡面會有阜號之類的設定。
![設定介面](03.png)

## 佈景主題

可以更改色彩佈景主題跟圖示佈景主題。
![佈景主題](02.png)

> Atom 的色彩佈景主題： [Atom One Dark Theme](https://marketplace.visualstudio.com/items?itemName=akamud.vscode-theme-onedark)
> 圖示佈景主題： [Material Icon Theme](https://marketplace.visualstudio.com/items?itemName=PKief.material-icon-theme)

## 套件

VSCode 中可以用套件做到的事情很多，例如觀看 PDF 、 CSV 等檔案類型，也能看 PTT 。
以下是一些實用的套件：
* [indent-rainbow](https://marketplace.visualstudio.com/items?itemName=oderwat.indent-rainbow)
  * 把縮排上色
* [Bracket Pair Colorizer 2](https://marketplace.visualstudio.com/items?itemName=CoenraadS.bracket-pair-colorizer-2)
  * 把括號上色
* [GitLens](https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens)
  * 提示程式碼上一次提交的時間和使用者
* [Code Spell Checker](https://marketplace.visualstudio.com/items?itemName=streetsidesoftware.code-spell-checker)
  * 標示錯字、可以建立字典
* [favorites](https://marketplace.visualstudio.com/items?itemName=howardzuo.vscode-favorites)
  * 收藏常用的檔案
* [Project Manager](https://marketplace.visualstudio.com/items?itemName=alefragnani.project-manager)
  * 建立常用專案的捷徑
* [Polacode-2019](https://marketplace.visualstudio.com/items?itemName=jeff-hykin.polacode-2019)
  * 將程式碼轉存為圖片
  * ![Polacode](04.png)

## 支援程式語言

VSCode 支援許多程式語言，例如 JavaScript 、 C++ 、 Python 等。通常包含 IntelliSence 和 Snippets 等功能；也就是能夠顯示相關註釋及快速完成程式碼。此外還有重新命名變數的功能（選取變數按 F2 ）；又例如 Formatting ，即排版程式碼的功能。

> 官方文件：[Language Support in Visual Studio Code](https://code.visualstudio.com/docs/languages/overview)

### JSDoc Support

使用者可以自訂 JavaScript 的文件。

```js
/**
 * 一些相關的說明
 * @param {Number} value1 這是一個數字參數
 * @param {String} value2 這是一個字串參數
 */
function testFunction(value1, value2){
    console.log(1);
}
```

![JSDoc Support](05.png)
