---
title: 【Hexo】使用Hexo+github pages+travis ci搭建好看的个人博客（一）
tags:
  - 环境搭建
  - Hexo
  - github pages
  - travis ci
categories:
  - 编程
  - 生活
toc: true
date: 2020-05-02 11:51:35
---

## 一、说明

本系列文章将会详细说明使用 `Hexo` + `github pages` 来搭建个人博客，并对主题进行配置，然后使用 `travis ci` 来进行自动化部署的全过程。

搭建一个赏心悦目的博客，写文章和阅读也会更加舒适，一次搭建，终生使用，而且还全程免费，何乐而不为呢。

通过本系列文章的学习，你将收获一个免费且漂亮的个人博客，并熟悉搭建、写作、部署的全流程以及其中一些很好用的工具。

## 二、成品展示

[在线 Demo](https://mfrank2016.github.io/breeze-blog/)

![hexo-1.jpg](https://i.loli.net/2020/05/02/j6fIGtNmqB9Vlgi.jpg)
![hexo-2.jpg](https://i.loli.net/2020/05/02/Eko6AGPpxIfrUMX.jpg)
![hexo-3.jpg](https://i.loli.net/2020/05/02/g8FYkmlAK7dX2qc.jpg)
![hexo-4.jpg](https://i.loli.net/2020/05/02/pcWruwXdm3aO7VZ.jpg)
![hexo-5.jpg](https://i.loli.net/2020/05/02/OCNwnEh3SdvBUe4.jpg)

这只是其中的一个主题，如果不喜欢，也可以很方便的切换其它主题。

`hexo` 主题相当丰富，可以在[这里](https://hexo.io/themes/)选择喜欢的主题进行切换即可。

## 三、前期准备

在开始搭建之前，需要准备以下几样东西：

* 本地安装 `node.js`
* 本地安装 `git`
* 一个 `github` 账号
* 创建一个 `github` 仓库
* 一个 `travis ci` 账号

已经有过安装经验的同学，可以根据自己情况选择性的跳过部分章节。

### 本地安装 node.js

`windows` 系统可以在[这里](https://nodejs.org/zh-cn/download/)下载 `installer` 安装包进行安装。

`mac` 系统可以在[这里](https://nodejs.org/zh-cn/download/)下载 `pkg` 安装包，也可以使用 `homebrew` 进行安装：

```bash
brew install node
```

然后在命令行输入以下命令来验证是否正确安装:

```bash
node -v
```

### 本地安装 git

`windows` 系统可以从[这里](https://gitforwindows.org/)下载安装包后进行安装。

`mac` 系统可以从[这里](https://sourceforge.net/projects/git-osx-installer/)下载安装包进行安装。

也可以使用 `homebrew` 进行安装：

```bash
brew install git
```

输入以下命令来查看是否正确安装好了 `git` ：

```bash
git --version
```

然后设置自己的用户名和邮箱：

```bash
git config --global user.name "你的用户名"
git config --global user.email "你的公司或个人邮箱"
```

### github 账号

首先，需要注册一个 `github` 账号，[点击这里](https://github.com/join?source=header-home)。

填写好用户名和密码，验证完成后，便可以将一个 `github` 账号收入囊中。

创建好账号之后，我们还需要把我们本地的 `SSH Key` 添加到 `github` 中去，这样我们之后才能有权限将本地代码推送到 `github` 中。

先本地生成一对 `RSA` 密钥：

```bash
ssh-keygen -t rsa -b 4096 -C "你的邮箱"
```

然后用食指敲击你的回车键三次，记住，要用食指，但别问为什么。

找到你刚才创建的密钥，`windows` 用户可以在 `C://用户//admin//.ssh` 目录下查找，mac 用户可以在 `~/.ssh` 目录下找到。复制 `id_rsa.pub` 文件里的信息，然后到[这里](https://github.com/settings/keys)添加新的 `SSHKEY` 。

![hexo-10.jpg](https://i.loli.net/2020/05/02/k3uVFawNbvL84eB.jpg)

![hexo-11.jpg](https://i.loli.net/2020/05/02/ijrXcU1ZT96QqsP.jpg)

把我们刚才的 `key` 复制进行后保存即可。

### 创建仓库

点击[这里](https://github.com/new)，创建一个新的仓库。

![hexo-5.jpg](https://i.loli.net/2020/05/02/zoDx4HQrACMSYbI.png)

仓库名称可以随便取，比如：`blog`、`my-blog`，随便取一个就好。仓库说明也可以随便写，可以大概描述一下你这个仓库是做什么的。可以参考一下[我的仓库](https://github.com/MFrank2016/breeze-blog)

然后把仓库地址记下来，是下图中箭头标示的 `git` 开头的地址，后面需要用到。

![hexo-12.jpg](https://i.loli.net/2020/05/02/e36Vp8isyRfYvaw.jpg)

创建好仓库之后，就可以进行下一步了。

### travis ci 账号

`travis ci` 账号是跟 `github` 账号关联的，所以需要先创建好 `github` 账号，创建好之后，点击[这里](https://travis-ci.org/signin)进行账号关联登陆。

在设置里进行一次[账户同步](https://travis-ci.org/account/repositories)：

![hexo-7.jpg](https://i.loli.net/2020/05/02/Th5tdWvNYlXbz3D.jpg)

同步完后刷新一下页面，刚才创建的仓库应该就会出现在这里：

![hexo-8.jpg](https://i.loli.net/2020/05/02/vznkDHh9frOMlIX.jpg)

## 四、安装 Hexo

`hexo` 是一款静态网站生成工具，可以根据设置的主题样式和配置文件，来生成丰富多彩的网页，通常配置文件设置好之后不需要经常修改，我们只需要负责写好我们的博文，写好之后就能使用命令一键生成网站，而且还可以为所欲为的切换主题，可以说是写博客的一大利器。

`hexo` 的安装其实很简单，只需要输入以下咒语：

```bash
npm install -g hexo-cli
```

然后轻轻的敲击你的回车键，`hexo` 便能成功的安装在你的电脑中。

可以使用以下命令进行验证：

```bash
hexo -v
```

## 五、使用 hexo 搭建博客

选择一个准备放置博客网站的目录，然后使用以下命令来初始化一个项目：

```bash
hexo init breeze-blog
cd breeze-blog
npm install
```

该命令将会在当前目录下，生成一个名为 `breeze-blog` 的新目录，当然，你可以把这个名字换成任何你想要的名字，并将 `hexo` 的初始化文件写入其中。

新建完成后，`breeze-blog` 文件夹的目录如下：

```bash
.
├── _config.yml
├── package.json
├── node_modules
├── scaffolds
├── source
|   ├── _drafts
|   └── _posts
└── themes
```

`_config.yml` 是配置文件，里面有很多可以配置的数据，这里暂时不多介绍，后面的文章里会进行详细说明。

`package.json` 是应用程序信息，通常不需要关心。

`node_modules` 用来存放 `node` 相关的模块，通常不需要关心。

`scaffolds` 里面是模版文件，也就是每次新建文章时，都会根据模版文件来创建对应的 `md` 文件，这一点也会在后续的文章里进行详细介绍。

`source` 是资源文件夹，用来存放用户资源的地方。除 `_posts` 文件夹之外，开头命名为 _ (下划线)的文件 / 文件夹和隐藏的文件将会被忽略。

`theme` 是主题文件夹，每个主题的配置都会有些不一样，需要根据具体主题情况来定，后续介绍主题的文章里会有说明。

在 `breeze-blog` 目录下使用以下命令来运行我们的博客：

```bash
hexo server
```

在默认情况下，服务会使用 `4000` 端口，如果已经被占用，也可以添加 `-p` 参数来换用其它端口：

```bash
hexo server -p 8080
```

打开 `http://localhost:4000` 即可访问我们生成的网站了。

![hexo-9.jpg](https://i.loli.net/2020/05/02/ntX31VrhMTjFozQ.jpg)

这样，我们的博客就搭建起来了。

## 六、部署到 github pages

`github pages` 可以理解为 ~~gayhub~~ `github` 提供的免费网页空间，可以用来存放你的静态网页文件，并通过 `https://用户名.github.io/项目名/` 的方式来访问，比如我新创建的 `blog` 地址就是：`https://mfrank2016.github.io/breeze-blog/`。

利用 `github pages` 就能创建我们的免费博客站点了，至于为什么要使用免费站点，而不选择购买服务器来搭建，是因为根据之前使用服务器经常忘记续费，导致博客数据丢失，损失惨重。`github` 已经稳定运行了很多年，是全球最大的 ~~同性交友网站~~ 开发者网站，他们的服务值得信赖。而且是免费的。

我们之前已经注册好 `github` 的账号并创建好了对应的仓库，本地也安装好了 `git` ，现在让我们来把他们利用起来。（如果还没有完成的同学可以往上面翻翻，先完成前面的步骤）

> 注意：有两种类型的 `github pages`，一种是使用 `用户名.github.io` 作为项目名，一种是使用其它名称。虽然看起来只是名字不一样，但两种方式其实是有差异的，前一种方式里，网页静态文件只能存放在 master 分支，所以如果想要把博客源文件也存到同一个仓库，必须使用其它分支来存放，相应的 travis ci 监听和推送的分支也需要修改，当然也可以使用另一个新的仓库来存放。后一种方式则没这个限制，通常使用名为 `gh-pages` 作为分支名，`Hexo` 内默认设置的分支也是叫这个名字。这里我们使用的是后一种方案，即源文件和生成的网页静态文件存放在同一个仓库，源文件在 `master` 分支，静态文件在 `gh-pages` 分支。

首先，我们将本地的文件推送到 `github` 上。

在 `breeze-blog` 目录下，初始化 `git` 仓库，将现有文件添加到 `git` 仓库中，并创建 `gh-pages` 分支：

```bash
cd breeze-blog
git init
git add .
git commit -am"init blog"
git remote add origin 仓库地址
```

仓库地址是前面我们创建仓库时说过的地址，比如我的地址是： `git@github.com:MFrank2016/breeze-blog.git` ，把它复制到这里来替换即可。

然后我们使用最后一句咒语，把代码推送到仓库中去。

```bash
git push -u origin master
```

如果你的仓库原来已经有数据了，可以添加 -f 参数来强制推送，但这样会使得你原来的数据丢失，所以慎用。

```bash
git push -f -u origin master
```

然后创建一个新的本地分支 `gh-pages`，并关联远程分支：

```bash
git checkout -b gh-pages
git push -u origin gh-pages
```

⚠️不要改用其它分支名。

然后在项目 `settings` 页面里开启 `github pages`：

![hexo-13.jpg](https://i.loli.net/2020/05/02/bQWKGwMsAVYk5hp.jpg)

![hexo-14.jpg](https://i.loli.net/2020/05/02/HqpxGos7J9mLRkC.jpg)

这里要选择 `gh-pages` 分支，不要选 `master` 分支。

然后我们修改一下 `hexo` 的配置文件(`_config.yml`)，找到对应的地方进行修改，指定我们的仓库信息，并修改 `root` 和 `url` 信息。

```bash
url: https://mfrank2016.github.io/
root: /breeze-blog/

deploy:
  type: 'git'
  repo: git@github.com:MFrank2016/breeze-blog
  branch: gh-pages
```

把这里的 `repo` 地址修改为你的仓库地址即可。

安装 `hexo-deployer-git`：

```bash
cd breeze-blog
npm install hexo-deployer-git --save
```

万事具备，发车！

```bash
hexo clean && hexo generate
hexo deploy
```

运行完成后，我们的博客文件就顺利部署到 `github pages` 上了，现在我们打开下面网址来查看我们的博客效果：

```bash
https://用户名.github.io/项目名/
```

之后每次我们添加或修改完本地文件后，使用：

```bash
hexo clean && hexo g -d
```

即可重新生成项目文件，并推送到 `github` 项目的 `gh-pages` 分支，为了备份数据，也方便我们在不同设备上进行编辑，最好将我们修改的文件推送到 `master` 分支进行保存：

```bash
git checkout master
git add .
git commit -am "这里可以写一下修改的备注信息"
git push
```

## 七、使用 travis ci 进行自动化部署

如果我们每次都按前面的方式进行操作，也会略显麻烦，使用 `travis ci` 后，可以将前面部署的步骤自动化，我们只需要将本地修改的文件推送到 `github` 仓库，就会触发 `travis ci` 的自动部署。

`travis ci` 的配置也很简单，而且只需要配置一次，之后就不需要修改了。

首先，我们需要把*_config.yml*文件里的*repo*信息注释一下，不需要在配置文件里指定仓库地址，`travis ci` 会直接在其监听项目上进行部署。

```yml
deploy:
  type: 'git'
#  repo: git@github.com:MFrank2016/breeze-blog
  branch: gh-pages
```

在本地博客目录下创建一个名为 `.travis.yml` 的文件，与 `_config.yml` 要在同级目录。

然后在文件中写入以下内容：

```yml
sudo: false
language: node_js
node_js:
  - 12
cache: npm
branches:
  only:
    - master # build master branch only
script:
  - hexo generate
deploy:
  provider: pages
  skip-cleanup: true
  github-token: $GH_TOKEN
  keep-history: true
  on:
    branch: master
  local-dir: public
```

这里没有任何东西需要修改，直接复制粘贴即可。

接下来，需要在 `travis` 里配置一个环境变量，`GH_TOKEN` 。

前面我们已经将 `travis` 关联了 `github` 账号，并同步了项目，如果操作正确，[这里](https://travis-ci.org/dashboard)应该会出现我们的仓库信息。

![hexo-15.jpg](https://i.loli.net/2020/05/02/Xc2OGJvDNFbhYPs.jpg)

![hexo-16.jpg](https://i.loli.net/2020/05/02/E7QyHTNMdjJWP26.jpg)

![hexo-17.jpg](https://i.loli.net/2020/05/02/qjJkhAgErKL5tUR.jpg)

这里的 `access token` 是指 `github token`，可以在[这里](https://github.com/settings/tokens)获取：

![hexo-18.jpg](https://i.loli.net/2020/05/02/K35fhMCGLJ7xiUw.jpg)

![hexo-19.jpg](https://i.loli.net/2020/05/02/7wNrX19YqGZKLOs.jpg)

选好后，点击 `generate` 即可生成一个新的 `access token`，这个 `token` 即是用于权限验证的，好好保存，不要泄露，千万不要直接写到 `config` 文件里，而且之后是无法进行查看的，所以需要记录在一个安全的地方。

创建好之后，把这个 `token` 填写到前面的 `travis ci` 的项目环境变量中保存，这样一切就准备好了。

现在让我们在本地创建一篇新的博客，然后推送到远程仓库：

```bash
cd breeze-blog
git checkout master
hexo new "my first blog"
git add .
git commit -am"add a new blog"
git push
```

然后我们可以在 `travis ci` 中看到构建过程被触发了，等待一会即可完成部署，然后再打开我们的博客，查看一下我们新生成的文章是否已经在上面了。（浏览器有缓存，所以可能需要刷新几次才有效果）

## 小结

整个过程看起来有些麻烦，距上次部署博客已经有很长时间了，我也是摸索了几次后才大致掌握，因为不想每次都重新来一遍，所以还是记录一下为好，利人利己。

写博客是一种生活态度，记录并整理生活和编程中的心得和经验并分享，在漫漫人生路上留下自己一路走来的印记，这样以后再回过头来看时，就不会感慨时间都去哪了。如果能因此遇到有共同兴趣爱好的人，那也将会是人生里的不错点缀。

这里只是完成了博客搭建和自动化部署的过程，关于博客的配置和主题的配置以及博客写作的一些技巧会在后面的文章中进行说明，敬请关注～

![微信公众号](https://i.loli.net/2020/05/02/AfHOY5RXge9tlVo.png)