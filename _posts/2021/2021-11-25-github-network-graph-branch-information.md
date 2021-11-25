---
layout: post
title:  "GitHub Network & Branch Information Display"
tags: github network graph branch
---
# GitHub Network & Branch Information Display
Hope this topic can help to easier understand the `GitHub Network Graph`.
  
## Line
### New branch:  
 ![New branch]({{ site.cdn_prefix }}assets/images/2021/11/25/line-branch.png)  
### First commit on the new branch:  
 ![First commit]({{ site.cdn_prefix }}assets/images/2021/11/25/line-first-commit.png)  
### A Merge:  
 ![Merge]({{ site.cdn_prefix }}assets/images/2021/11/25/line-merge.png)
### A Merge, which is also the First commit on this new branch:  
 ![First commit Merge]({{ site.cdn_prefix }}assets/images/2021/11/25/line-merge-first-commit.png)

## Branch Information
### The head of the branch (not merged):
 ![Branch head]({{ site.cdn_prefix }}assets/images/2021/11/25/branch-head.png)
### Merge without PR
 ![Merge without PR]({{ site.cdn_prefix }}assets/images/2021/11/25/branch-merge-nopr.png)
### Merged branch. Usually on the last commit before the merge.
 ![Merged branch]({{ site.cdn_prefix }}assets/images/2021/11/25/branch-merged.png)
### A special case:
 ![special case]({{ site.cdn_prefix }}assets/images/2021/11/25/branch-special-case.png)

## Branch Information Display
### Merge code from release branch. (#PR to release branch)
 ![Merge code]({{ site.cdn_prefix }}assets/images/2021/11/25/branch-info-merge.png)
### Branch commit, no PR
 ![branch commit no PR]({{ site.cdn_prefix }}assets/images/2021/11/25/branch-info-commit-nopr.png)
### Branch commit, has PR
 ![branch commit has PR]({{ site.cdn_prefix }}assets/images/2021/11/25/branch-info-commit-pr.png)
### PR to release branch
 ![PR to release branch]({{ site.cdn_prefix }}assets/images/2021/11/25/branch-info-pr-to-release.png)
### Tags (including all futures) on this branch
 ![Tags]({{ site.cdn_prefix }}assets/images/2021/11/25/branch-info-tags.png)
