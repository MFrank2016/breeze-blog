---
title: 【Hexo】使用Hexo+github pages+travis ci搭建好看的个人博客（二）
tags:
  - 环境搭建
  - Hexo
categories:
  - 编程
toc: true
date: 2020-05-03 21:29:17
---

## 说明

上一篇里，介绍了使用 `Hexo` + `github pages` + `travis ci` 实现自动化博客部署，我们已经收获了一个属于自己的博客，但现在还比较简陋，里面的设置信息都是默认的数据，所以我们需要把它们改成我们想要的内容。

所以这一篇里，主要介绍 `Hexo` 的配置文件如何设置。通过本篇的学习，你将知道 `Hexo` 配置文件的各个属性是什么意思，并给出我使用的配置，这样你就能随心所欲的进行配置了。

## 站点信息

先来看第一部分，站点信息的配置。先说明一下各个字段的含义：

|参数|	描述|
|--|--|
|title|	网站标题|
|subtitle|	网站副标题|
|description|	网站描述，主要用于 SEO，告诉搜索引擎关于站点的简要信息|
|keywords|	网站的关键词。使用半角逗号, 分隔多个关键词。|
|author|	你的名字|
|language|	网站使用的语言，常见的有 zh-Hans 、zh-CN 、 en|
|timezone|	网站时区。默认使用本地时区。也可以指定其它时区，如 America/New_York, Japan, 和 UTC 。一般的，对于中国大陆地区可以使用 Asia/Shanghai。|

下面是我的配置，可以作为参考：

```yml
# Site
title: 弗兰克的猫
subtitle: '永远年轻，永远热泪盈眶'
description: '铭记过去，拥抱未来，心中有梦，眼里有光'
keywords: 生活,编程,阅读,音乐,电影
author: 清风
language: zh-CN
timezone: ''
```

## 网址信息

网址信息配置主要是设置网站的地址和文章链接格式。

|参数	|描述|	默认值|
|--|--|--|
|url	|网址|	|
|root	|网站根目录|	|
|permalink	|文章的永久链接 |格式	:year/:month/:day/:title/|
|permalink_defaults	|永久链接中各部分的默认值|	|
|pretty_urls	|改写 permalink 的值来美化 URL|	
|pretty_urls.trailing_index	|是否在永久链接中保留尾部的 index.html，设置为 false 时去除|	true|
|pretty_urls.trailing_html	|是否在永久链接中保留尾部的 .html, 设置为 false 时去除 (对尾部的 index.html无效)	|true|

例如：

```yml
# 比如，一个页面的永久链接是 http://example.com/foo/bar/index.html
pretty_urls:
  trailing_index: false
# 此时页面的永久链接会变为 http://example.com/foo/bar/
```

这里通常只需要修改 `url` 和 `root`，如果博客是使用 `github pages` 进行部署的，`url` 配置成对应的博客地址即可。这里需要注意的是 `root` 的值，如果是按照我们上一篇中的方式进行部署的，则需要把 `root` 的值设置为 `/项目名/`。

下面是我的配置：

```yml
# URL
## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
url: https://mfrank2016.github.io/
root: /breeze-blog/
permalink: :year/:month/:day/:title/
permalink_defaults:
pretty_urls:
  trailing_index: true # Set to false to remove trailing 'index.html' from permalinks
  trailing_html: true # Set to false to remove trailing '.html' from permalinks
```

## 目录信息

目录信息是指定各类目录对应的位置，通常不需要修改。

|参数	|描述	|默认值|
|--|--|--|
|source_dir|	资源文件夹，这个文件夹用来存放博客 `md` 等文件。|	source|
|public_dir|	公共文件夹，这个文件夹用于存放生成的站点静态文件。	|public|
|tag_dir|	标签文件夹	|tags|
|archive_dir|	归档文件夹	|archives|
|category_dir|	分类文件夹	|categories|
|code_dir|	`Include code` 文件夹，`source_dir` 下的子目录	|downloads/code|
|i18n_dir|	国际化（i18n）文件夹	|:lang|
|skip_render|	跳过指定文件的渲染。匹配到的文件将会被不做改动地复制到 `public` 目录中。您可使用 `glob` 表达式来匹配路径。	| |

例如：

```yml
skip_render: "mypage/**/*"
# 将会直接将 `source/mypage/index.html` 和 `source/mypage/code.js` 不做改动地输出到 'public' 目录
# 你也可以用这种方法来跳过对指定文章文件的渲染
skip_render: "_posts/test-post.md"
# 这将会忽略对 'test-post.md' 的渲染
```

我的配置如下：

```yml
# Directory
source_dir: source
public_dir: public
tag_dir: tags
archive_dir: archives
category_dir: categories
code_dir: downloads/code
i18n_dir: :lang
skip_render:
```

## 文章配置

