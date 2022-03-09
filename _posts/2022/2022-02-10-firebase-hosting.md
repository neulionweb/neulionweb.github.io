---
layout: post
title:  "用Firebase Hosting部署一个Web App"
tags: firebase hosting deploy webapp
---
## Firebase
Firebase: `https://firebase.google.com/` 可以用Google账号登陆。  
进去后先建个project, 就能看到他的主界面.  
他上面有很多Products, 像Analytic, Error Report等，都是mobile app的常用功能。

## Hosting
如果你要setup一个SPA website, 可以用他的Hosting功能。  
具体步骤很简单，进到Hosting Page， 照着网站`Get started`步骤说明走就行了.  
以下是所有命令：
```
npm install -g firebase-tools
firebase login
firebase init
firebase deploy
```
简单说明：
1. `firebase init`会在根目录创建2个配置文件：`.firebaserc`和`firebase.json`，这文件要看情况选择是否提交到仓库。  
2. 以后只需要敲`firebase deploy`, 他就会把你当前的public目录(默认)给发布上去了, 非常方便.
也可以考虑把这个命令放到`npm scripts`里去。
```
"deploy": "firebase deploy --only hosting"
```

注意：
1. `firebase login`需要翻墙。`firebase deploy`也可能需要翻墙，否则这个API会打不开`https://firebase-public.firebaseio.com/cli.json`
2. 免费账号(Spark Plan)每天有[360MB](https://firebase.google.com/pricing?authuser=0) 的流量限制, 所以只能用于小范围跑跑demo
