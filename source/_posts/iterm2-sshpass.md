---
title: iTerm2+sshpass配置
s: iterm2+sshpass
date: 2016-06-18 23:43:31
tags: [iTerm2, sshpass]
categories: Tools
---
## 下载`sshpass` [links](http://sourceforge.net/projects/sshpass/)




## 安装`sshpass`
```sh
tar zxvf sshpass-1.05.tar.gz
cd sshpass-1.05
./configure && make
sudo make install
```

## 设置iTerm2
1


```sh
mkdir sshpass && touch sshpass/vagrant
echo 'vagrant' > vagrant # 写入密码
/usr/local/bin/sshpass -f /Users/lePig/sshpass/vagrant ssh -p 1122 vagrant@127.0.0.1
```



第一次登陆验证秘钥问题

```sh
/usr/local/bin/sshpass -p 密码 ssh -o stricthostkeychecking=no -p 1122 vagrant@127.0.0.1
```
