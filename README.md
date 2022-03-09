# 团队博客

这些人很懒, 啥都没写……


## Contributing
**介绍**  
本博客基于[Jekyll](https://jekyllcn.com/)构建.  
博客主题: [Yummy-Jekyll](https://github.com/DONGChuan/Yummy-Jekyll) (参考文章: <https://juejin.cn/post/6844903705972572168>)  
域名: <https://neulionweb.dev/>  
部署: GitHub Action 自动部署  

**本地环境**  
* 安装 `Jekyll` <http://jekyllcn.com/docs/installation/>  
Note: 建议通过`Homebrew`安装`Ruby`替换macOS自带版本  

版本参考：
```
`ruby -v` ruby 3.1.0p0 (2021-12-25 revision fb4df44d16) [arm64-darwin21]  
`gem -v` 3.3.8  
`bundle -v`  Bundler version 2.3.8  
`jekyll -v` jekyll 4.2.2
```  

* 初始化 `bundle install`
* 启动 `jekyll serve`

**文档说明**  
* 文章Markdown文件存放在`./_posts`下, 目录格式: `_posts/yyyy/yyyy-mm-dd-{title}.md`  
* Assets文件(主要是图片)存放在`./assets`下, 目录格式: `assets/images/yyyy/mm/dd/{filename}`, 引用方式举例:
`
![Browser Marketing]({{ site.cdn_prefix }}assets/images/2021/01/14/browser-marketing-2020.png)
`

**其他**  
域名服务商: [Google Domains](https://domains.google.com/registrar/), `your-name.dev`, $12/Year。  
DNS: [Cloudflare](https://dash.cloudflare.com/), 免费, 并提供SSL证书, 对GitHub友好。  
