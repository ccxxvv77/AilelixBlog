---
title: 使用Serverless平台+Hexo搭建个人博客
date: 2022-11-19
categories: Tech
tags:
  - Serverless
  - Node.js
---

# 前言
Serverless平台在特定场景的应用前景我还是蛮看好的  
配合上Hexo这种前端整个个人博客玩一玩真不戳
为什么不选择[Serverless平台+Notion](https://github.com/transitive-bullshit/nextjs-notion-starter-kit)？
也是一种选择
部署和码字会简单很多，但是没有这么丰富的{% hint "自定义" "折腾" %}功能

这套方案的优点
* 便宜（如果不强求域名的话可以完全免费
* {% hint "简单" "然而整Hexo也并不简单（" %}（不用接触LNMP或WNMP

缺点
* 网页只能静态（评论功能也可以整不过会略麻烦些
* Post会麻烦些（下文会细嗦

# 准备

1. 一个Serverless平台
   * [华为云](https://www.huaweicloud.com/product/servicestage.html?utm_source=cbg_develope)、[腾讯云](https://cloud.tencent.com/product/sls)：可以看看
   * [Cloudflare](https://www.cloudflare.com/zh-cn/products/workers/)：可以看看
   * [Github Page](https://docs.github.com/cn/pages/quickstart): 可以看看
   * [Heroku](https://www.heroku.com/)：要开始收费了
   * [Koyeb](https://www.koyeb.com/)：注册账号没给我通过
   * [Vercel](https://vercel.com/)：IP会被WallBan，有些人的不会，可以一试
   * [Render](http://render.com/)：限制稍多，访问速度略慢但功能齐全较推荐
   * [Netlify](https://www.netlify.com/)：这篇博客现在用的就是，功能同样齐全，但速度比Render稍快一点

2. 需要装的软件
  * [VSCode](https://code.visualstudio.com/)（或其它IDE
    > 推荐插件：
    > [VS Browser](https://marketplace.visualstudio.com/items?itemName=Phu1237.vs-browser) - 方便码字
    > [Markdown All in One](https://marketplace.visualstudio.com/items?itemName=yzhang.markdown-all-in-one) - 同上
    > [Hexo Utils](https://marketplace.visualstudio.com/items?itemName=fantasy.vscode-hexo-utils) - 方便管理文章
    > [stylus](https://marketplace.visualstudio.com/items?itemName=sysoev.language-stylus) - 方便编辑css
  * [Node.js](https://nodejs.org/zh-cn/) - 最新版本就行，本文使用18.12.1
  * [Git](https://git-scm.com/downloads) - 这应该不用写的吧

# 本地部署Hexo与NexT主题
## 安装Hexo
安装软件就不说了，一路next就行

!!! info 关于npm
    记得换[国内源或用cnpm](https://npmmirror.com/)

    Win用户请默认在npm和hexo后加.cmd

找一个好地方，然后

```bash
npm install hexo-cli -g
hexo init HexoBlog
cd HexoBlog
npm install
```

现在如果输入

```
hexo server
```

应该就可以在浏览器里通过 [htttp://localhost:4000/](htttp://localhost:4000/) 访问Hexo了

## 安装NexT

Hexo默认的主题很一般，对吧？
现在我们来安装NexT主题
Ctrl C关掉刚才的Hexo
还是在Hexo根目录下

```
git clone https://github.com/theme-next/hexo-theme-next themes/next
```

然后来到Hexo的 ```_config.yml``` 中

```yaml
# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
theme: next
```

!!! info 关于_config.yml
    Hexo有一个 ```_config.yml``` 存放Hexo的设置

    在根目录下

    而NexT主题也有一个 ```_config.yml``` 存放NexT的设置

    在 ```themes/next/``` 下

    文件夹```source```同理

    注意区分

现在再来 ```hexo g;hexo s``` 一下
不出意外的话就可以看到NexT主题的Hexo了

!!! info 关于hexo-cli
    Hexo是需要“编译”的（从源代码生成静态网页

    在你的Hexo项目根目录下
    
    运行 ```hexo generate``` 或 ```hexo g``` 以在 ```public/``` 文件夹下生成静态网页文件
    
    运行 ```hexo clean``` 来清除之前的静态文件

    不要直接修改public中的文件，更改会在下次生成中被覆盖

    修改/增加/删除 ```.md``` 文件不需要重新生成，也无需重启hexo

    而在修改了任何设置文件（.yml）或样式文件（.styl/.swig）后

    关闭当前hexo并 ```hexo clean;hexo g``` 然后再 ```hexo s``` 或 ```hexo server``` 来启动新实例

至此，Hexo+NexT就算是部署完成了

# 装插件与调设置
这一部分是如何把NexT设置成与我一样的步骤
非常折磨可以跳过
## 插件
[Hexo](https://hexo.io/plugins/)与[NexT](https://github.com/orgs/theme-next/repositories)都各自有非常多的插件
这里贴一下我的Hexo的 ```package.json```

```json
"dependencies": {
  "hexo": "^6.3.0",
  "hexo-admonition": "^1.1.2", //上面的"关于"插件
  "hexo-deployer-git": "^3.0.0", //Github部署插件
  "hexo-generator-archive": "^2.0.0",
  "hexo-generator-category": "^2.0.0",
  "hexo-generator-index-plus": "^1.0.0", //Index生成插件
  "hexo-generator-tag": "^2.0.0",
  "hexo-renderer-ejs": "^2.0.0",
  "hexo-renderer-markdown-it-plus": "^1.0.6", //Markdown渲染插件
  "hexo-renderer-stylus": "^2.1.0",
  "hexo-server": "^3.0.0",
  "hexo-symbols-count-time": "^0.7.1", //字数统计&时长估计插件
  "hexo-tag-hint": "^0.3.1", //提示框插件（上面那个“简单”
  "hexo-tag-katex": "^1.1.1", //KaTeX支持
  "hexo-tag-steamgame": "^1.0.0", //Steam游戏库插件
  "hexo-tag-wikipedia": "^1.0.1",  //维基百科插件
  "highlight.js": "^11.6.0",
  "js-yaml": "^4.1.0"
}
```

至于NexT，我只装了一个[Pjax](https://github.com/theme-next/theme-next-pjax)以加快网页加载速度

以上的插件都可以在Github找到{% hint "并且都提供了各自的安装方式" "我懒得写了（" %}

## _config.yml

### Hexo

#### Site&URL
基本信息
很简单没啥好说的

```yaml
# Site
title: Ailelix Blog #网站标题
subtitle: '' #副标题
description: '' #描述
keywords: #关键词
author: Ailelix Chen #博主
language: zh-CN #语言
timezone: 'Asia/Shanghai' #时区

# URL
## Set your site url here. For example, if you use GitHub Page, set url as 'https://username.github.io/project'
url: http://ailelix.icu/ #地址
```
#### HomePage

主页设置
这个讲到的人不多
先把Hexo中的config贴到这里
但先别急着改

```
# Home page setting
# path: Root path for your blogs index page. (default = '')
# per_page: Posts displayed per page. (0 = disable pagination)
# order_by: Posts order. (Order by date descending by default)
index_generator_plus:
  path: /home/
  per_page: 0
  order_by: -date
```

到了这一步，你可能注意到了
你自己的Hexo与我的Hexo的顶栏有些许不同

!!! info Hexo的架构
    这部分会*略长一些*，但是很重要

    Hexo不为主页、归档页等页面设置专门的“页”（Page）以与文章（Post）区别开

    而是将所有Markdown统一为Page、Post、Draft三个概念
    
    Page：页，主页归档页等页都是这个

    不同页结构不同如何处理？

    在 ```themes/next/layout/``` 文件夹里定义
    
    而每个.md文件的front-matter（---里面那个，作用类似一个Metadata）里可以通过 ```type:``` 定义其是“页”还是“文章”,抑或是“草稿”

    Post：文章，一个.md不指定type就会被认作为Post

    Draft: Hexo新更新出来的一个特性，用处不大

    然后我们再和文件目录 ```source/``` 结合一下

    假设有home、about两个page，A、B两个post

    > _posts/
    > 
    > > A.md
    > >
    > > B.md
    >
    > home/
    >
    > > index.md
    >
    > about/
    >
    > > index.md

    但是

    这是 ```home/index.md``` 中的的front-matter:

    type: **home**

    而非 type: page

    为什么要如此设计呢？

    上面的例子放在 ```themes/next/_config.yml``` 中
    
    参数会是这样：

    ```yaml
    # --------------------------------
    # Menu Settings
    # --------------------------------

    # Usage: `Key: /link/ || icon`
    # Key is the name of menu item. If the translation for this item is available, the translated text will be loaded, otherwise the Key name will be used. Key is case-senstive.
    # Value before `||` delimiter is the target link, value after `||` delimiter is the name of Font Awesome icon.
    # When running the site in a subdirectory (e.g. yoursite.com/blog), remove the leading slash from link value (/archives -> archives).
    # External url should start with http:// or https://
    menu:
      home: /home/ || fa fa-home
      #archives: /archives/ || fa fa-archive
      #categories: /categories/ || fa fa-th
      #tags: /tags/ || fa fa-tags
      about: /about/ || fa fa-user
      #schedule: /schedule/ || fa fa-calendar
      #sitemap: /sitemap.xml || fa fa-sitemap
      #commonweal: /404/ || fa fa-heartbeat
    ```

    现在是不是有点感觉了

    对，和你猜的那样大差不差

    当你点击顶栏中相应的按钮（如Home），它会在设置文件中的目录(/home/)去找相应名字的type（type: home）的.md文件

    而```home/```，```about/```文件夹根本不重要

    只要你在config里的目录设置下有那个 ```type: home``` 的文件就可以

    而这个.md究竟是什么结构，取决于其front-matter中设置的layout

    也就是说，Post和Page其实没什么区别，一个.md究竟是什么是由其front-matter决定的，而像上文一样编排source目录和配置文件只是为了方便管理罢了

    但要注意的是，只有 ```_post/``` 内的.md会被展示在归档页面上

现在回到我们的设置上来
为了方便管理
我选择同时使用Category与Tags
前者负责大体分类，后者相当于文章的关键词
加上主页、归档和关于页
我的source目录便是这样的：
![Source目录截图](文件夹目录.webp)
（请暂时无视font文件夹
e.g. 对应名字的文件夹可以放图片，插入直接写名字就ok

而在实践中，你可能很快会发现
你默认打开的主页，是一个文章汇总页
而且就算设置了Index Generator（Plus）
主页还是有问题的

```yaml
# Home page setting
# path: Root path for your blogs index page. (default = '')
# per_page: Posts displayed per page. (0 = disable pagination)
# order_by: Posts order. (Order by date descending by default)
index_generator_plus:
  path: /home/
  per_page: 0
  order_by: -date
```

这里我参考了网上的一个解决方案
用了一个讨巧的方法
在 ```home/index.md``` 的front-matter中加入：

```yaml
permalink: index.html
```

permalink可以设置一个地址以覆盖默认地址
这里我们把它设置成 ```index.html``` ，相当于覆盖了原来的主页
但是这样一来，点击“主页”图标相应的地址/home/就没有网页了
但是既然点击顶栏左侧的logo也能回到主页
那干脆就直接隐藏主页按钮吧（
那么我的 ```themes/next/_config.yml``` 便成了这样

```yaml
menu:
  #home: /home/ || fa fa-home
  archives: /archives/ || fa fa-archive
  categories: /categories/ || fa fa-th
  tags: /tags/ || fa fa-tags
  about: /about/ || fa fa-user
  #schedule: /schedule/ || fa fa-calendar
  #sitemap: /sitemap.xml || fa fa-sitemap
  #commonweal: /404/ || fa fa-heartbeat
```

#### Font

你应该也注意到了
我的网站更换了{% hint "字体" "JetBrainsMono负责英文和符号，思源黑体做中文Fallback" %}
如果你使用Google Font，那么官方已经提供了简单的配置方式，请自行搜索教程
如果你想使用本地字体，那么请继续阅读，但我还是建议你搜索一下别人的方案
我的这种方案很烂，但是至少能用不是吗（
在source下创建 ```font/``` 文件夹把字体放进去（woff格式为佳
next的 ```source/css/``` 文件夹下创建 ```_custom/custom_fonts.styl```

```css
@font-face{
    font-family: 'JetBrains Mono Light';
    src: url('../font/JetBrainsMono-Light.woff2');   
}

@font-face{
    font-family: 'Source Han Sans';
    src: url('../font/SourceHanSansSC-VF.woff2');   
}
```

font-maily：名字，随意填
url：字体路径

在next的 ```source/css/main.style``` 最后添加

```css
@import "_custom/custom_font.styl";
```

来导入自定义字体的css
但只导入是不够的，需要将其替换其它字体
在 ```next/source/css的_variables/base.style``` 下
将

```css
$font-family-chinese = "PingFang SC", "Microsoft YaHei";
```

的前面加上

```css
"JetBrains Mono Light", "Source Han Sans", 
```

!!! note 找不到上面这一项参数？
    如果没有的话，记得看看上面{% hint _config.yml language: zh-CN %}是否正确设置了

如果现在运行的话，你会发现字体仍然是原来的
还记得 ```public/``` 文件夹的作用吗
你可用在那底下找一找font
是的，我们的 ```source/font/``` 在生成时并没有被复制到静态文件中
我们只需要在 ```_config.yml``` 中

```yaml
# Include / Exclude file(s)
## include:/exclude: options only apply to the 'source/' folder
include: 
  - "font/"
exclude:
ignore:
```

就可以了

### NexT

这一部分真没啥好说的
都是一些很杂乱的参数
耐下心来慢慢整完就行

## Hexo-Admonition

这个插件要专门提一嘴
好用确实是好用
好看确实是好看
但是开了DarkMode这东西宛若依托答辩
官方给了个白色模式的css示例
我稍微改了改整了个黑夜版的
圆点和标题的位置稍微有点问题
但我懒得打磨了
附上 ```_custom/hexo_admonition.styl```

```css
.admonition {
  color: #1d1d1f;
  --link-color: #1d1d1f;
  --link-hover-color: #1d1d1f;
  margin: 1.5625em 0;
  padding: .6rem;
  overflow: hidden;
  page-break-inside: avoid;
  border-left: .3rem solid #42b983;
  border-radius: .3rem;
  box-shadow: 0 0.1rem 0.4rem rgba(0,0,0,.05), 0 0 0.05rem rgba(0,0,0,.1);
  background-color: #f2f2f7;
}

p.admonition-title {
  position: relative;
  margin: -0.9rem -1rem 0.8em -1rem !important;
  padding: .4rem .6rem .4rem 2.5rem;
  font-weight: 500;
  background-color:rgba(66, 185, 131, .1);
}

.admonition-title::before {
  position: absolute;
  margin: 0.2rem 0rem 0rem 0.2rem;
  top: .9rem;
  left: 1rem;
  width: 12px;
  height: 12px;
  background-color: #42b983;
  border-radius: 50%;
  content: ' ';
}

.admonition.note {
  border-left: .3rem solid #42b983 !important;
}

.info>.admonition-title, .todo>.admonition-title {
  background-color: rgba(0,184,212,.1);
}

.warning>.admonition-title, .attention>.admonition-title, .caution>.admonition-title {
  background-color: rgba(255,145,0,.1);
}

.failure>.admonition-title, .missing>.admonition-title, .fail>.admonition-title, .error>.admonition-title {
  background-color: rgba(255,82,82,.1);
}

.admonition.info, .admonition.todo {
  border-color: #00b8d4;
}

.admonition.warning, .admonition.attention, .admonition.caution {
  border-color: #ff9100;
}

.admonition.failure, .admonition.missing, .admonition.fail, .admonition.error {
  border-color: #ff5252;
}

.info>.admonition-title::before, .todo>.admonition-title::before {
  background-color: #00b8d4;
  border-radius: 50%;
}

.warning>.admonition-title::before, .attention>.admonition-title::before, .caution>.admonition-title::before {
  background-color: #ff9100;
  border-radius: 50%;
}

.failure>.admonition-title::before,.missing>.admonition-title::before,.fail>.admonition-title::before,.error>.admonition-title::before{
  background-color: #ff5252;;
  border-radius: 50%;
}

.admonition>:last-child {
  margin-bottom: 0 !important;
}
```

# 部署

这里一共有三种方式
假设Github有Repo A和B分别存放博客的代码和Buid文件
每次本地写完push到repo A备份

1. 对于支持Hexo的平台，可以直接Link到A，每次push完平台就会自动更新，也无需repo B的存在
2. 本地hexo d将静态文件push到B，需要安装hexo-deployer-git，并在config里设置B的url，平台Link到B的
3. 在A设置一个on: push的Github Actions，每次只需本地push到A，GA会自动build并把静态文件push到B，平台Link到B即可

平台Build时间：1>2=3
日用便捷性：1=3>2
几种我全部都试过，2=>3=>1

一开始我觉得平台一个月就300min的部署时间还是省着用
然后觉得2太麻烦了就换了3
然后发现以我更的频率根本用不完（
3的变种是我改这篇的时侯想到的
这时候我还在方案三

## 方案一
就很简单，没啥好说的
build命令是hexo g
build文件夹填public
运行命令不填

## 方案二
在 ```_config.yml``` 里，拉到最底下

```yaml
# Deployment
## Docs: https://hexo.io/docs/one-command-deployment
deploy:
  type: git
  repo: repo B的git url
  branch: repo B存放静态文件的分支
```
平台那里的部署选择静态即可

在我写到这里时，突然想到还有一种路子
在repo A里面设置main和build两个分支
然后静态文件存repo A/build即可
之前确实是有点傻了

# 方案三
在你的电脑上生成一个rsa密钥对
不要设置密码（两次询问密码直接enter

```shell
ssh-keygen -t rsa
```

将 ```_config.yml``` 中的deploy按照上文设置，注意url需要设置成ssh的
给repo B添加一个Deploy Key，然后把公钥填进去
给repo A的Secrets/Actions添加一个Secret
repo A设置Github Actions，代码如下

```yaml
name: 随便起个名字
on: push
jobs:
  main:
    runs-on: ubuntu-latest
    env:
      DEPLOY_KEY: ${{ secrets.你的Secret的名字，注意全大写 }}
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: 18
      - run: npm install hexo-cli -g
      - run: npm install
      - run: mkdir -p ~/.ssh
      - run: echo "$DEPLOY_KEY" > ~/.ssh/id_rsa
      - run: chmod 700 ~/.ssh
      - run: chmod 600 ~/.ssh/id_rsa
      - run: ssh-keyscan github.com >> ~/.ssh/known_hosts
      - run: git config --global user.email "Github的邮箱"
      - run: git config --global user.name "Github的用户名"
      - run: hexo clean
      - run: hexo g
      - run: hexo d
```
方案三的变种与上文方案二同理
GA可以直接build完把静态文件push到build分支里
（因为是同repo无需设置密钥即可操作

!!! note 关于Render的证书
    请**不要**给你的域名注册证书，{% hint Render会自行处理好 比养条狗还省心 %}

# 使用

## 写作

平时使用的体验是这套方案的最大痛点
我整出来了一个还算能用的方案
平时的写作是这样的
![用VSC写博客](VSCode.webp)
需要预览点击右边刷新即可

!!! note Tips
    VS Browser这个插件的”刷新“是刷新到输入url的那个页面，为了方便预览文章要直接输入文章的url而非homepage（根目录）

    你可能没听懂我在说什么，但是只要你用一下这个插件就会立马明白

这破坏了Markdown写作的一大特色：双手不离开键盘的纯净码字体验
不过熟练使用Markdown之后不需要预览也是完全可以写作的
我的情况是写完一大段再刷新预览一下内容
而我认为这是可以接受的

而想在VSC上获得舒适的Mardown写作体验，Tuning是必不可少的
这里列一下我作为暗色党的配置

> 主题：One Dark Pro Darker
> 字体：JetBrains Mono，思源黑体 粗细Normal 大小20 行高 26
> 插件：往上翻

体验还是相当不错的
如果需要管理文章打开Hexo Utils（在侧边栏）即可

## 上传

确认已经安装了hexo-deployer-git插件后
在 ```_config.yml``` 中最后：

```yaml
# Deployment
## Docs: https://hexo.io/docs/one-command-deployment
deploy:
  type: git
  repo: Repo B的.git url
  branch: main（或master）
```

每次写完，在根目录下运行 ```hexo clean;hexo g;hexo d``` 即可，hexo d（deploy）会自动将你的静态文件push到config中的git库

Repo A的备份只需要你在GitLens里点点按钮即可

Render等平台会自动检查你设置的库（Repo B）的更新并自动部署，如果你不喜欢刷新的延迟也可以用Github Actions做一个自动化

!!! note 另一种可能的方案
    不需要在本地hexo d

    每次写完只需要把更改push到Repo A
    
    在Github Actions里做一个自动化
    
    生成静态文件并hexo d到Repo B
    
    我没试过，但是理论上是可行的

# 小结

Serverless用来做博客确实称不上是完美的体验
Hexo+NexT这么一整也确实折腾
但不得不说这个成品确实是赏心悦目
况且，能白嫖服务器和二级域名还要啥自行车
~~以后有时间整一整评论功能吧~~
