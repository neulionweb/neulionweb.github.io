---
layout: post
title:  "Testing Jekyll Locally"
tags: jekyll github-page ruby bundler
---
# Testing Jekyll Locally
## Install
You can follow [Testing your GitHub Pages site locally with Jekyll](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/testing-your-github-pages-site-locally-with-jekyll)
step by step, or here is some brief instruction:
1. [Download and install Ruby and Jekyll](https://jekyllrb.com/docs/installation/windows/).  
    Make sure to choose 'Ruby+Devkit'. At the end, the installer will install `ridk ` automatically.
2. Run `gem install jekyll bundler`.
3. Run `jekyll -v`.  
    Make sure to not run it on a directory that contains `Gemfile` or you will see exception.
4. Run `gem install bundler`.
5. Run `bundle exec jekyll serve`  
    And the service will start up at http://localhost:4000/

## Trouble Shooting
> C:/Ruby30-x64/lib/ruby/gems/3.0.0/gems/bundler-2.2.32/lib/bundler/runtime.rb:309:in `check_for_activated_spec!': You have already activated addressable 2.8.0, but your Gemfile requires addressable 2.7.0. Prepending `bundle exec` to your command may solve this. (Gem::LoadError)

Solution: Do not run `jekyll -v` on a directory that contains `Gemfile`.

> bundler: failed to load command: jekyll (/usr/local/lib/ruby/gems/3.0.0/bin/jekyll)
/usr/local/lib/ruby/gems/3.0.0/gems/jekyll-3.9.0/lib/jekyll/commands/serve/servlet.rb:3:in `require': cannot load such file -- webrick (LoadError)

Cause: Because you installed Ruby 3.0 and it doesn't include `webrick`
Solution: `bundle add webrick`  
Reference: https://github.com/github/pages-gem/issues/752  

