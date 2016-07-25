---
title: 'Alpine轻量安全的linux发行版 '
s: try-alpine-linux-release
date: 2016-07-25 18:57:24
tags: [alpine]
categroies: Linux
---

```sh
echo “http://nl.alpinelinux.org/alpine/v3.3/community” >> /etc/apk/repositories
apk update
apk add docker
rc-update add docker boot
service docker start
```