---
title: PrePrompt 開發日記
tags:
  - JavaScript
categories:
  - 開發日記
date: 2026-03-16 23:00
---

醞釀已久，今天終於開發了 [PrePrompt](https://clhuang224.github.io/PrePrompt/) 的第一個版本！

<!-- more -->

打從去年正式開始大量使用 ChatGPT 整理文本，就時常覺得要花力氣過濾一些敏感資訊很麻煩。

因為有用 NAS 儲存資料，我可以用筆電跟手機編輯同一份文件。

所以原本我寫了一個 shell script，在筆電上把文件中的敏感字詞替換掉，把這些替換過的文件存在固定的資料夾裡，需要的時候再從手機去找這些文件來丟給 ChatGPT。

<script src="https://gist.github.com/clhuang224/aaf38d8f3caec8aaf44d4dfa5c5ede15.js"></script>

但問題是有時候我想貼的文章沒有處理過，筆電又不在身邊，就還是很麻煩。
而且 script 要輸入檔案路徑其實也有點囉唆，感覺如果有介面可以操作，又能在手機上直接使用是最好。

於是靈感在腦中放了一陣子之後，就決定來實作一下。

## 先看看這串 commit

從 commit history 應該大概可以看出來我做了哪些事情（對的，這串文字被 pre-prompted 過！）。

```
docs: update GitHub Pages link in usage instructions
_我的帳號_
_我的帳號_
committed
34 minutes ago
·
docs: expand agent guidance for project conventions
_我的帳號_
_我的帳號_
committed
45 minutes ago
·
style: unify border radius tokens
_我的帳號_
_我的帳號_
committed
1 hour ago
fix: use pointer cursor for mapping search clear button
_我的帳號_
_我的帳號_
committed
1 hour ago
fix: disable drag handles while searching mappings
_我的帳號_
_我的帳號_
committed
1 hour ago
feat: add mapping search and refine toolbar layout
_我的帳號_
_我的帳號_
committed
1 hour ago
docs: add localized screenshots for the README files
_我的帳號_
_我的帳號_
committed
2 hours ago
·
feat: add i18n support for the UI and README
_我的帳號_
_我的帳號_
committed
2 hours ago
·
docs: update project structure section in README
_我的帳號_
_我的帳號_
committed
3 hours ago
·
refactor: split frontend code into modular JS and CSS files
_我的帳號_
_我的帳號_
committed
3 hours ago
feat: implement drag-and-drop functionality for mappings
_我的帳號_
_我的帳號_
committed
4 hours ago
docs: streamline README and link the original shell script gist
_我的帳號_
_我的帳號_
committed
4 hours ago
·
feat: add favicon to the project
_我的帳號_
_我的帳號_
committed
5 hours ago
·
feat: add click-to-copy output and refresh README
_我的帳號_
_我的帳號_
committed
5 hours ago
·
docs: update readme
_我的帳號_
_我的帳號_
committed
5 hours ago
·
feat: initialize default mappings and input text if not present
_我的帳號_
_我的帳號_
committed
5 hours ago
style: refresh the UI with a Quasar-inspired layout and mobile fixes
_我的帳號_
_我的帳號_
committed
5 hours ago
feat: add mapping import/export with overwrite warning and toast feedback
_我的帳號_
_我的帳號_
committed
5 hours ago
refactor: extract inline CSS and JavaScript into separate files
_我的帳號_
_我的帳號_
committed
5 hours ago
feat: add initial PrePrompt web app and project documentation
_我的帳號_
_我的帳號_
committed
6 hours ago
·
```

## 開發過程

### 核心功能

一開始我的想法很簡單，就是把 script 丟給 Codex（GPT-5.4）。
我的指示大概是這樣：

1. 寫一個 HTML 檔（不需要分檔）
2. 用 input 輸入替換前後的字詞
3. 用 textarea 輸入原本文章跟顯示結果
4. 可以新增跟刪除詞組
5. 每個詞組可以用 checkbox 啟用或停用
6. 用 localStorage 儲存詞組設定

於是我得到了這樣一個畫面：

![初代畫面](01.png)

其實，這樣就已經可以用了！

我稍微改了一些文字跟排版，並讓他幫我把原文也存進 localStorage（發現這樣比較符合使用者習慣），檔案大概 150 行：

```html
<!DOCTYPE html>
<html lang="zh-TW">
<head>
  <meta charset="UTF-8">
  <title>PrePrompt</title>
  <style>
    body { font-family: sans-serif; margin: 2em; }
    textarea { width: 100%; min-height: 100px; margin-bottom: 1em; }
    .mapping-row { display: flex; align-items: center; margin-bottom: 0.5em; }
    .mapping-row input[type="text"] { width: 120px; margin-right: 0.5em; }
    .mapping-row input[type="checkbox"] { margin-right: 0.5em; }
    .mapping-row button { margin-left: 0.5em; }
    #mappings { margin-bottom: 1em; }
  </style>
</head>
<body>
  <h2>PrePrompt</h2>
  <div>
    <label>原始文章：</label>
    <textarea id="inputText" placeholder="請貼上文章"></textarea>
  </div>
  <div>
    <label>結果：</label>
    <textarea id="outputText" readonly></textarea>
  </div>
  <div id="mappings"></div>
  <button id="addMapping">新增</button>
  <script>
    const inputText = document.getElementById('inputText');
    const outputText = document.getElementById('outputText');
    const mappingsDiv = document.getElementById('mappings');
    const addMappingBtn = document.getElementById('addMapping');
    const STORAGE_KEY = 'word_mappings_v1';

    function loadMappings() {
      try {
        return JSON.parse(localStorage.getItem(STORAGE_KEY)) || [];
      } catch {
        return [];
      }
    }

    function saveMappings(mappings) {
      localStorage.setItem(STORAGE_KEY, JSON.stringify(mappings));
    }

    function renderMappings(mappings) {
      mappingsDiv.innerHTML = '';
      mappings.forEach((map, idx) => {
        const row = document.createElement('div');
        row.className = 'mapping-row';

        const enable = document.createElement('input');
        enable.type = 'checkbox';
        enable.checked = map.enabled !== false; // 預設 true
        enable.title = '啟用';
        enable.addEventListener('change', () => {
          mappings[idx].enabled = enable.checked;
          saveMappings(mappings);
          updateOutput();
        });

        const from = document.createElement('input');
        from.type = 'text';
        from.placeholder = '原字詞';
        from.value = map.from;
        from.addEventListener('input', () => {
          mappings[idx].from = from.value;
          saveMappings(mappings);
          updateOutput();
        });

        const to = document.createElement('input');
        to.type = 'text';
        to.placeholder = '新字詞';
        to.value = map.to;
        to.addEventListener('input', () => {
          mappings[idx].to = to.value;
          saveMappings(mappings);
          updateOutput();
        });

        const del = document.createElement('button');
        del.textContent = '刪除';
        del.addEventListener('click', () => {
          mappings.splice(idx, 1);
          saveMappings(mappings);
          renderMappings(mappings);
          updateOutput();
        });

        row.appendChild(enable);
        row.appendChild(from);
        row.appendChild(to);
        row.appendChild(del);
        mappingsDiv.appendChild(row);
      });
    }

    function updateOutput() {
      const mappings = loadMappings();
      let text = inputText.value;
      mappings.forEach(map => {
        if (map.enabled !== false && map.from) {
          // 新字詞自動加上 _ 前後綴
          const toWord = map.to ? `_${map.to}_` : '';
          const pattern = new RegExp(escapeRegExp(map.from), 'g');
          text = text.replace(pattern, toWord);
        }
      });
      outputText.value = text;
    }

    function escapeRegExp(str) {
      return str.replace(/[.*+?^${}()|[\]\\]/g, '\\$&');
    }

    function addMapping() {
      const mappings = loadMappings();
      mappings.push({ from: '', to: '', enabled: true });
      saveMappings(mappings);
      renderMappings(mappings);
    }

    // 原始文章 localStorage
    const INPUT_STORAGE_KEY = 'input_text_v1';
    function saveInputText(text) {
      localStorage.setItem(INPUT_STORAGE_KEY, text);
    }
    function loadInputText() {
      return localStorage.getItem(INPUT_STORAGE_KEY) || '';
    }

    inputText.value = loadInputText();
    inputText.addEventListener('input', () => {
      saveInputText(inputText.value);
      updateOutput();
    });
    addMappingBtn.addEventListener('click', addMapping);

    // 首次載入
    renderMappings(loadMappings());
    updateOutput();

    // 監控 localStorage 變動（多分頁同步）
    window.addEventListener('storage', () => {
      inputText.value = loadInputText();
      renderMappings(loadMappings());
      updateOutput();
    });
  </script>
</body>
</html>
```

在 AI coding 的時代，開發時寫文件是很必要的，所以我就先寫了一個 README，一方面記錄一下我想要做的 features，另一方面給 AI 一些規範。

大致上是這樣：

```md
# PrePrompt

將文字貼進 LLM 前，用 PrePrompt 替換敏感資訊。

PrePrompt 是一個為個人工作流程設計的小工具，方便在把文章貼給 LLM 之前，先用自訂詞組替換掉敏感資料。

## 使用說明

1. 在「原始文章」輸入框貼上文章。
2. 點選「新增」，輸入「原字詞」與「新字詞」。
3. 點選核取方塊啟用或停用替代規則，點選「刪除」可移除不需要的字詞。
4. 結果會依照替代規則，顯示在「結果」輸入框（唯讀）。

> 新字詞會統一加上 `_` 前後綴，以避免與原字詞混淆。

## 資料儲存

原始文章和替代詞組會儲存在瀏覽器的 LocalStorage 中，不會送到伺服器。  
你可以在瀏覽器的開發者工具中查看或清除這些資料。

## 未來方向

### 待整理

- 檢查並整理 vibe coding 產生的程式碼
- 分檔

### 功能擴充

- 支援正則表達式
- 大小寫判斷
- 斷詞問題
- 替換規則優先順序問題
- 輸入 / 輸出 JSON
- 美美的介面
- 自動偵測敏感資訊

```

緊接著我就覺得，其實這些待辦事項，現在就可以做（？

### 匯入匯出

緊接著我就新增了匯入匯出功能，讓使用者可以把設定備份起來或是跨裝置使用。

會先加這個功能是因為，如果要在筆電跟手機通用的話，這個功能是不能少的。

以我用 NAS 私有雲端儲存資料的方式來說，匯出設定後丟到 NAS 裡，手機再從 NAS 下載設定檔就可以了。

手機修改之後再覆蓋 NAS 的檔案就萬無一失ˊˇˋ

### Toast 提示

Codex 產生的設計，在我按匯入匯出之後，會直接把「已匯出 5 個詞組」這段文字寫在按鈕後面，讓我覺得很幽默（？

於是我讓他幫我改成 toast 的提示，比較符合一般的設計。

讓 AI agent 寫這些樣式、按鈕等等，如果沒有要追求完美，好像沒有什麼大問題，但其實寫到這邊我的完美主義稍微有點在醞釀發作⋯⋯

### 美化介面

結果還是忍不住開始處理畫面。

本來我做這個工具的重點不是在樣式，但我發現考慮到在手機上要能使用，其實排版還是有點重要。

在美觀上我覺得還算無所謂，我直接跟 Codex 說幫我弄一個 Quasar 風格的 UI，他就弄好了（？

其實我不知道 Quasar 風格是指什麼（？），不過看起來還不錯。

![Quasar 風格](02.png)

### 預設文字

弄著弄著 PrePrompt 愈來愈像一個真正的產品了。

於是我開始想，一般人使用上好像會需要預設文字作為範例，能夠直接理解要怎麼使用。

所以我就把我拿來做專案截圖的文字直接當成預設文字了ˊˇˋ

```js
defaults: {
    inputText: '竹筍公主最喜歡吃北門口肉圓，配上東泉辣椒醬。',
    mappings: [
      { from: '竹筍公主', to: '我朋友', enabled: true },
      { from: '東泉辣椒醬', to: '台中神醬', enabled: true },
      { from: '北門口肉圓', to: '彰化第一肉圓', enabled: false },
      { from: '', to: '', enabled: true },
    ],
  }
```

到目前為止功能應該已經很完美了！

應該可以休息了？

### 一鍵複製

不過想到大部分這種應用，在結果的地方都可以左鍵複製，所以我又加上去了⋯⋯

### 再次整理 Readme

忽然想到我把整個 shell script 貼在 README 其實有點太長，所以我把 script 丟到 gist 上，然後在 README 裡面放一個連結，這樣看起來會比較乾淨，而且專案的質感好像有提升（？

### 拖曳排序

在第一代的 README 有提到，替換規則有順序的問題。

雖然我一時間還沒有打算處理這件事，但可以讓使用者用拖曳的方式移動詞組的順序，這樣也是一個暫代的方法。

另外在 UX 上，同類型的字詞可能會想排在一起，這也是必須增加拖曳排序功能的原因。

一開始我想說，這個專案不要太複雜，每個功能都不大，直接刻功能就好。

但拖曳的這個行為，Codex 可能需要更多的提示跟想法，而且 js 跟 css 會變得比較複雜。

雖然不是不能手刻，但這並不是專案的重點，所以我決定用 SortableJS 這個 library 來處理。

這也是專案唯一依賴的三方 library。

### 分檔

到這裡為止，專案就變得有一點冗長了。

依據之前練手寫 [calculator](https://github.com/clhuang224/calculator) 的做法，本來我只想簡單分成 consts 、 functions 跟 main。

但因為 PrePrompt 的功能比較多，所以更恰當的做法是依據功能來模組化，像是把 mapping 的相關功能放在一個檔案，localStorage 的功能放在一個檔案，UI 相關的功能放在一個檔案。

而 CSS 的部分，也應該分成 base 、 layout 和 components 這三個檔案，這樣比較清楚。

在命名的部分，Codex 在 css 裡寫了 .hero 這樣的名稱，這不是我本來的習慣，但我暫時也沒有想處理這種細節，就先放置不管。

我著重的是每個模組是不是有依照職責被分開，確認符合我的方向之後，也把資料夾結構加進 README，這樣以後比較方便依循。

### 競品？

「這樣應該已經做好了吧？」正當 Lynn 這麼心想的時候，忽然宇宙有一個力量告訴她，好像應該去看看這個工具是不是已經有人做過了？

於是 Lynn 在 GitHub 上搜尋了「Prompt Anonymize」，發現的確有幾個相關的專案。

其中一個 Star 數最高的是 Svelte 應用，其它好幾個是 Python 的工具。

共通點是這些工具都有依賴 NER（Named Entity Recognition，命名實體識別）的功能，來自動偵測敏感資訊。

在我的未來規劃裡面，的確有希望可以自動偵測敏感資訊，但這樣說起來，其實 PrePrompt 跟 NER-based 的工具就有很關鍵的差別。

因為我的工具重要的是，依照使用者的需求，手動維護詞組配對，跟搜尋到的工具是不同的。

雖然替換詞彙是非常基礎的功能，但對於非技術圈的人來說，這樣的工具可能是有符合需求的。

Lynn 是這麼相信的，嗯嗯。

### i18n

考慮到這個工具可能真的有一些用途，於是我決定把介面跟 README 改成中英日三語。

主要是想改 README，因為應用本身沒有什麼文字。

確認分檔沒什麼問題，翻譯的部分我就沒有深究。

在這部分有一個未來可以嘗試的地方是，我可以在 Crowdin 上開一個專案來處理這些翻譯。

想像中應該不會太複雜，只是覺得可以累積一下這個實作的經驗。

另外，看到日文翻譯，就不小心笑出來：

```js
defaults: {
    inputText: 'たけのこ姫は東泉チリソースをつけた北門口肉圓が大好きです。',
    mappings: [
      { from: 'たけのこ姫', to: '友だち', enabled: true },
      { from: '東泉チリソース', to: '台中の魔法ソース', enabled: true },
      { from: '北門口肉圓', to: '彰化名物の肉圓', enabled: false },
      { from: '', to: '', enabled: true },
    ],
  }
```

### 搜尋詞組

隨著詞組越來越多，搜尋詞組的功能就變得很重要了。

無論是在電腦還是手機上，一眼看過去其實大概同時只能顯示 2-5 個詞組，但我自己使用的經驗是可能會用到 20-30 個詞組，所以搜尋的功能很重要。

這部分並不是太複雜，所以讓 Codex 處理一下就好了。

### 統一 Style

最後我還是忍不住很想處理一下 Style 的規範，因為 Codex 用了一些我不常用的語法（`min()` 之類的）。

雖然查了一下 caniuse，並不是不能用，但總覺得在排版上有點亂。

所以我寫了一些規則，讓 Codex 重新掃過 CSS，畫面看起來就好多了。

### AI Agent 規範

最後我讓 Codex 把專案架構跟規範寫成 AGENTS.md，維護會比較方便。

## 後續規劃

在專案的 README 裡面，我已經列出了一些未來想要做的功能，像是支援正則表達式、大小寫判斷、斷詞問題、替換規則優先順序問題、自動偵測敏感資訊等等。

另外還有想到，也許可以做反向的功能，就是把已經被替換過的文章，還原回原本的樣子。

在專案的虛華（？）程度上，覺得或許能變成一個 Chrome extension 或 APP。

不過其實以我自己的需求來說，目前的功能已經夠用了。

而且我覺得做簡單一點，比較能讓人覺得安心，畢竟專案變複雜的話，就會開始擔心資料有沒有被存到什麼地方去。

不過我想可能還是可以稍微觀察一下類似的工具，看看有沒有什麼可以優化的地方。

## 完！
