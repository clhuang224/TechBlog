---
title: Finding the bus 技術選擇
tags:
  - React
  - Mantine
  - Map
  - Redux Toolkit
categories:
  - 開發日記
date: 2026-04-02 07:05
updated: 2026-04-02 21:45
---

最近終於把 [Finding the bus](https://github.com/clhuang224/bus) 做到一個算是完整的程度，一直在想要怎麼寫文，讓自己更方便回顧這個專案。

一開始我開這個專案的時候，有糾結要全部自己寫，還是用 AI agent 來幫忙。
因為相對來說，我對 Vue 比較熟，所以想說或許完全自己寫，可以更熟悉 React。

但寫到一個程度，我卡在規格到底要長什麼樣子，因為我參考了好幾個公車查詢 APP，每個操作流程都有一點點不同。中間又卡了一些別的事情，就把專案放了一陣子。

後來跟 AI 討論規格的取捨，順手就把整個專案推到可以算是做完的程度。

不過這篇文我想先寫自己一開始規劃的時候都在考慮哪些事情。

<!-- more -->

首先是題目的選擇。

我是回去看以前六角學院活動的題目有哪些，然後看來看去就選了公車查詢。  
（翻到計算機的題目，加上回顧了資料結構的作業，才先小小做了 [Calculator](https://github.com/clhuang224/calculator)。）  
也有旅遊或開票網站這種題目，但我覺得能表現的東西不太一樣。  
我去看了 [TDX](https://tdx.transportdata.tw/) 的一些 API，覺得公車查詢需要處理一些資料，能呈現在地圖上，又需要處理 RWD，很適合練手。  
而且我自己本來就常用公車 APP，比較不用花時間去思考相關的情境，就這點來說，開票網站的情境可能就比較複雜。  
（但現在想起來覺得或許也可以找時間來做開票網站，因為也想開發一些跟公共事務有關的應用。）

在框架的選擇上，就是刻意用 React，因為想更熟悉 React 的生態系。

狀態管理的部分，我稍微看過 [Valtio](https://github.com/pmndrs/valtio) 這個 library，看起來是可以很簡單地用 mutable 的方式來改變狀態，可能很適合放飛自我式的輕量開發，但因為我想要了解 React 比較主流的方式，所以選擇用 Redux Toolkit。

UI 框架的部分，是在 [Chakra](https://github.com/chakra-ui/chakra-ui) 跟 [Mantine](https://github.com/mantinedev/mantine) 之間做選擇。  
雖然 Chakra 的 star 數比較多，而且我喜歡它的名稱，感覺有些忍術在身上（？  
但是在視覺上，我覺得跟我想要的公車查詢的感覺有點距離。  
Mantine 的文件對我來說很順眼，而且有很直接可以用的 layout，我想把時間花在熟悉 React 上，所以就選了 Mantine。  
不過兩者在各種元件上都很齊全，公車查詢也用不到太多元件，大概最需要的是 Timeline（嗎），但兩個框架都有 Timeline，所以也不是差在元件多寡。  
我覺得很多東西都是要用用看才知道，但一次只能試一種，這次我先嘗試用 Mantine。

在地圖的部分，因為我在口罩地圖用過 [Leaflet](https://github.com/Leaflet/Leaflet) 了，所以我搜尋還有沒有其他的框架可以用，就找到 [MapLibre](https://github.com/maplibre/maplibre-gl-js)。  
其實以公車應用會用到的功能來說，兩個框架差不多，但是 MapLibre 的原生功能涵蓋更廣，感覺如果真的要做更花俏的事情，會很有趣。  

接著也因為以前用過 [OpenStreetMap](https://osm.tw/) 的圖磚，就特意找了一個不同的。  
我本來先隨便用了 [Esri](https://github.com/Esri)，是問 ChatGPT 的，但我看了一下 license，覺得不太理想。  
然後也看了一下 [OpenTopoMap](https://opentopomap.org/)，但感覺太像地理課本了，不太符合公車應用。  
接著選擇用 Carto 的 Positron，資料是基於 Open Street Map 的，地圖看起來很乾淨，上面也沒有太多的資訊，跟 Mantine 預設的感覺滿接近的。  
但我在寫文的時候重看了[授權](https://github.com/CartoDB/basemap-styles?tab=License-1-ov-file)，發現 Carto 的樣式設計是開放的，但直接拿官方的圖磚來公開使用不太適當，所以我換成 [OpenFreeMap](https://github.com/hyperknot/openfreemap) 的 Positron，外觀很接近，而且確定是可以免費使用。

開專案的時候，我是直接看 React v19 文件，選用 React Router v7 的 framework mode。  
（我第一次看 React 文件的時候應該是 v16 - v18 之間，但這種版本數字我實在是記不住。）  
除了看文件的描述，選擇跟自己的需求相符的方式以外，也沒有什麼特別的原因，就是覺得 side project 可以直接嘗試最新的東西，看看跟以前接觸的專案有什麼不一樣。

一開始開專案的時候，就有看到 `ssr: false` 的設定，但我執行起來一直看到 hydration 的錯誤。  
因為以前沒有做過 SSR 的專案，所以去查了 hydration，然後一直不知道問題出在哪裡。  
明明是 `ssr: false`，我不懂為什麼還會有錯誤，想說是不是不應該用一個很新又不熟的框架呢？  
我想既然要好好做一個 side project，就不要有任何 error 留在專案裡。  
然後我剛好看了一本 Nuxt 的書，用 Nuxt 的角度複習了相關的一些知識。  
我才突然想到好像要看一下是不是 browser extension 的影響，把「萌典」關掉，就沒有報錯了（忘記做基本的檢查了呢）。

到這邊才覺得，那大概就是照這些框架下去實作吧。  
然後才開始開發。

相關的過程會寫在後續的文章裡。
