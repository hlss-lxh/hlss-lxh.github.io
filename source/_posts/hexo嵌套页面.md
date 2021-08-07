---
title: Hexo多层级嵌套页面方法
date: 2021-08-03 16:57:20
keywords: Hexo 多级
tags: Hexo
categories: Hexo博客
---
<b>前言</b>：关于嵌套页面的实现方法，官方文档和百度上都没有讲的很明白，通过自己摸索总结出这一方法

<!--more-->

---

##### 1.在站点配置文件的`menu`中加入以下内容

```yaml
menu:
  home: / || fa fa-home
  Hexo:
    default: /hexo/ || fa fa-folder-open	#一级页面
    next:
        default: /next/ || fa fa-sticky-note	#二级页面
        Nested_pages: /nested_pages.html || fa fa-file	#三级页面，建议建立三级即可，过多会影响搜索引擎的收录
```

每一个`default`对应一个页面，我这里模仿官方文档的写法，因此`Nested_pages`的目标链接为<span style='background:yellow'><b>.html</b></span>

`||`前面是目标链接，指向内容由自己编写，`||`后面是自定义图标，在[Font Awesome](https://fontawesome.dashgame.com/)这个网站复制想要的图标名称进行替换就行了

##### 2.生成对应文件

根目录下右键打开git bash,新建3个层级页面对应的文件

```yaml
hexo new page hexo
hexo new page next
hexo new page nested_pages
```

生成的文件会分别以名为<span style='background:yellow'>hexo, next, nested_pages</span>的三个文件夹的形式存在根目录下的`source`文件夹中

##### 2.设置文件层级结构

先将nested_pages文件夹下的<span style='color:文字颜色;background:yellow;font-size:文字大小;font-family:字体;'>index.md</span>文件重命名为<span style='color:文字颜色;background:yellow;font-size:文字大小;font-family:字体;'>nested_pages.md</span>

打开根目录下`source`文件夹，将nested_pages文件夹中的<span style='background:yellow'>index.md</span>文件移到next文件夹中，再将next文件夹移到hexo文件夹中，最终的层级目录如下:

![目录层级](../images/hexo%E5%B5%8C%E5%A5%97%E9%A1%B5%E9%9D%A2/%E7%9B%AE%E5%BD%95%E5%B1%82%E7%BA%A7.png)

##### 3.简单编写一下各个层级页面的内容

下面是我为了测试和区分编写的内容

hexo文件夹下的index.md文件：

![hexo_index](../images/hexo%E5%B5%8C%E5%A5%97%E9%A1%B5%E9%9D%A2/hexo_index.png)

next文件夹下的index.md文件

![next_index](../images/hexo%E5%B5%8C%E5%A5%97%E9%A1%B5%E9%9D%A2/next_index.png)

已经移动到next的nested_pages.md文件

![nested_pages](../images/hexo%E5%B5%8C%E5%A5%97%E9%A1%B5%E9%9D%A2/nested_pages.png)

##### 4.使用中文显示

在`themes/next/languages/zh-CN`中的`menu`选项加入翻译

```
Netsed_pages: 嵌套页面
```

之后就可以在不同层级的页面是继续奋“键”疾书啦

##### 5.总结

嵌套页面创建后的编写不同于`hexo n "文章名称"`，后者可以为文章添加标签和分类属性，包括`<!--more-->`的使用，嵌套页面内编写的文章添加不了这些属性，目前检测还不能使用`<!--more-->`,这是我比较不满足的地方

所以建议next主题的嵌套页面用来写专栏类的文章，例如文章最后链接官方的做法

我认为应该有解决办法，但是奈何技术不到家，网上又苦寻无果，只能留待来日在解决了

最后提醒大家如果要删除嵌套页面，除了修改回`menu`外，文件夹应该从低层级向高层级依次删除，如先删除next文件夹下内容，再删除hexo文件夹下内容，最后删除hexo文件夹，否则系统会提示要管理员权限，然后各种操作都无法删除

##### 6.参考链接

> [Getting Started | NexT](https://theme-next.js.org/docs/getting-started/)