这一部分是配置与文章相关的各类属性。

| 参数                    | 描述                                                         | 默认值    |
| :---------------------- | :----------------------------------------------------------- | :-------- |
| `new_post_name`         | 新文章的文件名称                                             | :title.md |
| `default_layout`        | 预设布局                                                     | post      |
| `auto_spacing`          | 在中文和英文之间加入空格                                     | false     |
| `titlecase`             | 把标题转换为 `title case`                                      | false     |
| `external_link`         | 在新标签中打开链接                                           | true      |
| `external_link.enable`  | 在新标签中打开链接                                           | `true`    |
| `external_link.field`   | 对整个网站（`site`）生效或仅对文章（`post`）生效             | `site`    |
| `external_link.exclude` | 需要排除的域名。主域名和子域名如 `www` 需分别配置            | `[]`      |
| `filename_case`         | 把文件名称转换为 (1) 小写或 (2) 大写                         | 0         |
| `render_drafts`         | 显示草稿                                                     | false     |
| `post_asset_folder`     | 启动 `Asset` 文件夹| false     |
| `relative_link`         | 把链接改为与根目录的相对位址                                 | false     |
| `future`                | 显示未来的文章                                               | true      |
| `highlight`             | 代码块的设置                                                 |           |
| `highlight.enable`      | 开启代码块高亮                                               | `true`    |
| `highlight.auto_detect` | 如果未指定语言，则启用自动检测                               | `false`   |
| `highlight.line_number` | 显示行数 | `true`    |
| `highlight.tab_replace` | 用 n 个空格替换 `tabs`；如果值为空，则不会替换 `tabs`            | `''`      |
| `highlight.wrap`        | 把代码块用 `` 包裹 | `true`    |
| `highlight.hljs`        | 为 highlight 的 css 文件中的类添加 `hljs-*` 前缀                      | `false`   |

`auto_spacing` 建议开启，这样的话看起来更美观，`titlecase` 是指关键单词首字母大写，如果不太清楚，可以看下面的说明：

> Titles should be written in title case. This means only using capital letters for the principal words. Articles, conjunctions, and prepositions do not get capital letters unless they start the title. For example:
> The Last of the Mohicans 

`post_asset_folder` 建议开启，这样会在生成新的文章时，同时会同一目录下生成同名文件夹，这样可以把该文章相关的图片等资源放进去，方便引用和管理。

下面是我的配置：

```yml
new_post_name: :title.md # File name of new posts
default_layout: post
auto_spacing: true
titlecase: false # Transform title into titlecase
external_link:
  enable: true # Open external links in new tab
  field: post # Apply to the whole site
  exclude: ''
filename_case: 0
render_drafts: false
post_asset_folder: true
relative_link: false
future: true
# highlight:
#   enable: true
#   line_number: true
#   auto_detect: false
#   tab_replace: ''
#   wrap: true
#   hljs: false
highlight:
  enable: false

# 代码高亮
prism_plugin:
  mode: 'preprocess'    # realtime/preprocess
  theme: 'tomorrow'
  line_number: false    # default false
  custom_css:
```

