---
layout: post
title:  "GitHub Network Instruction & Branch, Tag"
tags: github network graph branch tag
---
# GitHub Network Instruction & Branch, Tag
Hope this topic can help you to easier understand the `GitHub Network Graph`.  
Cases can be seen on <https://github.com/helloint/github-network/network>

## Line
### The head of the branch. The branch label is on the head:
![Branch head]({{ site.cdn_prefix }}assets/images/2021/11/25/line-branch-head.png)  
### New branch:  
![New branch]({{ site.cdn_prefix }}assets/images/2021/11/25/line-branch.png)  
### First commit on the branch:  
![First commit]({{ site.cdn_prefix }}assets/images/2021/11/25/line-first-commit.png)  
### A Merge:  
![Merge]({{ site.cdn_prefix }}assets/images/2021/11/25/line-merge.png)  
### * A Merge, which is also the First commit on this new branch  
![First commit Merge]({{ site.cdn_prefix }}assets/images/2021/11/25/line-merge-first-commit.png)  
(? no idea how this happens)  
### Merged branch via PR, feature branch not deleted. The branch label is usually on the last commit before the merge.
![Merged branch]({{ site.cdn_prefix }}assets/images/2021/11/25/line-branch-merged.png)  
### Merged branch via PR, feature branch deleted.
![Merge without PR]({{ site.cdn_prefix }}assets/images/2021/11/25/line-branch-merged-deleted.png)  
### Colored branch.
![Colored branch]({{ site.cdn_prefix }}assets/images/2021/11/25/line-color.png)

## Commit Information
### A Merge via PR  
![Merge code]({{ site.cdn_prefix }}assets/images/2021/11/25/branch-info-merge.png)
### A commit on a branch that has already been merged to another branch
![A commit merged]({{ site.cdn_prefix }}assets/images/2021/11/25/branch-info-commit-merged.png)
### A commit on a branch that is not merged
![A commit not merged]({{ site.cdn_prefix }}assets/images/2021/11/25/branch-info-commit-not-merged.png)

## Tags
### Tags including all children commit
![Tags multiple]({{ site.cdn_prefix }}assets/images/2021/11/25/tag-multiple.png)  
### One single tag
![Tags single]({{ site.cdn_prefix }}assets/images/2021/11/25/tag-single.png)

? Weird that tags on [this commit](https://github.com/helloint/github-network/commit/9d2ec75add9e296599cc45393c60bcf23dbdcb5e) doesn't display.  
