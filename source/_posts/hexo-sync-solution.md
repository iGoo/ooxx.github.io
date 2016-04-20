---
title: Hexo内容同步方案
s: hexo-sync-solution
date: 2016-04-19 18:07:26
categories: Hexo
tags: [hexo, 同步]
---
> 解决思路利用的是github仓库。google搜索后发现大多数用的都是类似的方法。

Step1: 
切换到本地hexo根目录，比如我的是F:\hexo 然后新开辟一个分支(branch)或者新建一个新的仓库(repository)

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
<!-- more -->
```sh
cd F:\hexo
git init 
git checkout -b source
git add .
git commit -m "first commit"
git push origin source
```
到这里基本上A电脑上已经差不多完成了。以后发文章的时候执行`hexo d -g`进行文章发布操作。然后在执行`git push origin source`推送Markdown源文件到新创建的`branch`分支上就可以了。

Step2:

这一步其实是在B电脑上，也就是比如我家里的Mac上操作。

* 首先和在A电脑上一样，配置好环境，也就是安装`hexo`和`node.js`

  ```sh
  brew update
  brew install node
  npm install hexo-cli -g
  ```

  ​

* 安装完成后**千万**不要执行`hexo init`进行初始化。而是使用克隆github仓库的方式来生成`hexo`本地文件夹。

  ```sh
  git clone -b source git@github.com:你的github用户名/你的github用户名.github.io.git
  ```

  上面的操作是需要ssh公钥的，关于如何在github配置公钥请自行Google。

  然后使用`npm install`来安装所依赖的`node.js`插件。(根据package.json文件来安装)



Step3:

到这里其实A、B电脑的同步工作差不多已经完成了。现在就可以在B电脑上进行写文章了。

* 写文章

  ```sh
  hexo new "First Post for Mac"
  hexo g
  hexo d
  git add .
  git commit -m "Added new Post"
  git push origin source
  ```

这样就完成了两个终端之间的同步。这种方法的缺点是每次发布文章的时候其实要执行2次push操作。一次是`hexo d -g`一次是`git push`操作。所以后来发现一个更优雅的方法是使用`Travis CI`持续构建方法。以后有空研究下，目前刚接触`hexo`还是使用这种同步方式吧。

---

关于说同步主题`theme`文件夹出现`.git`文件夹嵌套问题，貌似我push的时候没有报错。所以先不管了，等遇到的时候在解决。