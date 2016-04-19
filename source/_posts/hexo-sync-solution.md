---
title: Hexo内容同步方案
s: hexo-sync-solution
date: 2016-04-19 18:07:26
tags: [hexo, 同步]
---
> 解决思路利用的是github仓库。google搜索后发现大多数用的都是类似的方法。

1. 切换到本地hexo根目录，比如我的是F:\hexo 然后新开辟一个分支(branch)或者新建一个新的仓库(repository)
`如果hexo根目录下没有.gitignore文件就新建一个，加入如下内容`
    ```sh
    .DS_Store
    Thumbs.db
    db.json
    *.log
    node_modules/
    public/
    .deploy_git/
    .deploy/
    ```
    
    ```bash
        cd F:\hexo
        git init 
        git checkout -b source
        git add .
        git commit -m "first commit"
        git push origin source
    ```


2. 
