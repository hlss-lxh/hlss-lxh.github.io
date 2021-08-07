---
title: hexo在github部署博客的基本过程
tags: Hexo
categories: Hexo博客
author: hlss
date: 2021.08.01 13:37:28
---

​	<b>前言</b>：主要记录了自己利用hexo搭建一个基本静态博客的方法，以及对首次yilia主题的尝试

<!--more-->

---

#### 关于侧边栏标签如何添加标签

> [参考链接](https://www.cnblogs.com/zzw1024/p/12051995.html)

- 只用前三步即可创建一个标签（例如：分类）

- 注意修改<span style='background:yellow'>\themes\yilia\_config.yml</span>时的书写方式

  ```yaml
  menu:
    主页: /
    #随笔: /tags/随笔/
    分类: /categories/
  ```

#### more语句截断改善，以及如何给文章添加多标签

> [参考链接](https://www.freesion.com/article/1931619786/)

#### hexo基本博客的搭建以及如何在github部署

1. 下载安装nodejs

2. 安装git

3. 安装镜像源
   
   ```
   #打开git,输入指令安装镜像源，cnpm,提高速度
   npm install -g cnpm --registry=https://registry.npm.taobao.org
   #可以输入 cnpm -v 检验
   ```
   
4. 全局安装hexo

   ```
   cnpm install -g hexo-cli
   #可以输入 hexo -v 验证
   ```

5. 选择一个磁盘建立Blog文件夹（文件夹名字任意）

   > 如果后期搭建或者配置博客出错,可以删除Blog文件夹和第八步中的`.ssl`文件夹，从这一步重新开始搭建

6. 在Blog文件夹下右键git bash here

7. hexo初始化博客
   
   ```
   #输入命令初始化博客
   hexo init
   ```

   > 此时输入命令hexo s ，就可以通过生成的http://localhost:4000访问基于本地的博客网页
   
   
   
8. 部署博客到github的准备

   1. github新建仓库，命名为yourname.github.io(yourname是你的github名)

   2. 安装git部署插件（仍旧在Blog目录下进行）

      ```
      cnpm install --save hexo-deployer-git
      ```

   3. 记事本打开并修改Blog根目录下的_config.yml中的deploy,如下

      ```
      #除了中文，其余都用英文输入（包括冒号）
      
      # Deployment
      ## Docs: https://hexo.io/docs/one-command-deployment
      deploy:
        type: git
        repo: git@github.com:GitHub用户名/GitHub用户名.github.io.git   
        branch: main   #github最新为main，而不是原来的master
      ```

   4. 回到git bash,生成SSL添加到github

      ```
      #先让github知道你是谁
      git config --global user.name "yourname"  #github用户名
      git config --global user.email "youremail"  #github注册邮箱
      
      #生成SSH，默认生成在C:\Users\username\.ssh文件夹下
      ssh-keygen -t rsa -C "youremail"
      #输入命令后需要连按三次enter
      ```

      ssh，简单来讲，就是一个秘钥，其中，id_rsa.pub是公共秘钥，把这个公钥放在GitHub上，这样当你链接GitHub自己的账户时，它就会根据公钥匹配你的私钥，当能够相互匹配时，才能够顺利的通过git上传你的文件到GitHub上。

      而后在GitHub的setting中，找到SSH keys的设置选项，点击New SSH key
      把你的id_rsa.pub里面的信息复制进去。

9. 回到git bar

   ```
   #部署到github
   hexo d
   #现在浏览器就可以访问啦（github注册名.github.io）
   ```

10. 剩下的就是选择主题和美化的事情了

> 参考链接
>
> [CSDN](https://blog.csdn.net/sinat_37781304/article/details/82729029)
>
> [bilibili](https://www.bilibili.com/video/BV1Yb411a7ty?from=search&seid=11345417621504747834)
>
> [知乎](https://www.zhihu.com/question/38219432?sort=created)

#### 主题安装及配置

1. ###### yilia主题安装

   1. Blog根目录下执行安装指令

      ```
      git clone git://github.com/litten/hexo-theme-yilia.git themes/yilia
      ```

   2. 修改Blog根目录下_config.yml文件_

      ```
      # Extensions
      ## Plugins: https://hexo.io/plugins/
      ## Themes: https://hexo.io/themes/
      theme: yilia  #这里修改为yilia
      ```

   3. 回到git bash,输入以下指令

      ```
      hexo clean  #清除多余缓存
      hexo g
      hexo s      #(可选项)用以生成本地预览，地址http://localhost:4000
      hexo d		#部署到github,完成后浏览器查看实际效果
      ```

2. 头像、网站图标等添加

   > [参考链接](https://www.freesion.com/article/1736594755/)

3. 侧边栏作者和名言不显示问题（侧边栏背景的解决方案我没找相关修改项）

   > [参考链接](https://blog.csdn.net/m0_51078229/article/details/110943313)

4. 这个主题截止于此吧，我出错了，找不到问题

   
