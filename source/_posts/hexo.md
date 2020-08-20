---
title: hexo
date: 2020-05-05 15:02:23
cover: true
top: true
summary:  Hexo是一款快速、简洁且高效的基于Node.js的静态博客框架
tags:
  - 静态建站
  - hexo
categories: 前端
---

转载来自于[https://github.com/blinkfox/hexo-theme-matery]

# hexo

## Hexo介绍

[Hexo](https://hexo.io/zh-cn/)是一款快速、简洁且高效的基于`Node.js`的静态博客框架，四大特性：

- 超快速度：`Node.js` 所带来的超快生成速度，让上百个页面在几秒内瞬间完成渲染。
- 支持Markdown：`Hexo` 支持 `GitHub Flavored Markdown` 的所有功能，甚至可以整合 `Octopress` 的大多数插件。
- 一键部署：只需一条指令即可部署到 `GitHub Pages`, `Heroku`或其他平台。
- 插件和可扩展性：强大的 `API` 带来无限的可能，与数种模板引擎`（EJS，Pug，Nunjucks）`和工具`（Babel，PostCSS，Less/Sass）`轻易集成。

这使得很多非编程人员可以很轻松，很自由的定制博客。废话不多说，开始进入搭建环境把。



## 准备

### Linux

直接命令行输入：

```
sudo apt-get install nodejs
sudo apt-get install npm
```

或者到[官网](http://nodejs.cn/download/)下载：

下载完成后解压到指定文件夹，然后配置环境变量（目的是为了在终端可以任意位置使用它）：

首先打开`~/.bashrc`文件

在文件的最下端填写如下代码

```
export PATH=${PATH}:$HOME/node-v12.13.0-linux-x64/bin/
```

因为我下载的是`64`位`12.13.0`版本，并且放到了根目录`home`下，你可以根据自己的需求进行更改上面的路径。保存退出后，执行命令让修改生效。

```shell
source ~/.bashrc
```

然后在终端输入`npm -v`和`node -v`验证是否安装配置成功

```shell
$npm -v
6.13.0

$node -v
v12.13.0
```

### Windows

下载稳定版或者最新版都可以[Node.js](http://nodejs.cn/download/)，安装选项全部默认，一路点击`Next`。最后安装好之后，按`Win+R`打开命令提示符，输入`node -v`和`npm -v`，如果出现版本号，那么就安装成功了。

### npm加速

一般国内通过`npm`下载东西会比较慢，所以需要添加阿里的源进行加速。

```shell
npm config set registry https://registry.npm.taobao.org
```

### 安装Git

为了把本地的网页文件上传到`Github`上面去，我们需要用到分布式版本控制工具 `git`。关于`git`和`Github`这里就不多介绍了。同样分为两个版本：

#### Linux

在Linux平台比较方便，直接使用命令就可以安装：

```shell
sudo apt-get install git
```

安装完成后即可享用。

#### Windows

需要去官网下载[Git](https://git-scm.com/download/win)，下载完成后按照向导安装即可。

> 注意：在安装的最后一步添加路径时选择 Use Git from the Windows Command Prompt 。这是把Git添加到了环境变量中，以便可以在cmd中使用。而本人推荐使用下载附带的git bash进行操作，比较方便。

对于git的讲解和使用，大家可以自行到网上查找。`Hexo`搭建的过程中，已经封装好一个git命令，可以直接使用`hexo`的命令将生成的静态网站代码同步到`github`的仓库里。但是如果想要自己同步源码的话，那么就需要掌握一下git命令了。在这里我只列举一下常用的命令：

```shell
git init #初始化一个git库，生成.git文件夹，里面保存的是该git库的记录和配置
git remote add origin 远程仓库地址 #将本地仓库和远程仓库链接起来
git pull #同步代码
git status #检查本地仓库修改状态
git add 文件名 或者 git add .  #将本地修改的文件加入缓存
git commit 文件名 -m "描述" 或者 git commit . -m "描述"  #提交缓存，并描述该提交
git push -u origin code 
# 将本地的提交推送到远程仓库.-u是代表输入账号密码，如果你已经配置了git的公钥，那么可直接push.
```



### 注册Github

`Git`安装完成之后就可以去[Github](https://github.com/)上注册账号并创建仓库， 用来存放我们的网站了。

> Github是基于 Git 做版本控制的代码托管平台，同时也是全球最大的代（同）码（性）托（交）管（友）网站。

创建完账户之后新建一个项目仓库`New repository`，如下所示

接着输入仓库名，后面一定要加`.github.io`后缀，README初始化也要勾上。 如下图配置（因为我的已经存在相同的仓库，所以报错）

> 要创建一个和你用户名相同的仓库，后面加.github.io，只有这样将来要部署到GitHub page的时候，才会被识别，也就是[http://xxxx.github.io，其中xxx就是你注册`GitHub`的用户名](http://xxxx.github.xn--io%2Cxxx`github`-2o22as9ojum8na498bvs1c3q5altvxp5b8jyclii/)



然后项目就建成了，点击`Settings`，向下拉到最后有个`GitHub Pages`，点击`Choose a theme`选择一个主题。然后等一会儿，再回到`GitHub Pages`，点击新出来的链接，就会进入到`github page`的界面。看到这个界面就说明`Github`的`page`已经可以使用了，接下来我们进入`Hexo`的搭建。

## 搭建

### 安装Hexo

首先创建一个文件夹，名字自取如`YoungBlog`，用来存放自己的博客文件，然后`cd`到这个文件夹下（或者在这个文件夹下直接右键`git bash`打开）。在该目录下输入如下命令安装`Hexo`：

```shell
npm install -g hexo-cli
```

接下来初始化一下`hexo`,即初始化我们的网站，

```shell
hexo init
```

> 初始化要求必须是空的目录下进行。

接着输入`npm install`安装必备的组件。

初始化完成后会在目下生成几个文件和文件夹，这些就是我们需要编写的网站源码了：

- `node_modules:` 依赖包，npm安装的一些插件存放的文件夹。
- `public：`存放生成的页面，网站正式展示的内容。
- `scaffolds：`生成文章和页面的一些模板。
- `source：`用来存放你的文章和数据。
- `themes：`主题存放文件夹。
- `_config.yml:` 博客的配置文件，非主题的配置。
- `db.json`：博客的版本信息等。
- `package.json`和`package-lock.json`：依赖包和版本信息。

这样本地的网站配置也弄好啦，输入`hexo g`生成静态网页，然后输入`hexo s`打开本地服务器，然后浏览器打开[http://localhost:4000](http://localhost:4000/)就可以看到我们的博客啦，效果如下：

这里介绍一下`Hexo`常用的几个命令：

```shell
hexo clean #清除db和public文件下的内容，或可写成hexo cl
hexo g #根据源码生成静态文件
hexo s #开启本地的server，这样可在本地通过localhost:4000访问博客。或可写成hexo server
hexo d #部署网站的静态文件到配置好的托管网站，如Github或者Coding，配置在_config中的Deploy。
#后续如果安装了一些插件，可能导致缩写无法使用，所以hexo d也可以写成hexo deploy。
```

看完展示后，可以按`ctrl+c`关闭本地服务器。

### 部署到Github

首先要安装一个插件，用于`Hexo`部署代码的。

```
npm i hexo-deployer-git
```

安装完成之后，在`_config.yml`配置文件中加入如下代码，这样我们在使用`hexo d`的时候就可以直接部署到`Github`上了，如果你想部署到其他平台（支持`Git`），也可以添加到这里。

```
deploy:
  type: git
  repository: https://github.com/daiyizheng123/daiyizheng.github.io.git
  branch: master
```

> 如果不了解git那么请先自行百度学习一下git的相关配置。

`Git`分为无密推送和需要输入账户密码推送。无密码推送就是需要在本地生成公钥，然后添加到代码托管平台如`Github`，这样在推送时候就不需要输入账户密码了。而反之的话，每次推送就会要求你输入账户密码。下面说一下无密推送的配置过程。

首先打开`Git bash`，输入如下内容：

```shell
git config --global user.name "你的用户名"
git config --global user.email "你的邮箱"
```

用户名和邮箱根据你注册`github`的信息自行修改。

然后生成密钥SSH key：

```shell
ssh-keygen -t rsa -C "你的邮箱"
```

这个时候它会告诉你已经生成了`.ssh`的文件夹。在你的电脑中找到这个文件夹。或者`git bash`中输入

```shell
cat ~/.ssh/id_rsa.pub
```

打开[github](http://github.com/)，在头像下面点击`settings`，再点击`SSH and GPG keys`，新建一个`SSH`，名字随便取一个都可以，把你的`id_rsa.pub`里面的信息复制进去。

这样你的电脑就跟`Github`建立起的安全联系，以后推送代码就不需要输入密码了。

> 注意：这里使用hexo d推送代码，推送的是编译完成的静态文件，也就是上面说的public文件夹下的代码，而不是网站的源代码。



## 插入音乐/视频

Hexo博客插入视频、音频有三种方法：

### Hexo博客利用iframe标签插入网易音乐方法

```xml
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=430 height=86 src="//music.163.com/outchain/player?type=2&id=114389&auto=0&height=66"></iframe>
```

### Hexo博客利用embed 标签插入音乐和视频

```xml
<embed height="415" width="544" quality="high" allowfullscreen="true" type="application/x-shockwave-flash" src="//static.hdslb.com/miniloader.swf" flashvars="aid=8506694&page=1" pluginspage="//www.adobe.com/shockwave/download/download.cgi?P1_Prod_Version=ShockwaveFlash"></embed>
```

### Hexo博客利用Hexo插件插入音乐/视频

- npm install hexo-tag-aplayer

  ```bash
  {% aplayer "她的睫毛" "周杰伦" "http://home.ustc.edu.cn/~mmmwhy/%d6%dc%bd%dc%c2%d7%20-%20%cb%fd%b5%c4%bd%de%c3%ab.mp3"  "http://home.ustc.edu.cn/~mmmwhy/jay.jpg" "autoplay=false" %}
  ```

- npm install hexo-tag-dplayer

  ```bash
  {% dplayer "url=http://home.ustc.edu.cn/~mmmwhy/GEM.mp4"  "pic=http://home.ustc.edu.cn/~mmmwhy/GEM.jpg" "loop=yes" "theme=#FADFA3" "autoplay=false" "token=tokendemo" %}
  ```

### blibi视频嵌入代码

```js
<iframe src="//player.bilibili.com/player.html?aid=73657563&bvid=BV1HE411Y7G9&cid=125986571&page=2" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"  width="680" height="520" align="center"> </iframe>
```

### 切换主题

修改 Hexo 根目录下的 `_config.yml` 的 `theme` 的值：`theme: hexo-theme-matery`

#### `_config.yml` 文件的其它修改建议:

- 请修改 `_config.yml` 的 `url` 的值为你的网站主 `URL`（如：`http://xxx.github.io`）。
- 建议修改两个 `per_page` 的分页条数值为 `6` 的倍数，如：`12`、`18` 等，这样文章列表在各个屏幕下都能较好的显示。
- 如果你是中文用户，则建议修改 `language` 的值为 `zh-CN`。

### 新建分类 categories 页

`categories` 页是用来展示所有分类的页面，如果在你的博客 `source` 目录下还没有 `categories/index.md` 文件，那么你就需要新建一个，命令如下：

```
hexo new page "categories"
```

编辑你刚刚新建的页面文件 `/source/categories/index.md`，至少需要以下内容：

```
---
title: categories
date: 2018-09-30 17:25:30
type: "categories"
layout: "categories"
---
```

### 新建标签 tags 页

`tags` 页是用来展示所有标签的页面，如果在你的博客 `source` 目录下还没有 `tags/index.md` 文件，那么你就需要新建一个，命令如下：

```
hexo new page "tags"
```

编辑你刚刚新建的页面文件 `/source/tags/index.md`，至少需要以下内容：

```
---
title: tags
date: 2018-09-30 18:23:38
type: "tags"
layout: "tags"
---
```

### 新建关于我 about 页

`about` 页是用来展示**关于我和我的博客**信息的页面，如果在你的博客 `source` 目录下还没有 `about/index.md` 文件，那么你就需要新建一个，命令如下：

```
hexo new page "about"
```

编辑你刚刚新建的页面文件 `/source/about/index.md`，至少需要以下内容：

```
---
title: about
date: 2018-09-30 17:25:30
type: "about"
layout: "about"
---
```

### 新建留言板 contact 页（可选的）

`contact` 页是用来展示**留言板**信息的页面，如果在你的博客 `source` 目录下还没有 `contact/index.md` 文件，那么你就需要新建一个，命令如下：

```
hexo new page "contact"
```

编辑你刚刚新建的页面文件 `/source/contact/index.md`，至少需要以下内容：

```
---
title: contact
date: 2018-09-30 17:25:30
type: "contact"
layout: "contact"
---
```

> **注**：本留言板功能依赖于第三方评论系统，请**激活**你的评论系统才有效果。并且在主题的 `_config.yml` 文件中，第 `19` 至 `21` 行的“**菜单**”配置，取消关于留言板的注释即可。

### 新建友情链接 friends 页（可选的）

`friends` 页是用来展示**友情链接**信息的页面，如果在你的博客 `source` 目录下还没有 `friends/index.md` 文件，那么你就需要新建一个，命令如下：

```
hexo new page "friends"
```

编辑你刚刚新建的页面文件 `/source/friends/index.md`，至少需要以下内容：

```
---
title: friends
date: 2018-12-12 21:25:30
type: "friends"
layout: "friends"
---
```

同时，在你的博客 `source` 目录下新建 `_data` 目录，在 `_data` 目录中新建 `friends.json` 文件，文件内容如下所示：

```
[{
    "avatar": "http://image.luokangyuan.com/1_qq_27922023.jpg",
    "name": "码酱",
    "introduction": "我不是大佬，只是在追寻大佬的脚步",
    "url": "http://luokangyuan.com/",
    "title": "前去学习"
}, {
    "avatar": "http://image.luokangyuan.com/4027734.jpeg",
    "name": "闪烁之狐",
    "introduction": "编程界大佬，技术牛，人还特别好，不懂的都可以请教大佬",
    "url": "https://blinkfox.github.io/",
    "title": "前去学习"
}, {
    "avatar": "http://image.luokangyuan.com/avatar.jpg",
    "name": "ja_rome",
    "introduction": "平凡的脚步也可以走出伟大的行程",
    "url": "https://me.csdn.net/jlh912008548",
    "title": "前去学习"
}]
```

### 新建 404 页

如果在你的博客 `source` 目录下还没有 `404.md` 文件，那么你就需要新建一个

编辑你刚刚新建的页面文件 `/source/404.md`，至少需要以下内容：

```
---
title: 404
date: 2018-09-30 17:25:30
type: "404"
layout: "404"
description: "Oops～，我崩溃了！找不到你想要的页面 :("
---
```

### 菜单导航配置

#### 配置基本菜单导航的名称、路径url和图标icon.

1.菜单导航名称可以是中文也可以是英文(如：`Index`或`主页`) 2.图标icon 可以在[Font Awesome](https://fontawesome.com/icons) 中查找

```
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

```
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

```
npm i -S hexo-prism-plugin
```

然后，修改 Hexo 根目录下 `_config.yml` 文件中 `highlight.enable` 的值为 `false`，并新增 `prism` 插件相关的配置，主要配置如下：

```
highlight:
  enable: false

prism_plugin:
  mode: 'preprocess'    # realtime/preprocess
  theme: 'tomorrow'
  line_number: false    # default false
  custom_css:
```

### 搜索

本主题中还使用到了 [hexo-generator-search](https://github.com/wzpan/hexo-generator-search) 的 Hexo 插件来做内容搜索，安装命令如下：

```
npm install hexo-generator-search --save
```

在 Hexo 根目录下的 `_config.yml` 文件中，新增以下的配置项：

```
search:
  path: search.xml
  field: post
```

### 中文链接转拼音（建议安装）

如果你的文章名称是中文的，那么 Hexo 默认生成的永久链接也会有中文，这样不利于 `SEO`，且 `gitment` 评论对中文链接也不支持。我们可以用 [hexo-permalink-pinyin](https://github.com/viko16/hexo-permalink-pinyin) Hexo 插件使在生成文章时生成中文拼音的永久链接。

安装命令如下：

```
npm i hexo-permalink-pinyin --save
```

在 Hexo 根目录下的 `_config.yml` 文件中，新增以下的配置项：

```
permalink_pinyin:
  enable: true
  separator: '-' # default: '-'
```

> **注**：除了此插件外，[hexo-abbrlink](https://github.com/rozbo/hexo-abbrlink) 插件也可以生成非中文的链接。

### 文章字数统计插件（建议安装）

如果你想要在文章中显示文章字数、阅读时长信息，可以安装 [hexo-wordcount](https://github.com/willin/hexo-wordcount)插件。

安装命令如下：

```
npm i --save hexo-wordcount
```

然后只需在本主题下的 `_config.yml` 文件中，将各个文章字数相关的配置激活即可：

```
postInfo:
  date: true
  update: false
  wordCount: false # 设置文章字数统计为 true.
  totalCount: false # 设置站点文章总字数统计为 true.
  min2read: false # 阅读时长.
  readCount: false # 阅读次数.
```

### 添加emoji表情支持（可选的）

本主题新增了对`emoji`表情的支持，使用到了 [hexo-filter-github-emojis](https://npm.taobao.org/package/hexo-filter-github-emojis) 的 Hexo 插件来支持 `emoji`表情的生成，把对应的`markdown emoji`语法（`::`,例如：`:smile:`）转变成会跳跃的`emoji`表情，安装命令如下：

```
npm install hexo-filter-github-emojis --save
```

在 Hexo 根目录下的 `_config.yml` 文件中，新增以下的配置项：

```
githubEmojis:
  enable: true
  className: github-emoji
  inject: true
  styles:
  customEmojis:
```

### 添加 RSS 订阅支持（可选的）

本主题中还使用到了 [hexo-generator-feed](https://github.com/hexojs/hexo-generator-feed) 的 Hexo 插件来做 `RSS`，安装命令如下：

```
npm install hexo-generator-feed --save
```

在 Hexo 根目录下的 `_config.yml` 文件中，新增以下的配置项：

```
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

### 修改页脚

页脚信息可能需要做定制化修改，而且它不便于做成配置信息，所以可能需要你自己去再修改和加工。修改的地方在主题文件的 `/layout/_partial/footer.ejs` 文件中，包括站点、使用的主题、访问量等。

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

> **注意**: 本主题中使用的 `Font Awesome` 版本为 `5.11.0`。

### 修改打赏的二维码图片

在主题文件的 `source/medias/reward` 文件中，你可以替换成你的的微信和支付宝的打赏二维码图片。

### 配置音乐播放器（可选的）

要支持音乐播放，在主题的 `_config.yml` 配置文件中激活music配置即可：

```
# 是否在首页显示音乐
music:
  enable: true
  title:     	    # 非吸底模式有效
    enable: true
    show: 听听音乐
  server: netease   # require music platform: netease, tencent, kugou, xiami, baidu
  type: playlist    # require song, playlist, album, search, artist
  id: 503838841     # require song id / playlist id / album id / search keyword
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
> ```
> id`获取方法示例: 浏览器打开网易云音乐，点击我喜欢的音乐歌单，浏览器地址栏后面会有一串数字，`playlist`的`id
> ```
>
> 即为这串数字。

## 文章 Front-matter 介绍

### Front-matter 选项详解

`Front-matter` 选项中的所有内容均为**非必填**的。但我仍然建议至少填写 `title` 和 `date` 的值。

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

### 最简示例

```
---
title: typora-vue-theme主题介绍
date: 2018-09-07 09:25:00
---
```

### 最全示例

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

## 插件

### 图片插件

#### 第一步：安装插件，在hexo根目录打开Git Bash,执行

```javascript
npm install hexo-asset-image --save
```

#### 第二步：打开hexo的配置文件_config.yml

```shell
找到 post_asset_folder，把这个选项从false改成true
```

####  第三步：打开

修改 /node_modules/hexo-asset-image/index.js

```javascript
'use strict';
var cheerio = require('cheerio');

function getPosition(str, m, i) {
  return str.split(m, i).join(m).length;
}

var version = String(hexo.version).split('.');
hexo.extend.filter.register('after_post_render', function(data){
  var config = hexo.config;
  if(config.post_asset_folder){
        var link = data.permalink;
    if(version.length > 0 && Number(version[0]) == 3)
       var beginPos = getPosition(link, '/', 1) + 1;
    else
       var beginPos = getPosition(link, '/', 3) + 1;
    var endPos = link.lastIndexOf('/') + 1;
    link = link.substring(beginPos, endPos);

    var toprocess = ['excerpt', 'more', 'content'];
    for(var i = 0; i < toprocess.length; i++){
      var key = toprocess[i];
 
      var $ = cheerio.load(data[key], {
        ignoreWhitespace: false,
        xmlMode: false,
        lowerCaseTags: false,
        decodeEntities: false
      });

      $('img').each(function(){
        if ($(this).attr('src')){
           
            var src = $(this).attr('src').replace('\\', '/');
            if(!/http[s]*.*|\/\/.*/.test(src) &&
               !/^\s*\//.test(src)) {
              var linkArray = link.split('/').filter(function(elem){
                return elem != '';
              });
              var srcArray = src.split('/').filter(function(elem){
                return elem != '' && elem != '.';
              });
              if(srcArray.length > 1)
                srcArray.shift();
              src = srcArray.join('/');
              $(this).attr('src', config.root + link + src);
              console.info&&console.info("update link as:-->"+config.root + link + src);
            }
        }else{
            console.info&&console.info("no src attr, skipped...");
            console.info&&console.info($(this));
        }
      });
      data[key] = $.html();
    }
  }
});
```

修改代码见- 补充文件夹下的images_file.js

#### 第四步：现在就可以插入图片了

比如hexo new  photo之后就在source/_posts生成photo.md文件和photo文件夹，我们把要插入的图片复制到photo文件夹内， 在photo.md文件里面按markdown的标准写,（我的文件名是head.jpeg）比如

```bash
![这是代替图片的文字，随便写](head.jpeg)
```



### 评论系统

#### 注册

​       先去注册，valine依附于LeanCloud，先去注册，并完成*实名认证* [LeanCloud注册](https://links.jianshu.com/go?to=https%3A%2F%2Fleancloud.cn%2F)

#### 实名认证

​       注册成功切记**实名认证**，反正他会自己提醒的，不完成也做不了下一步

​       实名认证后创建应用，应用名字啥的随便取。

####appid

然后获得appid，和keys，如下图：

![appid和appkeys](/Users/daiyizheng/Desktop/daiyz/source/_posts/hexo/valine1.webp)

​       然后下一步把你的博客域名写到安全中心。

![安全中心](/Users/daiyizheng/Desktop/daiyz/source/_posts/hexo/valine2.webp)

#### valine配置

​      打开next主题的config文件，找到valine配置，把你的appid和appkey填进去，重新hexo g，hexo d，一下，三分钟后，看看你的文章底部，简约而不简单的评论出来了。

```yaml
# Valine.
# You can get your appid and appkey from https://leancloud.cn
# more info please open https://valine.js.org
valine:
  enable: true
  appid:  手动打码
  appkey:  手动打码
  notify: false # mail notifier , https://github.com/xCss/Valine/wiki
  verify: false # Verification code
  placeholder: come on baby # 来啊，快活啊
  avatar: mm # gravatar style
  guest_info: nick,mail,link # custom comment header
  pageSize: 10 # pagination size
```



## 问题

### 数学公式不显示

在Hexo中渲染MathJax数学公式

在用markdown写技术文档时，免不了会碰到数学公式。常用的Markdown编辑器都会集成[Mathjax](https://link.jianshu.com?t=https%3A%2F%2Fwww.mathjax.org%2F)，用来渲染文档中的类Latex格式书写的数学公式。基于Hexo搭建的个人博客，默认情况下渲染数学公式却会出现各种各样的问题。

#### 原因

Hexo默认使用"hexo-renderer-marked"引擎渲染网页，该引擎会把一些特殊的markdown符号转换为相应的html标签，比如在markdown语法中，下划线'_'代表斜体，会被渲染引擎处理为``标签。

因为类Latex格式书写的数学公式下划线 '_' 表示下标，有特殊的含义，如果被强制转换为``标签，那么MathJax引擎在渲染数学公式的时候就会出错。例如，$x_i$在开始被渲染的时候，处理为$x``i``$，这样MathJax引擎就认为该公式有语法错误，因为不会渲染。

类似的语义冲突的符号还包括'*', '{', '}', '\'等。

####  解决方法

解决方案有很多，可以网上搜下，为了节省大家的时间，这里只提供亲身测试过的最靠谱的方法。

更换Hexo的markdown渲染引擎，[hexo-renderer-kramed](https://link.jianshu.com?t=https%3A%2F%2Fgithub.com%2Fsun11%2Fhexo-renderer-kramed)引擎是在默认的渲染引擎[hexo-renderer-marked](https://link.jianshu.com?t=https%3A%2F%2Fgithub.com%2Fhexojs%2Fhexo-renderer-marked)的基础上修改了一些bug，两者比较接近，也比较轻量级。

```undefined
npm uninstall hexo-renderer-marked --save
npm install hexo-renderer-kramed --save
```

执行上面的命令即可，先卸载原来的渲染引擎，再安装新的。

然后，跟换引擎后行间公式可以正确渲染了，但是这样还没有完全解决问题，行内公式的渲染还是有问题，因为[hexo-renderer-kramed](https://link.jianshu.com?t=https%3A%2F%2Fgithub.com%2Fsun11%2Fhexo-renderer-kramed)引擎也有语义冲突的问题。接下来到博客根目录下，找到node_modules\kramed\lib\rules\inline.js，把第11行的escape变量的值做相应的修改：

```js
//  escape: /^\\([\\`*{}\[\]()#$+\-.!_>])/,
  escape: /^\\([`*\[\]()#$+\-.!_>])/
```

这一步是在原基础上取消了对\,{,}的转义(escape)。
 同时把第20行的em变量也要做相应的修改。

```ruby
//  em: /^\b_((?:__|[\s\S])+?)_\b|^\*((?:\*\*|[\s\S])+?)\*(?!\*)/,
  em: /^\*((?:\*\*|[\s\S])+?)\*(?!\*)/
```

重新启动hexo（先clean再generate）,问题完美解决。哦，如果不幸还没解决的话，看看是不是还需要在使用的主题中配置mathjax开关。

#### 在主题中开启mathjax开关

如何使用了主题了，别忘了在主题（Theme）中开启mathjax开关，下面以next主题为例，介绍下如何打开mathjax开关。

进入到主题目录，找到_config.yml配置问题，把mathjax默认的false修改为true，具体如下：

```bash
# MathJax Support
mathjax:
  enable: true
  per_page: true
```

别着急，这样还不够，还需要在文章的Front-matter里打开mathjax开关，如下：

```css
title: index.html
date: 2016-12-28 21:01:30
tags:
mathjax: true
```

不要嫌麻烦，之所以要在文章头里设置开关，是因为考虑只有在用到公式的页面才加载 Mathjax，这样不需要渲染数学公式的页面的访问速度就不会受到影响了

