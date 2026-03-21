---
title: lynns.me 自訂域名
tags:
  - Infra
categories:
  - 開發日記
date: 2026-03-20 08:32
updated: 2026-03-22 01:26:20
---

最近忽然想起剛念資工系的時候，看到有同學買自己的域名架站。

然後想到兩三年前我也有買一個網域，但後來就忘記它的存在。

本來想說那就重新挑一個吧？

但我想到的選項都有點價格，畢竟 Lynn 這個名字雖然不算常見，但也不算少見。

然後我才發現原來兩年前買的網域剛好只剩十幾天到期（原來我那時候買了兩年），所以就續訂了。

<!-- more -->

---

我連了一下 `https://lynns.me`，發現就只有一個剛開好的 Vue 專案畫面，還有 favicon（就是現在用的頭像）。

回想起來那時候我好像是花了非常多時間畫 Figma mockup，想說做一個前後端分離的個人網站。

畫好首頁跟各種元件之後，我開了 Vue 的新專案，部署到 Azure，把域名跟憑證設定好，然後就沒有然後了（？

不過既然我現在想起來域名這件事，就乾脆來思考要怎麼運用這個東西吧？

我的想法是這樣：

- 網誌：`blog.lynns.me`
- projects
  - [Finding the bus](https://github.com/clhuang224/bus)：`bus.lynns.me`
  - [PrePrompt](https://github.com/clhuang224/pre-prompt)：`pre-prompt.lynns.me`
  - [Queener](https://github.com/clhuang224/queener)：`queener.lynns.me`
  - [Calculator](https://github.com/clhuang224/calculator)：`calculator.lynns.me`
  - [Pomodoro](https://github.com/clhuang224/Pomodoro_2020)：`pomodoro.lynns.me`
- redirect
  - GitHub：`github.lynns.me`
  - LinkedIn：`linkedin.lynns.me`
  - Crowdin：`crowdin.lynns.me`
  - Soundcloud：`soundcloud.lynns.me`

如果做了個人網站，就可以用 `lynns.me`，底下有 `/blog`、`/portfolio`、`/drawings`⋯⋯這些東西。

---

我是用 Gandi 買域名的，說到為什麼的話，其實我也沒有很確定，只是感覺介面比較有眼緣，價格也比較便宜。

設定很簡單，就是在 Gandi `DNS 紀錄`的地方，加一些東西。

例如我希望 `blog.lynns.me` 顯示 `clhuang224.github.io` 的內容的話，就去 Gandi `DNS 紀錄` 裡新增一筆紀錄：

```
類型：CNAME
名稱：blog
主機名稱：clhuang224.github.io. ←最後面的點點非常重要，一定要有
```

（DNS 中的 CNAME 如果沒有加上最後的 `.`，會被當作相對路徑，自動補上目前的 domain，導致解析錯誤。）

然後到 GitHub Repo 的 Settings > Pages 裡面把 `Custom domain` 設定成 `blog.lynns.me` 就好。

這時候畫面上會顯示正在確認 DNS，但有可能會一直顯示失敗。

這是因為 DNS 的設定需要一段時間才會生效，有可能會到好幾個小時。

想要確認 DNS 有沒有生效，可以在 terminal 裡面輸入 `nslookup blog.lynns.me`，如果看到的結果是 `clhuang224.github.io.` 的話，就表示 DNS 已經生效了。

這邊是一個失敗的結果：

```
nslookup blog.lynns.me
Server:         192.168.0.1
Address:        192.168.0.1#53

** server can't find blog.lynns.me: NXDOMAIN
```

另一種指令是用 `dig blog.lynns.me`，如果看到的結果裡面有 `CNAME clhuang224.github.io` 的話，也表示 DNS 已經生效。

這是一個「看起來有 CNAME，但其實錯誤」的結果：

```
; <<>> DiG 9.10.6 <<>> blog.lynns.me
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NXDOMAIN, id: 54874
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
;; QUESTION SECTION:
;blog.lynns.me.                 IN      A

;; ANSWER SECTION:
blog.lynns.me.          9224    IN      CNAME   clhuang224.github.io.lynns.me.

;; Query time: 3 msec
;; SERVER: 192.168.0.1#53(192.168.0.1)
;; WHEN: Fri Mar 20 08:51:08 CST 2026
;; MSG SIZE  rcvd: 85
```

可以看到有 `clhuang224.github.io.lynns.me.` 的紀錄，這是因為我在 Gandi 沒有加上最後一個點，所以設定錯誤。

redirect 的部分在 Gandi 上很簡單，到 `網頁轉址` 的頁面新增轉址就好。

---

`blog.lynns.me` 生效以後，我發現網誌的樣式、圖片全部都不見了。

看了 devtool 的 Network，發現所有的靜態資源都 `404`，感覺就是 base url 出了問題。

因為原本網誌的 url 是 `clhuang224.github.io/TechBlog`，靜態資源的路徑是相對於 `/TechBlog` 。

但當域名變成 `blog.lynns.me` 以後，靜態資源的路徑就變成相對於 `/` 了，所以就會找不到資源。

網誌是 Hexo 框架產生的靜態網站，相關設定都在 `_config.yml` 裡，調整一下設定再重新部署就可以了。

```yml
url: https://blog.lynns.me
root: /
```

---

同樣地，`bus.lynns.me` 這邊也要調整 base url。

Finding the bus 是 Vite + React Router v7，需要改兩個地方：

`vite.config.ts` 的 `base`:

```ts
export default defineConfig(() => ({
  base: '/',
  plugins: [reactRouter(), tsconfigPaths()]
}))
```

`react-router.config.ts` 的 `basename`:

```ts
export default {
  basename: '/',
  ssr: false
} satisfies Config
```

---

`pomodoro.lynns.me` 是 vue-cli-service build 出來的靜態網站，要調整 `vue.config.js` 的 `publicPath`：

```js
module.exports = {
  publicPath: "/"
};
```

但是在部署遇到一點問題。

因為這個專案是六年前寫的，`npm install` 的時候有很多錯誤，調整 Node 版本要花一些時間，我覺得不值得。

所以我直接到 `gh-pages` 分支把 `/Pomodoro_2020` 取代為 `/`。

---

`pre-prompt.lynns.me` 和 `calculator.lynns.me` 是直接寫 `index.html` 的靜態網站，裡面所有的資源路徑都是相對於 `/` 的，所以不需要調整什麼設定。

---

`queener.lynns.me` 是 Vite + Vue 3 + GitHub Action 部署，只要調整 `vite.config.ts` 的 `base` 就好：

```ts
export default defineConfig(() => ({
  base: '/',
  // .....
}))
```

---

到這邊，所有的 DNS 設定和 base url 調整都完成了。

網誌、外部平台跟專案們成功登入 Lynn's 宇宙ˊˇˋ
