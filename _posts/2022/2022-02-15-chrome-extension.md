---
layout: post
title:  "Chrome Extension 学习心得"
tags: chrome extension keynote
---
# Chrome Extension 简单心得
昨天为了一个"从网站A Copy Data 到网站B"的行为，想写个Chrome插件来实现快捷操作，从而避免在Console里来回复制命令。
本以为很简单的事情，实际做起来，在一些细节上绕了不少弯路。现在事情做完了，就简单记录一下。

Keynotes:
* [`chrome.storage`](https://developer.chrome.com/docs/extensions/reference/storage/) 可以用于持久化插件所需要的数据。
这个数据和localStorage的相互独立，在DevTools里不可见。  
在`background`和`content_script`里保存的数据可以相互读取。
* 可以通过HTML5的`localStorage`访问网页的 localStorage 数据，但这个API只能在`content_script`里调用。  
`background`里的`localStorage`读写的是background页面的内容，一个独立的空间。
* `manifest.json`里的`icons`定义的是插件在各个插件页面上显示的图标。Chrome会根据需要选择合适的大小。
* `manifest.json`里的`browser_action.default_icon`定义了插件在工具栏上显示的图标。
* 可以通过`chrome.browserAction.setIcon`动态改变插件地址栏的图标，以此来直观的显示插件的状态变化，但这个API只能在`background`里调用。
* 未提交到Chrome Extension Store里的插件， 只能通过先下载解压，然后加载的形式安装使用。
