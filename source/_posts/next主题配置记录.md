---
title: Hexo博客配置next主题
tags: Hexo
categories: Hexo博客
author: hlss
date: 2021-08-02 14:26:27
typora-root-url: ..
---



**前言**：搭建好hexo博客后，配置next主题的经验记录

<!--more-->

---

### 1.安装NexT

```
#Blog根目录下git clone
git clone https://github.com/theme-next/hexo-theme-next themes/next
```

### 2.启用主题

在Blog根目录下打开<span style='color:文字颜色;background:yellow;font-size:文字大小;font-family:字体;'>_config.yml</span>文件，找到`theme`字段，修改其值为`next`

> 之后Blog根目录下的_cofig.yml称为<span style='color:文字颜色;background:yellow;font-size:文字大小;font-family:字体;'>站点配置文件</span>，themes/next/_config,yml称为是<span style='color:文字颜色;background:yellow;font-size:文字大小;font-family:字体;'>主题配置文件</span>

```
theme: next
```

next包含四个不同的主题，可以在主题配置文件里修改

```
#取消要使用的主题前的注释即可
# Schemes
#scheme: Muse
#scheme: Mist
scheme: Pisces
#scheme: Gemini
```

### 3.设置菜单

包含三个部分：菜单项（名称和链接），菜单项的显示文本，菜单项对应图标

设置菜单项方法：打开主题配置文件，查找`menu`,删除前面的注释`#`即可

```
menu:
  home: / || home                      #首页
  archives: /archives/ || archive      #归档
  categories: /categories/ || th       #分类
  tags: /tags/ || tags                 #标签
  about: /about/ || user               #关于
  #schedule: /schedule/ || calendar    #日历
  #sitemap: /sitemap.xml || sitemap    #站点地图，供搜索引擎爬取
  #commonweal: /404/ || heartbeat      #腾讯公益404
```

