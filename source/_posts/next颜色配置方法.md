---
title: next颜色配置方法
author: hlss
date: 2021-08-04 14:41:23
tags: Hexo
categories: Hexo博客
keywords: 颜色
---

<b>前言：</b>通过F12审查网页元素，总结得出的next主题颜色配置方法，下面是我的颜色配置

![成果预览](../images/next%E9%A2%9C%E8%89%B2%E9%85%8D%E7%BD%AE%E6%96%B9%E6%B3%95/%E6%88%90%E6%9E%9C%E9%A2%84%E8%A7%88.png)

<!--more-->

---

##### 1.实时预览效果

`hexo s` ,在4000端口打开本地预览，在按下 `F12`，进入 `source`下的 `main.css`中

![F12审查](../images/next%E9%A2%9C%E8%89%B2%E9%85%8D%E7%BD%AE%E6%96%B9%E6%B3%95/F12%E5%AE%A1%E6%9F%A5.png)

root标签下的就是我们要修改的颜色配置项，通过 [RGB颜色对照](https://tool.oschina.net/commons?type=3)复制想要的颜色代码，替换相应配置项的内容就可以达到实时预览效果，而不用担心本地配置文件被破坏

##### 2.本地修改

预览结果满意后，就可以在本地同步修改，然后部署上去啦

对应修改文件的路径：`themes\next\source\css\_colors.styl`

```stylus
:root {
  --body-bg-color: $body-bg-color;
  --content-bg-color: $content-bg-color;
  --card-bg-color: $card-bg-color;
  --text-color: #00000;	//正文颜色
  --blockquote-color: $blockquote-color;
  --link-color: #0000CD;	//链接颜色（菜单选项，文章标题等）
  --link-hover-color: #FF8C00;	//链接悬停颜色
  --brand-color: #1E90FF;	//网站标题颜色
  --brand-hover-color: #FF8C00;	//网站标题悬停色
  --table-row-odd-bg-color: $table-row-odd-bg-color;
  --table-row-hover-bg-color: $table-row-hover-bg-color;
  --menu-item-bg-color: #E6E6FA;	//菜单选中背景颜色
  --btn-default-bg: #FFFFF;	 //'阅读更多'按钮背景颜色
  --btn-default-color: #FDF5E6;	//‘阅读更多’文字颜色
  --btn-default-border-color: #6495ED;	//'阅读更多'方框颜色
  --btn-default-hover-bg: $btn-default-hover-bg;	//'阅读更多'按钮背景悬停色
  --btn-default-hover-color: #7CFC00;	//'阅读更多'文字悬停色
  --btn-default-hover-border-color: #000000;	//'阅读更多'方框悬停色
```

希望本方法能对你有所帮助
