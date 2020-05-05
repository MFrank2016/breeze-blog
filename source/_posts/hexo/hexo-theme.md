---
title: 【Hexo】使用Hexo+github pages+travis ci搭建好看的个人博客（三）
tags:
  - 环境搭建
  - Hexo
categories:
  - 编程
toc: true
date: 2020-05-04 10:40:14
---

## 说明

前两篇文章介绍了 `Hexo` + `github pages` + `travis ci` 进行自动化部署，并介绍了 `Hexo` 的配置文件中的各个属性，相信通过前两篇文章的学习，你已经学会了如何搭建自己的博客，并能够根据自己的需要进行个性化配置。

这一篇将以 `Matery` 这款主题为例，说明一下主题应该如何配置。包括主题配置、插件设置、注意事项等。

## 设置博客主题

先到[这里](https://hexo.io/themes/) 选择你喜欢的主题，点击它的标题（注意，点图片是进去 *demo* 站点，点标题才是跳转到对应到 *github* 仓库），跳转到 *github* 仓库，复制其仓库地址。

比如我使用到主题是 `Matery` ，其项目地址为：`git@github.com:blinkfox/hexo-theme-matery.git`。

打开本地目录，来到与我们的博客项目同级的目录（注意是同级，不是在博客项目内部），将主题项目克隆到本地。

```bash
git clone git@github.com:blinkfox/hexo-theme-matery.git
```

在博客项目下的 *themes* 文件夹里新建一个叫 *matery* 的文件夹，然后将主题文件复制到这个文件夹里：

```bash
mkdir breeze-blog/themes/matery
cp -r hexo-theme-matery/* breeze-blog/themes/matery
```

这里的 *breeze-blog* 是我的博客项目所在的文件夹，这里替换成你的即可。

再次打开我们的配置文件(*_config.yml*)，修改 *theme* 属性，设置为博客主题所在的文件夹名，这里即为 *matery*，注意要与文件夹名称完全一致。

然后我们重新生成一下博客静态文件：

```bash
cd breeze-blog
hexo clean && hexo generate
hexo server
```

然后再打开我们的博客地址：`localhost:4000`，我们的主题便设置好了。

![hexo-theme-1.jpg](https://i.loli.net/2020/05/04/YKwFax1NrtVL6Zo.jpg)

但现在大部分信息都是默认数据，所以我们需要根据需要进行自定义设置，不同主题的配置文件都不太一样，因此设置方法也有所不同，这里仅介绍 `Matery` 主题的设置方法。

## 主题内容自定义

### 新建页面

如果你点击首页最上面的那一栏，会发现很多页面打开是没有的，因为我们还没有创建对应的页面，所以需要先创建对应的页面。

![hexo-theme-2.jpg](https://i.loli.net/2020/05/04/9o7sjVdBMcm24TP.jpg)

先新建分类 `categories` 页，`categories` 页是用来展示所有分类的页面，命令如下：

```bash
hexo new page "categories"
```

编辑新建的页面文件 */source/categories/index.md*，写入以下内容：

```
---
title: categories
date: 2020-05-04 10:40:13
type: "categories"
layout: "categories"
---
```

然后新建标签 `tags` 页，`tags` 页是用来展示所有标签的页面，命令如下：

```bash
hexo new page "tags"
```

编辑新建的页面文件 */source/tags/index.md*，写入以下内容：

```
---
title: tags
date: 2020-05-04 10:40:14
type: "tags"
layout: "tags"
---
```

接下来是新建 `about` 页，`about` 页是用来展示关于我和我的博客信息的页面，命令如下：

```bash
hexo new page "about"
```

编辑新建的页面文件 `/source/about/index.md`，写入以下内容：

```
---
title: about
date: 2020-05-04 10:40:15
type: "about"
layout: "about"
---
```

然后新建留言板 `contact` 页，`contact` 页是用来展示留言板信息的页面，方便其他人进行统一留言，命令如下：

```bash
hexo new page "contact"
```

编辑新建的页面文件 */source/contact/index.md*，写入以下内容：

```
---
title: contact
date: 2020-05-04 10:40:16
type: "contact"
layout: "contact"
---
```

注意，留言板功能依赖于第三方评论系统，需要先激活评论系统才有效果，如果嫌麻烦不想使用，也可以不处理，后面在菜单栏里去掉这一选项即可。

最后，新建友情链接 `friends` 页，`friends` 页是用来展示友情链接信息的页面，命令如下：

```bash
hexo new page "friends"
```

编辑新建的页面文件 */source/friends/index.md*，写入以下内容：

```
---
title: friends
date: 2020-05-04 10:40:17
type: "friends"
layout: "friends"
---
```

同时，在你的博客 `source` 目录下新建 *_data* 目录，在 *_data* 目录中新建 *friends.json* 文件，文件内容如下所示：

```json
[{
    "avatar": "https://mfrank2016.github.io/breeze-blog/medias/avatar.jpg",
    "name": "清风",
    "introduction": "技术博主，文章还不错",
    "url": "https://mfrank2016.github.io/breeze-blog/",
    "title": "前去学习"
}, {
    "avatar": "https://draveness.me/images/draven-logo.png",
    "name": "真没什么逻辑",
    "introduction": "面向信仰编程",
    "url": "https://draveness.me/",
    "title": "前去学习"
}]
```

这里可以根据需要进行删减，当然，要看效果可以先这样设置，之后再来修改也不迟。

新建完页面后，我们再重新生成一下博客静态页，然后运行一下项目，便能看到效果了。这里标签页和分类页都只会展示现有博客的标签和分类数据，所以如果还没有文章设置标题或者分类，那么对应页面是没有数据的。要想看到效果，就得先写几篇文章。

### 菜单导航配置

#### 配置基本菜单导航的名称、路径url和图标icon.

1.菜单导航名称可以是中文也可以是英文(如：`Index`或`主页`) 2.图标icon 可以在[Font Awesome](https://fontawesome.com/icons) 中查找

```yml
menu:
  Index:
    url: /
    icon: fas fa-home
  Tags:
    url: /tags
    icon: fas fa-tags
  Categories:
    url: /categories
    icon: fas fa-bookmark
  Archives:
    url: /archives
    icon: fas fa-archive
  About:
    url: /about
    icon: fas fa-user-circle
  Friends:
    url: /friends
    icon: fas fa-address-book
```

#### 二级菜单配置方法

如果你需要二级菜单则可以在原基本菜单导航的基础上如下操作
1.在需要添加二级菜单的一级菜单下添加`children`关键字(如:`About`菜单下添加`children`)
2.在`children`下创建二级菜单的 名称name,路径url和图标icon.
3.注意每个二级菜单模块前要加 `-`.
4.注意缩进格式

```yml
menu:
  Index:
    url: /
    icon: fas fa-home
  Tags:
    url: /tags
    icon: fas fa-tags
  Categories:
    url: /categories
    icon: fas fa-bookmark
  Archives:
    url: /archives
    icon: fas fa-archive
  About:
    url: /about
    icon: fas fa-user-circle-o
  Friends:
    url: /friends
    icon: fas fa-address-book
  Medias:
    icon: fas fa-list
    children:
      - name: Musics
        url: /musics
        icon: fas fa-music
      - name: Movies
        url: /movies
        icon: fas fa-film
      - name: Books
        url: /books
        icon: fas fa-book
      - name: Galleries
        url: /galleries
        icon: fas fa-image
```

执行 `hexo clean && hexo g` 重新生成博客文件，然后就可以在文章中对应位置看到你用`emoji`语法写的表情了。

### 代码高亮

由于 Hexo 自带的代码高亮主题显示不好看，所以主题中使用到了 [hexo-prism-plugin](https://github.com/ele828/hexo-prism-plugin) 的 Hexo 插件来做代码高亮，安装命令如下：

```bash
npm i -S hexo-prism-plugin
```

然后，修改 Hexo 根目录下 `_config.yml` 文件中 `highlight.enable` 的值为 `false`，并新增 `prism` 插件相关的配置，主要配置如下：

```yml
highlight:
  enable: false

prism_plugin:
  mode: 'preprocess'    # realtime/preprocess
  theme: 'tomorrow'
  line_number: false    # default false
  custom_css:
```

### 搜索

主题中还使用到了 [hexo-generator-search](https://github.com/wzpan/hexo-generator-search) 的 Hexo 插件来做内容搜索，安装命令如下：

```bash
npm install hexo-generator-search --save
```

在 Hexo 根目录下的 `_config.yml` 文件中，新增以下的配置项：

```yml
search:
  path: search.xml
  field: post
```

效果图如下：

![hexo-theme-3.jpg](https://i.loli.net/2020/05/04/RrE3pQdq9CmtcTP.jpg)

### 中文链接转拼音（建议安装）

如果你的文章名称是中文的，那么 *Hexo* 默认生成的永久链接也会有中文，这样不利于 `SEO`，且 `gitment` 评论对中文链接也不支持。我们可以用 [hexo-permalink-pinyin](https://github.com/viko16/hexo-permalink-pinyin) *Hexo* 插件使在生成文章时生成中文拼音的永久链接。

安装命令如下：

```bash
npm i hexo-permalink-pinyin --save
```

在 *Hexo* 根目录下的 `_config.yml` 文件中，新增以下的配置项：

```yml
permalink_pinyin:
  enable: true
  separator: '-' # default: '-'
```

### 文章字数统计插件（建议安装）

如果你想要在文章中显示文章字数、阅读时长信息，可以安装 [hexo-wordcount](https://github.com/willin/hexo-wordcount)插件。

安装命令如下：

```bash
npm i --save hexo-wordcount
```

然后只需在本主题下的 `_config.yml` 文件中，将各个文章字数相关的配置激活即可：

```yml
postInfo:
  date: true
  update: true
  wordCount: true # 设置文章字数统计为 true.
  totalCount: true # 设置站点文章总字数统计为 true.
  min2read: true # 阅读时长.
  readCount: true # 阅读次数.
```

### 添加emoji表情支持（可选的）

*Matery* 主题新增了对`emoji`表情的支持，使用到了 [hexo-filter-github-emojis](https://npm.taobao.org/package/hexo-filter-github-emojis) 的 Hexo 插件来支持 `emoji`表情的生成，把对应的`markdown emoji`语法（`::`,例如：`:smile:`）转变成会跳跃的`emoji`表情，安装命令如下：

```bash
npm install hexo-filter-github-emojis --save
```

在 *Hexo* 根目录下的 `_config.yml` 文件中，新增以下的配置项：

```yml
githubEmojis:
  enable: true
  className: github-emoji
  inject: true
  styles:
  customEmojis:
```

### 添加 RSS 订阅支持（可选的）

主题中还使用到了 [hexo-generator-feed](https://github.com/hexojs/hexo-generator-feed) 的 Hexo 插件来做 `RSS`，安装命令如下：

```
npm install hexo-generator-feed --save
```

在 *Hexo* 根目录下的 `_config.yml` 文件中，新增以下的配置项：

```yml
feed:
  type: atom
  path: atom.xml
  limit: 20
  hub:
  content:
  content_limit: 140
  content_limit_delim: ' '
  order_by: -date
```

执行 `hexo clean && hexo g` 重新生成博客文件，然后在 `public` 文件夹中即可看到 `atom.xml` 文件，说明你已经安装成功了。

### 添加 [DaoVoice](http://www.daovoice.io/) 在线聊天功能（可选的）

前往 [DaoVoice](http://www.daovoice.io/) 官网注册并且获取 `app_id`，并将 `app_id` 填入主题的 `_config.yml` 文件中。

### 添加 [Tidio](https://www.tidio.com/) 在线聊天功能（可选的）

前往 [Tidio](https://www.tidio.com/) 官网注册并且获取 `Public Key`，并将 `Public Key` 填入主题的 `_config.yml` 文件中。

### 修改社交链接

在主题的 `_config.yml` 文件中，默认支持 `QQ`、`GitHub` 和邮箱等的配置，你可以在主题文件的 `/layout/_partial/social-link.ejs` 文件中，新增、修改你需要的社交链接地址，增加链接可参考如下代码：

```
<% if (theme.socialLink.github) { %>
    <a href="<%= theme.socialLink.github %>" class="tooltipped" target="_blank" data-tooltip="访问我的GitHub" data-position="top" data-delay="50">
        <i class="fab fa-github"></i>
    </a>
<% } %>
```

其中，社交图标（如：`fa-github`）你可以在 [Font Awesome](https://fontawesome.com/icons) 中搜索找到。以下是常用社交图标的标识，供你参考：

- Facebook: `fab fa-facebook`
- Twitter: `fab fa-twitter`
- Google-plus: `fab fa-google-plus`
- Linkedin: `fab fa-linkedin`
- Tumblr: `fab fa-tumblr`
- Medium: `fab fa-medium`
- Slack: `fab fa-slack`
- Sina Weibo: `fab fa-weibo`
- Wechat: `fab fa-weixin`
- QQ: `fab fa-qq`
- Zhihu: `fab fa-zhihu`

### 修改打赏的二维码图片

在主题文件的 `source/medias/reward` 文件中，可以替换成你的微信和支付宝的打赏二维码图片。

### 配置音乐播放器（可选的）

要支持音乐播放，在主题的 `_config.yml` 配置文件中激活music配置即可：

```yml
# 是否在首页显示音乐
music:
  enable: true
  title:     	    #非吸底模式有效
    enable: true
    show: 听听音乐
  server: netease   #require music platform: netease, tencent, kugou, xiami, baidu
  type: playlist    #require song, playlist, album, search, artist
  id: 503838841     #require song id / playlist id / album id / search keyword
  fixed: false      # 开启吸底模式
  autoplay: false   # 是否自动播放
  theme: '#42b983'
  loop: 'all'       # 音频循环播放, 可选值: 'all', 'one', 'none'
  order: 'random'   # 音频循环顺序, 可选值: 'list', 'random'
  preload: 'auto'   # 预加载，可选值: 'none', 'metadata', 'auto'
  volume: 0.7       # 默认音量，请注意播放器会记忆用户设置，用户手动设置音量后默认音量即失效
  listFolded: true  # 列表默认折叠
```

> `server`可选`netease`（网易云音乐），`tencent`（QQ音乐），`kugou`（酷狗音乐），`xiami`（虾米音乐），
>
> `baidu`（百度音乐）。
>
> `type`可选`song`（歌曲），`playlist`（歌单），`album`（专辑），`search`（搜索关键字），`artist`（歌手）
>
> `id`获取示例: 打开手机版网易云音乐，选择喜欢的歌单，然后点击分享

我这里随便选了一个歌单，分享后的文字长这样：

> 分享真咸鱼饼干的歌单《青年节：致逐梦人，有志者事竟成》http://music.163.com/playlist/4965675848/1548006936/?userid=120124365 (@网易云音乐)

`4965675848` 这就是歌单的id，文件里默认设置的歌单其实也还不错，歌挺多的，所以如果没什么特殊要求，使用默认歌单也不错。

⚠️这里需要注意一点，如果你想要替换成自己的歌单时，会发现，后续歌单的更新是不会影响到它的，这也是我捣鼓半天才发现的，音乐插件使用的是 *Aplayer* 播放器，在 *https://cdn.jsdelivr.net/npm/meting@2/dist/Meting.min.js* 这个文件里可以看到，数据是从这个接口获取的：*https://api.i-meto.com/meting/api?server=:server&type=:type&id=:id&r=:r*，猜测服务端是直接将获取到的榜单列表写进里数据库，但后续不会进行更新，所以导致无论多少次刷新，都只能获取第一次取到的数据。

所以有两种解决办法，第一种是创建新歌单，然后一次性添加足够多的歌，然后在配置文件中替换成你的歌单id，另一种是自己写一个网易云音乐歌单解析接口，来获取网易云音乐数据。我比较懒，所以选择了第一种方案，23333，还在充实歌单中。

## 文章 Front-matter 介绍

### Front-matter 选项详解

`Front-matter` 选项中的所有内容均为**非必填**的。但建议至少填写 `title` 和 `date` 的值。

| 配置选项      | 默认值                         | 描述                                                         |
| ------------- | ------------------------------ | ------------------------------------------------------------ |
| title         | `Markdown` 的文件标题          | 文章标题，强烈建议填写此选项                                 |
| date          | 文件创建时的日期时间           | 发布时间，强烈建议填写此选项，且最好保证全局唯一             |
| author        | 根 `_config.yml` 中的 `author` | 文章作者                                                     |
| img           | `featureImages` 中的某个值     | 文章特征图，推荐使用图床(腾讯云、七牛云、又拍云等)来做图片的路径.如: `http://xxx.com/xxx.jpg` |
| top           | `true`                         | 推荐文章（文章是否置顶），如果 `top` 值为 `true`，则会作为首页推荐文章 |
| cover         | `false`                        | `v1.0.2`版本新增，表示该文章是否需要加入到首页轮播封面中     |
| coverImg      | 无                             | `v1.0.2`版本新增，表示该文章在首页轮播封面需要显示的图片路径，如果没有，则默认使用文章的特色图片 |
| password      | 无                             | 文章阅读密码，如果要对文章设置阅读验证密码的话，就可以设置 `password` 的值，该值必须是用 `SHA256` 加密后的密码，防止被他人识破。前提是在主题的 `config.yml` 中激活了 `verifyPassword` 选项 |
| toc           | `true`                         | 是否开启 TOC，可以针对某篇文章单独关闭 TOC 的功能。前提是在主题的 `config.yml` 中激活了 `toc` 选项 |
| mathjax       | `false`                        | 是否开启数学公式支持 ，本文章是否开启 `mathjax`，且需要在主题的 `_config.yml` 文件中也需要开启才行 |
| summary       | 无                             | 文章摘要，自定义的文章摘要内容，如果这个属性有值，文章卡片摘要就显示这段文字，否则程序会自动截取文章的部分内容作为摘要 |
| categories    | 无                             | 文章分类，本主题的分类表示宏观上大的分类，只建议一篇文章一个分类 |
| tags          | 无                             | 文章标签，一篇文章可以多个标签                               |
| keywords      | 文章标题                       | 文章关键字，SEO 时需要                                       |
| reprintPolicy | cc_by                          | 文章转载规则， 可以是 cc_by, cc_by_nd, cc_by_sa, cc_by_nc, cc_by_nc_nd, cc_by_nc_sa, cc0, noreprint 或 pay 中的一个 |

> **注意**:
>
> 1. 如果 `img` 属性不填写的话，文章特色图会根据文章标题的 `hashcode` 的值取余，然后选取主题中对应的特色图片，从而达到让所有文章都的特色图**各有特色**。
> 2. `date` 的值尽量保证每篇文章是唯一的，因为本主题中 `Gitalk` 和 `Gitment` 识别 `id` 是通过 `date` 的值来作为唯一标识的。
> 3. 如果要对文章设置阅读验证密码的功能，不仅要在 Front-matter 中设置采用了 SHA256 加密的 password 的值，还需要在主题的 `_config.yml` 中激活了配置。有些在线的 SHA256 加密的地址，可供你使用：[开源中国在线工具](http://tool.oschina.net/encrypt?type=2)、[chahuo](http://encode.chahuo.com/)、[站长工具](http://tool.chinaz.com/tools/hash.aspx)。
> 4. 您可以在文章md文件的 front-matter 中指定 reprintPolicy 来给单个文章配置转载规则

以下为文章的 `Front-matter` 示例。

最简示例

```
---
title: typora-vue-theme主题介绍
date: 2018-09-07 09:25:00
---
```

最全示例

```
---
title: typora-vue-theme主题介绍
date: 2018-09-07 09:25:00
author: 赵奇
img: /source/images/xxx.jpg
top: true
cover: true
coverImg: /images/1.jpg
password: 8d969eef6ecad3c29a3a629280e686cf0c3f5d5a86aff3ca12020c923adc6c92
toc: false
mathjax: false
summary: 这是你自定义的文章摘要内容，如果这个属性有值，文章卡片摘要就显示这段文字，否则程序会自动截取文章的部分内容作为摘要
categories: Markdown
tags:
  - Typora
  - Markdown
---
```

上述大部分内容都来自主题说明文档，只是添加了一下我觉得需要注意的地方。这里还有一些没有说到到的属性，这配置文件里都有详细的介绍，确实不需要过多解释。

最后再介绍一下我折腾比较久的插件，valine 评价插件。

## Valine 评价插件

插件主页：*https://valine.js.org/* ，上面有详细的介绍，可以查看[这里](https://valine.js.org/quickstart.html)，注册后，验证邮箱，绑定手机号，然后新建一个应用，获取到对应的 `AppId` 和 `AppKey`，然后写回到主题文件下到 `_config.yml` 文件里，但是要**注意一点，不要直接使用国内版进行注册，而要用国际版，否则无法申请二级域名**。

```yml
valine:
  enable: true
  appId: XXX
  appKey: XXX
  notify: true
  verify: false
  visitor: true
  avatar: 'retro' # Gravatar style : mm/identicon/monsterid/wavatar/retro/hide
  pageSize: 10
  placeholder: 'Comment here' # Comment Box placeholder
  background: /medias/comment_bg.png
```

最新版的 `Valine` 已经移除了邮件通知功能。如果没有邮件通知，很可能别人评价之后，你却毫不知情，后续回复对方也收不到。因此，需要再配置一个插件来实现， ++https://github.com/zhaojun1998/Valine-Admin++ 。在配置这个插件之前，需要确保 `Valine` 可以正常工作，可以自己给自己评论一下进行测试。

配置好之后，别人在你的文章下评论后你便能收到邮件通知了。

至此，`Matery` 主题就搭建配置完成了，接下来就只需要安心写博客了～

![微信公众号](https://i.loli.net/2020/05/02/AfHOY5RXge9tlVo.png)