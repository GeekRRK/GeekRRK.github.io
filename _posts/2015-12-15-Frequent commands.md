---
layout: post
title: Frequent commands
---

### CocoaPods
`$ pod install --verbose --no-repo-update`  
`$ pod update --verbose --no-repo-update`  
`$ gem sources --remove https://rubygems.org/`  
`$ gem sources -a http://ruby.taobao.org/`  
`$ gem sources -l`  
`$ sudo gem update --system`  
`$ sudo gem install cocoapods`  
`$ rvm use ruby-1.9.3-p448`  
`$ pod search AFNetworking`  
`$ pod spec create [NAME|https://github.com/USER/REPO]`  

### Git
Sync with author after fork  
`$ git clone -o forkversion https://github.com/geekrrk/test.git`  
`$ git remote add originalversion https://github.com/originalauthor/test.git`  
`$ git remote -v`  
`$ git remote -vv`  
`$ git remote rename oldname newname`  
`$ git branch`  
`$ git checkout originalversion`  
`$ git pull`  
`$ git checkout forkversion`  
`$ git push forkversion master`  
`$ git status`  

`$ git checkout -b mywork origin`  
`$ git fetch`  
`$ git rebase origin`  
`$ git merge origin`  

`$ git branch -u upstream/foo foo`  
`$ git branch --set-upstream-to=upstream/foo foo`  
`$ git branch -m oldname newname`  

`$ git tag -a v1.0.2 -m "my version 1.0.2"`  
`$ git tag`  
`$ git tag -l "v1.0.2*"`  
`$ git show v1.0.2`  
`$ git log --pretty=oneline`  
`$ git tag -a v1.0.1 9fceb02`  
`$ git push origin v1.0.1`  
`$ git push origin --tags`  
`$ git checkout -b version1 v1.0.1`  

### Linux
