---
layout: post
title:  "Testing Jekyll Locally"
tags: jekyll github-page ruby bundler
---
# Testing Jekyll Locally
## Install
You can follow [Testing your GitHub Pages site locally with Jekyll](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/testing-your-github-pages-site-locally-with-jekyll)
step by step, or here is some brief instruction:
1. [Download and install Ruby+Devkit](https://rubyinstaller.org/downloads/).  
    Make sure to choose 'Ruby+Devkit'. At the end, the installer will install `ridk` automatically.
2. Run `gem install jekyll bundler`.
3. Run `jekyll -v`, check if installation successfully.  
    Make sure to not run it on Jekyll project directory (contains `Gemfile`) or you will see exception.
4. Go to Jekyll project directory, run `bundle install`.
5. Run `bundle exec jekyll serve`.  
    And the service will start up at <http://localhost:4000/>

## Trouble Shooting
> C:/Ruby30-x64/lib/ruby/gems/3.0.0/gems/bundler-2.2.32/lib/bundler/runtime.rb:309:in `check_for_activated_spec!': You have already activated addressable 2.8.0, but your Gemfile requires addressable 2.7.0. Prepending `bundle exec` to your command may solve this. (Gem::LoadError)

**Solution:** Do not run `jekyll -v` on a directory that contains `Gemfile`.

> bundler: failed to load command: jekyll (/usr/local/lib/ruby/gems/3.0.0/bin/jekyll)
/usr/local/lib/ruby/gems/3.0.0/gems/jekyll-3.9.0/lib/jekyll/commands/serve/servlet.rb:3:in `require': cannot load such file -- webrick (LoadError)

**Cause:** Because you installed Ruby 3.0 and it doesn't include `webrick`  
**Solution:** Run `bundle add webrick`  
Reference: [Jekyll serve fails on Ruby 3.0 (webrick missing)](https://github.com/github/pages-gem/issues/752)  

## About Ruby and its eco system
### Ruby, Gem, Bundler
> * ruby是一个面向对象的脚本语言。
> * rvm是不同ruby版本的管理和切换工具。
> * gem是ruby写的软件包。
> * rubygems是用来打包、下载、安装、使用gem软件包的工具。
> * bundler是管理ruby项目一系列gem的工具，就像ios 包管理工具的cocopods一样。bundler会根据gemfile文件定义的约束去管理这些gem。

Reference: [ruby、rvm、gem、gems、bundle、gemfile简单总结](https://www.jianshu.com/p/c077bdbe85eb).

### Difference between ruby and node

| Language | Node | Ruby |
|  ----  | ----  | ----  |
| Package Manager | [npm](https://www.npmjs.com/) | [RubyGems](https://rubygems.org/) |
| A Package | package | gem |
| Install to Global | `npm install [pkg] -g` | `gem install [pkg]` |
| Install to Project | `npm install` | `bundle install` |
| Description File | `package.json` | `Gemfile` |
| Dependency Tree | `package-lock.json` | `Gemfile.lock` |
