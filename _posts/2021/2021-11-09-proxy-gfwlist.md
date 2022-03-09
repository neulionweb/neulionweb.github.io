---
layout: post
title:  "顺畅的网络访问"
tags: browser proxy network gfwlist
category: it
---
## 问题  
在Office里, 经常会遇到一些打不开的网站, 需要找IT来帮忙'代理'解决.
IT现在的代理策略是在交换机上实现的, 而交换机只能通过IP来指定需要代理的列表, 没办法通过域名来指定.
所以每次都需要把域名先解析成IP再发给IT来解决.  
对于一些动态IP的网站, 会稍微复杂一点. 具体方法是选定某个IP, 通过host文件固定住IP, 比如像Slack:
```
54.192.16.73 dicetechnology.slack.com files.slack.com app.slack.com slack.com a.slack-edge.com slack-imgs.com
```
然后把IP交给IT, 以实现稳定访问(只要代理不挂, 则服务就不受影响)  
但对于像Google这样的拥有众多二级域名的服务, 就有点力不从心了...  

## 对策
PAC则能很好的解决这个问题. 其核心API就一个:
```
var proxy = 'PROXY 172.16.0.33:1080';
function FindProxyForURL(url, host) {
  if (condition) {
    return proxy;
  }
  return 'DIRECT';
}
```
它可以根据请求的URL或域名, 通过自定义策略, 选择是否走代理.  

## 应用
在Office的实际应用就是: 基于域名判断是否需要代理, 需要的走代理, 不需要的则直连交换机.  
一些Use Case:
1. 内网应用的访问: 不受影响. 只要内网应用的域名不被加入代理列表, 就仍然会通过交换机来完成解析.  
2. Office外的访问: 不受影响. 因为PAC有fallback策略, 如果PAC文件访问不了, 或proxy server访问不了, 则仍然直连.
3. 在家: 可以在不切换PAC的情况下正常访问不需要代理的网站. 如果需要代理, 则连接公司VPN.

## 实施
首先需要一组需要代理的域名列表, 于是找到了 [gfwlist](https://github.com/gfwlist/gfwlist) 他会定期更新, 看起来是目前最好的选择.  
然后需要一个基于gfwlist生成PAC文件的工具, 于是找到了 [genpac](https://github.com/JinnLynn/genpac)  
最后需要一个使用的例子, 于是找到了 [gfwlist2pac](https://github.com/petronny/gfwlist2pac)  
搬砖的感觉真好!  
Fork过来稍微改了下, 设置了代理, 配上GitHub Action, 添加自定义域名列表, here you go:
```
https://raw.githubusercontent.com/helloint/gfwlist2pac/master/gfwlist.pac
```
源码在: `https://github.com/helloint/gfwlist2pac` 欢迎Fork.

## 解释
1. PAC文件一经设置, 在浏览器端立即生效.

## 已知问题
1. 网络环境发生切换时, 好像会导致PAC策略失效. 具体现象是, 走PAC打不开的网站, 直连proxy可以打开. `未解决`