“||”前面的是目标链接，后面的是图标名称，next使用的图标全是[图标库 - Font Awesome 中文网](http://www.fontawesome.com.cn/faicons/#web-application)这一网站的，有想用的图标直接在fontawesome上面找图标的名称就行。

<b> <span style='color:文字颜色;background:yellow;font-size:文字大小;font-family:字体;'>**</span>如果想自定义菜单项，如想加入<span style='color:文字颜色;background:yellow;font-size:文字大小;font-family:字体;'>something</span>到菜单，需要执行一下步骤</b>

在主题配置文件添加something项

```
menu:
  home: / || home                      #首页
  archives: /archives/ || archive      #归档
  categories: /categories/ || th       #分类
  tags: /tags/ || tags                 #标签
  about: /about/ || user               #关于
  resources: /resources/ || download   #资源
  #schedule: /schedule/ || calendar    #日历
  #sitemap: /sitemap.xml || sitemap    #站点地图，供搜索引擎爬取
  #commonweal: /404/ || heartbeat      #腾讯公益404
  something: /目标链接/ || 图标名
```

为something添加中文翻译。打开<span style='background:yellow'>/themes/next/languages/zh-CN.yml</span>,修改`menu`

```
menu:
  home: 首页
  archives: 归档
  categories: 分类
  tags: 标签
  about: 关于
  something: 有料
```

在根目录下打开Git Bash，输入如下代码：

```
hexo new page "something"
```

此时在根目录的sources文件夹下会生成一个something文件，文件中有一个`index.md`文件，修改内容如下：

```
---
title: something
date: 2021-07-28 12:37:17
type: "something"
comments: false   #评论
---
```

菜单项的设置到此完结

### 4.设置头像和网站图标

打开主题配置文件，建议将图片放在/themes/next/source/images下，修改如下

```
#查找avater,url后面是头像位置

# Sidebar Avatar
avatar:
  # Replace the default image and set the url here.
  url: /images/avatar.gif   #图片的位置，也可以是http://xxx.com/avatar.png
  # If true, the avatar will be dispalyed in circle.
  rounded: true   #头像展示在圈里
  # If true, the avatar will be rotated with the cursor.
  rotated: false  #头像随光标旋转
  
  
  #修改网站图标，只修改samll和medium就行
  favicon:
  small: /images/favicon-16x16.png  #注意图片大小16X16
  medium: /images/favicon-32x32.png  #32X32
  apple_touch_icon: /images/apple-touch-icon-next.png
  safari_pinned_tab: /images/logo.svg
  #android_manifest: /images/manifest.json
  #ms_browserconfig: /images/browserconfig.xml
```

###  5.添加本地搜索

安装插件


```
npm install hexo-generator-search
```

在站点配置文件最后加入如下代码

```
 # Search 
search:
  path: ./public/search.xml     #索引文件的路径，相对于站点根目录
  field: post   #搜索范围，默认是 post，还可以选择 page、all，设置成 all 表示搜索所有页面
  format: html
  limit: 10000  #限制搜索条目数
```



修改主题配置文件

```
local_search:
  enable: true
  # If auto, trigger search by changing input.
  # If manual, trigger search by pressing enter key or search button.
  trigger: manual
  # Show top n results per article, show all results by setting to -1
  top_n_per_article: 1  #表示在每篇文章中显示的搜索结果数量，设成 -1 会显示每篇文章的所有搜索结果数量
```

#### <span style='background:yellow'>记录一个错误</span>

```
hexo clean 
hexo g
hexo s
```

三部曲之后，在4000界面预览发现搜索功能一直在loading

```
#解决办法，再次更新hexo插件服务
npm install hexo-generator-searchdb 
```

> [Hexo NexT主题站内搜索功能异常](https://www.dazhuanlan.com/hfyiyue/topics/1640075) 

### 6.添加cnavas nest 3D效果,设置博文内链接为蓝色

> 设置方法文章顶部知乎链接

### 7.利用<span style='color:文字颜色;background:yellow;font-size:文字大小;font-family:字体;'>leancloud</span>统计单篇文章阅读量，以及实现评论功能

> [Hexo博客使用LeanCloud统计页面访问次数 - 程序员大本营](https://www.pianshen.com/article/9435262132/)
>
> [Hexo Next主题 使用LeanCloud统计文章阅读次数、添加热度排行页面 - 咖里De](https://blog.garryde.com/archives/48665.html)

### 8.利用<span style='color:文字颜色;background:yellow;font-size:文字大小;font-family:字体;'>不蒜子</span>统计总访客数和总访问量

- 基本配置

> [ hexo框架next主题统计访问人数_Xiaoweidumpb-CSDN博客](https://blog.csdn.net/qq_43751489/article/details/102990376?utm_medium=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7EBlogCommendFromMachineLearnPai2%7Edefault-1.pc_relevant_baidujshouduan&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7EBlogCommendFromMachineLearnPai2%7Edefault-1.pc_relevant_baidujshouduan)

- hexo s后底部总访问量很大

> [hexo框架next主题统计访问人数_Xiaoweidumpb-CSDN博客](https://blog.csdn.net/qq_43751489/article/details/102990376?utm_medium=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7EBlogCommendFromMachineLearnPai2%7Edefault-1.pc_relevant_baidujshouduan&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7EBlogCommendFromMachineLearnPai2%7Edefault-1.pc_relevant_baidujshouduan)

### 9.文章末尾添加版权声明

查找**主题配置文件**themes/next/_config.yml中的creative_commons：

```
creative_commons:
  license: by-nc-sa
  sidebar: false
  post: true  # 将false改为true即可显示版权信息
  language:
```

### 10.更换代码块背景色

> [主题配置 - NexT 使用文档](http://theme-next.iissnan.com/theme-settings.html)

### 11.高级设置

本来想实现把文章多级嵌套分类，结果失败了，这里提供链接

> [数据文件 | 小丁的个人博客](https://tding.top/docs/getting-started/data-files.html)

### 12.背景图片设置

我的next版本是7.8.0，网上的设置办法包含两种，一种是利用styles.styl配置文件来实现，但是因为版本更新，这个方法对我怎么也不适用，下面介绍第二种方法。

确认你能找到 `themes\next\source\css\_common\components\pages\pages.styl`，然后打开pages.styl,添加以下内容：

```stylus
body {
    background:url(/images/background.png); // 可以是路径也可以是链接
    background-repeat: no-repeat; // 不重复
    background-attachment:fixed; // 固定住背景图片
    background-position:50% 50%; // 图片位置：居中
    background-size: 100% 100%; // 图片长宽扩充为100%
}

```

背景图片路径：`themes\next\source\images\background.png`

最后部署一下，就行了

> [(1条消息) 【主题美化系列】NexT7主题添加背景图片_kris的博客-CSDN博客](https://blog.csdn.net/louis_li51/article/details/105227430?utm_medium=distribute.pc_relevant.none-task-blog-2~default~baidujs_baidulandingword~default-1.pc_relevant_baidujshouduan&spm=1001.2101.3001.4242)

### 13.npm删除多余插件

本来想找到一个在博文中插入图片的办法，结果按照网上的输入`cnpm install hexo-asset-image --save`后`hexo g`报错，一看根目录下的<span style='background:yellow'>node_moudles</span>文件夹下几个文件夹变成了快捷方式，果断删除这插件，命令如下：

```
cnpm uninstall hexo-asset-image
```

[卸载 hexo 插件 · 大专栏](https://www.dazhuanlan.com/dennis-zoo/topics/995051)

### 14.多级菜单/多层级嵌套页面的实现

> [hexo嵌套页面]([https://hlss-lxh.github.io/2021/08/03/hexo%E5%B5%8C%E5%A5%97%E9%A1%B5%E9%9D%A2/](https://hlss-lxh.github.io/2021/08/03/hexo嵌套页面/))

### 15.hexo结合typora的插入图片的办法

> [typora + hexo博客中插入图片 | yinyoupoet的博客](https://yinyoupoet.github.io/2019/09/03/hexo%E5%8D%9A%E5%AE%A2%E4%B8%AD%E6%8F%92%E5%85%A5%E5%9B%BE%E7%89%87/)

### 16.百度收录

> [hexo博客进行百度、谷歌SEO | yinyoupoet的博客](https://yinyoupoet.github.io/2019/09/04/hexo%E5%8D%9A%E5%AE%A2%E8%BF%9B%E8%A1%8C%E7%99%BE%E5%BA%A6%E3%80%81%E8%B0%B7%E6%AD%8CSEO/#%E5%89%8D%E8%A8%80)



### 17.代码块添加复制功能

最新的next已经内置这个功能，在主题配置文件`copybutton`作如下修改

```yaml
copy_button:
    enable: true    #开启代码复制
    # Show text copy result.
    show_result: true
    # Available values: default | flat | mac
    style: mac
```

### 18.透明背景

主要介绍 内容板块，菜单板块，侧边栏板块的透明度设置方法

内容板块和菜单板块的设置都在 `themes\next\source\css\_schemes\Pisces\_layout.styl`中

```stylus
#两个板块分别位于两个标签下
.content-wrap {
  background: rgba(255,255,255,0);	//内容透明度，0为透明度，可自行设置
  border-radius: $border-radius-inner;
  
  .header-inner {
  background: rgba(255,255,255,0.2);	//菜单栏背景透明度
  border-radius: $border-radius-inner;
```

侧边栏板块设置在 `themes\next\source\css\_schemes\Pisces\_sidebar.styl`

```stylus
.sidebar {
  background: rgba(255,255,255,0.2);	//侧栏透明度
  box-shadow: none;
  margin-top: 100%;
```

> [Hexo解决页面过小问题与设置透明背景 - Hk_Mayfly - 博客园](https://www.cnblogs.com/Mayfly-nymph/p/10622307.html)

### 19.“创建时间”等扩展颜色的修改

在 `themes\next\source\css\_common\components\post\post-header.styl`中找到 `.posts-expand .post-meta`标签，修改如下：

```stylus
.posts-expand .post-meta {
  color: $grey-lighter;	#颜色
```

在 `themes\next\source\css\_variables\base.styl`中内置了一些颜色供选择，或者可以在 [RGB颜色对照表](https://tool.oschina.net/commons?type=3)中选择，不过样式要写成 `#fffff`的形式

```stylus
// Colors,next内置颜色
// colors for use across theme.
// --------------------------------------------------
$whitesmoke   = #f5f5f5;
$gainsboro    = #eee;
$grey-lighter = #ddd;
$grey-light   = #ccc;
$grey         = #bbb;
$grey-dark    = #999;
$grey-dim     = #666;
$black-light  = #555;
$black-dim    = #333;
$black-deep   = #222;
$red          = #ff2a2a;
$blue-bright  = #87daff;
$blue         = #0684bd;
$blue-deep    = #262a30;
$orange       = #fc6423;
```

<b>推荐阅读：</b>[next颜色配置方法]()

---



>大方向参考
>
>[参考链接1：知乎](https://zhuanlan.zhihu.com/p/105584373)
>
>[参考链接2：NexT使用文档](http://theme-next.iissnan.com/getting-started.html)
>
>[参考链接3：Hexo Next主题进阶详细教程](https://blog.csdn.net/qq_31279347/article/details/82427562)

