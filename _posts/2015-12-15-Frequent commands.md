---
layout: post
title: Frequent commands
---

### CocoaPods
`$ pod install --verbose --no-repo-update`  
`$ pod update --verbose --no-repo-update`  
`$ pod setup`  
`$ gem sources --remove https://rubygems.org/`  
`$ gem sources -a http://ruby.taobao.org/`  
`$ gem sources -l`  
`$ sudo gem update --system`  
`$ sudo gem install cocoapods`  
`$ sudo gem install -n /usr/local/bin cocoapods`  
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

`$ git clone -b <branch> <remote_repo>`  
`$ git branch -d[D] testing`  

`$ git fetch origin develop:develop`  
`$ git fetch origin coderA:coderA`  
`$ git checkout develop`  
`$ git pull`  
`$ git diff coderA`  
`$ git merge coderA`  
`$ git push`  
`$ git reset [--hard]`  
`$ git revert`  
`$ git reflog`  

`$ git commit --amend`  
`$ git push --force example-branch`  

### Linux
`$ find . -name “*.html” |xargs grep "hello world"`  
`$ grep ‘GitHub’ Valuable websites.md`  
`$ find ./ -size 0 | xargs rm -f &`  
`$ grep 'test' d*`  
`$ grep 'test' aa bb cc`  
`$ grep '[a-z]\{5\}' aa`  

`$ tar -czf test.tar.gz /test1 /test2`  
`$ tar -tzf test.tar.gz`  
`$ tar -xvzf test.tar.gz`  

`$ head -n 10 example.txt`  
`$ tail -n 10 example.txt`  
`$ tail -f exmaple.log`  

`$ netstat -tln | grep 8080`  
`$ lsof -i :8080`  
`$ ps –ef|grep tomcat`  
`$ ps aux|grep java`  

`$ tree a`  
`$ alias tree="find . -print | sed -e 's;[^/]*/;|____;g;s;____|; |;g'"`  
`$ source ~/.profile`  

`$ wget http://file.tgz`  
`$ curl http://file.tgz`  

`$ ssh userName@ip`  
`$ scp sourecFile romoteUserName@remoteIp:remoteAddr`  

`$ export http_proxy="http://localhost:port"`  
`$ export https_proxy="http://localhost:port"`  

`$ ssh -N -D 7070 root@94.249.184.93`  
`$ SSH root@94.249.184.93`  
`$ groupadd internetfreedom`  
`$ useradd -d /home/freenutsdotcom -m -g internetfreedom -s`    
`$ /bin/false freenutsdotcom`  
`$ passwd freenutsdotcom`  
`$ ssh -N -D 7070 freenutsdotcom@94.249.184.93`  

The following iptables related commands refer to: <http://blog.csdn.net/cssmhyl/article/details/7966789>  
`$ iptables -P INPUT DROP`  
`$ iptables -P FORWARD DROP`  
`$ iptables -P OUTPUT DROP`  
`$ iptables -L -n`  
`$ service iptables save`  
`$ vim /etc/sysconfig/iptables`  
`$ iptables -A INPUT -p tcp --dport 22 -j ACCEPT`  
`$ iptables -A OUTPUT -p tcp --sport 22 -j ACCEPT`  
`$ iptables -A INPUT -p tcp -s 192.168.1.104 -j DROP`  
`$ iptables -L -n --line-number`  
`$ iptables -D INPUT 2`  
`$ iptables -A OUTPUT -p tcp --sport 22 -m state --state ESTABLISHED -j ACCEPT`  
`$ iptables -A OUTPUT -p tcp --sport 80 -m state --state ESTABLISHED -j ACCEPT`  
`$ grep domain /etc/services`  
`$ iptables -A OUTPUT -p udp --dport 53 -j ACCEPT`  
`$ iptables -A INPUT -p udp --sport 53 -j ACCEPT`  
`$ iptables -A INPUT -p udp --dport 53 -j ACCEPT`  
`$ iptables -A OUTPUT -p udp --sport 53 -j ACCEPT`  
`$ iptables -A INPUT -p tcp --dport 21 -j ACCEPT`  
`$ iptables -A INPUT -p tcp --dport 20 -j ACCEPT`  
`$ iptables -A OUTPUT -p tcp --sport 21 -j ACCEPT`  
`$ iptables -A OUTPUT -p tcp --sport 20 -j ACCEPT`  
`pasv_min_port=30001`  
`pasv_max_port=31000`  
`$ iptables -A INPUT -p tcp --dport 30001:31000 -j ACCEPT`  
`$ iptables -A OUTPUT -p tcp --sport 30001:31000 -j ACCEPT`  