这里我使用了另一个代码高亮插件，如果通常使用默认的 `hljs` 高亮即可。如果也想要使用这个插件，可以查看[这里](https://github.com/ele828/hexo-prism-plugin)，需要先进行安装：

```bash
npm i -S hexo-prism-plugin
```

## 分类和标签信息

这里配置的是别名，即映射信息，如果文章使用的是英文名分类，这里可以不用设置，如果使用了中文名分类，最好配置一些对应的英文名，否则在对应的分类链接中就会出现 `URL` 编码的中文，比如这样：

```
http://localhost:4000/breeze-blog/categories/programming/life/%E6%B5%8B%E8%AF%95/
```

| 参数               | 描述     | 默认值          |
| :----------------- | :------- | :-------------- |
| `default_category` | 默认分类 | `uncategorized` |
| `category_map`     | 分类别名 |                 |
| `tag_map`          | 标签别名 |                 |

我的配置如下：

```yml
# Category & Tag
default_category: uncategorized
category_map:
  编程: programming
  生活: life
  阅读: reading
  随想: thoughts
  理财: finance
tag_map:
  敏捷开发: agile-development
  环境搭建: environment-building
```

## 日期 / 时间格式

`Hexo` 使用 [Moment.js](http://momentjs.com/) 来解析和显示时间。

| 参数                   | 描述                                                         | 默认值       |
| :--------------------- | :----------------------------------------------------------- | :----------- |
| `date_format`          | 日期格式                                                     | `YYYY-MM-DD` |
| `time_format`          | 时间格式                                                     | `HH:mm:ss`   |
| `use_date_for_updated` | 启用以后，如果 Front Matter 中没有指定 `updated`， `post.updated` 将会使用 `date` 的值而不是文件的创建时间。在 Git 工作流中这个选项会很有用 | `true`       |

我的配置如下：

```yml
# Date / Time format
## Hexo uses Moment.js to parse and display date
## You can customize the date format as defined in
## http://momentjs.com/docs/#/displaying/format/
date_format: YYYY-MM-DD
time_format: HH:mm:ss
## Use post's date for updated date unless set in front-matter
use_date_for_updated: true
```

## 分页信息

| 参数             | 描述                                | 默认值 |
| :--------------- | :---------------------------------- | :----- |
| `per_page`       | 每页显示的文章量 (0 = 关闭分页功能) | `10`   |
| `pagination_dir` | 分页目录                            | `page` |


我的配置如下：

```yml
# Pagination
## Set per_page to 0 to disable pagination
per_page: 12
pagination_dir: page
```

## 扩展信息

| 参数             | 描述                                                         |
| :--------------- | :----------------------------------------------------------- |
| `theme`          | 当前主题名称。值为`false`时禁用主题                          |
| `theme_config`   | 主题的配置文件。在这里放置的配置会覆盖主题目录下的 `_config.yml` 中的配置 |
| `deploy`         | 部署部分的设置                                               |
| `meta_generator` | [Meta generator](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/meta#属性) 标签。 值为 `false` 时 Hexo 不会在头部插入该标签 |

这里设置 `theme` 即可开启对应的主题，具体如何设置会在后面的文章进行详细说明。

`theme_config` 可以在这里配置主题文件里的各个参数进行覆盖，这样就不用维护两份 `config` 文件了，不过个人觉得还是不同主题文件使用不同配置文件比较好。

`deploy` 是部署相关的配置，比如 `git` 部署，除此之外，还有很多其它部署姿势，比如：`Heroku`、`Netlify` 等，但都需要先安装对应的插件。

我的配置如下：

```yml
# Extensions
theme: hexo-theme-matery

# Deployment
deploy:
  type: 'git'
  repo: git@github.com:MFrank2016/breeze-blog
  branch: gh-pages
```

## 包括或不包括目录和文件

在 `Hexo` 配置文件中，通过设置 `include/exclude` 可以让 `Hexo` 进行处理或忽略某些目录和文件夹。可以使用 [glob 表达式](https://github.com/isaacs/minimatch) 对目录和文件进行匹配。

`include` 和 `exclude` 选项都只能应用于 `source/` 文件夹, 但 `ignore` 选项可以应用于所有文件夹。

| 参数      | 描述                                                         |
| :-------- | :----------------------------------------------------------- |
| `include` | `Hexo` 默认会忽略隐藏文件和文件夹（包括名称以下划线和 `.` 开头的文件和文件夹，`Hexo` 的 `_posts` 和 `_data` 等目录除外）。通过设置此字段将使 Hexo 处理他们并将它们复制到 `source` 目录下。 |
| `exclude` | `Hexo` 会忽略这些文件和目录                                    |
| `ignore`  | 忽略文件或文件夹                                         |

举例：

```yml
# Include/Exclude Files/Folders
include:
  - ".nojekyll"
  # 包括 'source/css/_typing.css'
  - "css/_typing.css"
  # 包括 'source/_css/' 中的任何文件，但不包括子目录及其其中的文件。
  - "_css/*"
  # 包含 'source/_css/' 中的任何文件和子目录下的任何文件
  - "_css/**/*"

exclude:
  # 不包括 'source/js/test.js'
  - "js/test.js"
  # 不包括 'source/js/' 中的文件、但包括子目录下的所有目录和文件
  - "js/*"
  # 不包括 'source/js/' 中的文件和子目录下的任何文件
  - "js/**/*"
  # 不包括 'source/js/' 目录下的所有文件名以 'test' 开头的文件，但包括其它文件和子目录下的单文件
  - "js/test*"
  # 不包括 'source/js/' 及其子目录中任何以 'test' 开头的文件
  - "js/**/test*"
  # 不要用 exclude 来忽略 'source/_posts/' 中的文件。你应该使用 'skip_render'，或者在要忽略的文件的文件名之前加一个下划线 '_'
  # 在这里配置一个 - "_posts/hello-world.md" 是没有用的。

ignore:
  # Ignore any folder named 'foo'.
  - "**/foo"
  # Ignore 'foo' folder in 'themes/' only.
  - "**/themes/*/foo"
  # Same as above, but applies to every subfolders of 'themes/'.
  - "**/themes/**/foo"
```

列表中的每一项都必须用单引号或双引号包裹起来。

`include` 和 `exclude` 并不适用于 `themes/` 目录下的文件。如果需要忽略 `themes/` 目录下的部分文件或文件夹，可以使用 `ignore` 或在文件名之前添加下划线 `_`。

![微信公众号](https://i.loli.net/2020/05/02/AfHOY5RXge9tlVo.png)











