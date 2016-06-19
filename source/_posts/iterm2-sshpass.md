---
title: iTerm2保存回话密码
s: iterm2+sshpass
date: 2016-06-18 23:43:31
tags: [iTerm2, sshpass]
categories: Tools
---
> 使用公钥私钥不在讨论范围之内 这篇只记录一下使用`expect`和`sshpass`

<!-- more -->

## 下载`sshpass` [links](http://sourceforge.net/projects/sshpass/)


## 安装`sshpass`
```sh
tar zxvf sshpass-1.05.tar.gz
cd sshpass-1.05
./configure && make
sudo make install
```

## 设置iTerm2

```sh
mkdir sshpass && touch sshpass/vagrant
echo 'vagrant' > vagrant # 写入密码

# iterm2->profiles->command 添加一下命令
/usr/local/bin/sshpass -f /Users/lePig/sshpass/vagrant ssh -p 1122 vagrant@127.0.0.1
```

> 第一次登陆验证秘钥问题,ssh的时候使用 `-o StrictHostKeyChecking=no`也可以在/etc/ssh/ssh_config 增加`StrictHostKeyChecking=no`

```sh
/usr/local/bin/sshpass -p 密码 ssh -o stricthostkeychecking=no -p 1122 vagrant@127.0.0.1
```

---
## 使用`expect`方法

```sh
#!/usr/bin/expect -f
	set host 主机地址
	set user 用户名
	set password 密码
	set timeout -1
	set port 22

	spawn ssh -o StrictHostKeyChecking=no -p $port $user@$host
	expect "*assword:*"
	send "$password\r"
	interact
	expect eof
```

