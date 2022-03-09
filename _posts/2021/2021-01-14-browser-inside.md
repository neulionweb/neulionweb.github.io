---
layout: post
title:  "Browser Inside"
tags: frontend browser chrome
---
这篇文章聊一下主流浏览器的内核版本, 浏览器引擎, 以及内部的Javascript引擎.  
导读文章: [[译] 你能分得清楚 Chromium, V8, Blink, Gecko, WebKit 之间的区别吗？](https://juejin.cn/post/6844904055236460558)  
>“3 个主要的浏览器引擎下，是 3 个不同的 JavaScript 引擎。也就是说现在市场上只有 3 个主要的 JavaScript 引擎。Chromium 市场份占据 65％，再加上从 Edge 和 Internet Explorer 吸收的大约 15％ 的市场份额，后面还会继续扩大。当前，Web 开发人员正在最流行的浏览器引擎上构建能够发挥最佳性能的网站。但是 Chromium 最后有没有可能重蹈 Internet Explorer 6 的覆辙呢？不过还是希望 Chromium 仍能继续跟进标准的步子，并且随着来自 Firefox 和 Safari 的竞争，相信未来的发展也会更加明朗和积极。希望 Google 不会减慢 Chromium 的开发速度，并在如此高的市场份额下继续保持竞争力。”

## Browser Engines and Marketing

|Browser|Chrome ([History](https://en.wikipedia.org/wiki/Google_Chrome_version_history))|Safari|Firefox|(Chromium)|Edge (79+)|Edge legacy|IE|
|:---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|Marketing<sup>[2]</sup>|63.38%|19.25%|3.77%|(67.15%)|3.08%|0.33%|1.05%|
|Core|Chromium||||Chromium|||
|Browser Engine||WebKit|Gecko|Blink<sup>[1]</sup>||EdgeHTML||
|Javascript Engine||JavaScriptCore|SpiderMonkey|V8||||

[1]: iOS的Chrome, 由于Apple的限制, 用的是WebKit引擎. 2017年, Google把WebKit的支持集成到了Chromium, 从而使基于Chromium的全平台开发变得更容易.  
[2]: Marketing Share data comes from statcounter 2020/12  

### How Chrome grows to No.1
0→63.38% takes around 10 years.
![Browser Marketing]({{ site.cdn_prefix }}assets/images/2021/01/14/browser-marketing-2020.png)

## Chromium版本号
### Chromium
See [The Chromium Projects Version Numbers](https://www.chromium.org/developers/version-numbers)  
>Chromium version numbers consist of 4 parts: MAJOR.MINOR.BUILD.PATCH.  

值得注意的是, MAJOR.MINOR是由Chrome基于商业目的决定的. 只有BUILD和PATCH是和dev release cycle相关.

### Chrome
`chrome://settings/help`  
Chrome的版本号始终和 Chromium保持一致. 因为一方面Chrome=Chromium+Google服务, 另一方面Chromium的release其实是和Chrome保持同步的.  
![Check Chrome Version]({{ site.cdn_prefix }}assets/images/2021/01/14/chromium-version-same-as-chrome.png)

userAgent:  
`Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/87.0.4280.141 Safari/537.36`

### New Edge
`edge://version/`  
Edge的版本号会略微落后 Chromium一些, 原因是Microsoft需要在Chromium之上整合他们自己的功能, 这需要一些时间.  
![Check Chromium Core Version of New Edge]({{ site.cdn_prefix }}assets/images/2021/01/14/new-edge-chromium-version.png)

userAgent:  
`Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/87.0.4280.141 Safari/537.36 Edg/87.0.664.75`  

## Others
### Chrome是否会令人担忧?
任何一个领域, 垄断都不是什么好事, 自然会让人产生担忧. 比如:
+ [Google的商业目的左右Chromium](https://blog.csdn.net/csdnnews/article/details/103692084)
+ [隐私问题](https://blog.csdn.net/csdnnews/article/details/88083654) 如何在Google 和 Chrome之间划清界限?

