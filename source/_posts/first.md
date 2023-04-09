---
title: 建站小记
cover: https://th.bing.com/th/id/OIP.EwqfHlsXs-Zrh-dl__bWdgHaGi?pid=ImgDet&rs=1
category: cs
tag: 
 - 文本
id: 1
date: 2023-4-6
timeline: event
---


## 建站原因

在b站上看到了一个搭建个人博客网站的视频，心血来潮以及作为Geek必备的博客作为驱动力，于是查找了一些资料最终找到了以github + hexo 的方式建立博客。感谢朋友（畅姐我的神）的~~倾情~~推荐，给了我一个好[教程](https://oceanwang.top/personal-website-1/)。

- - -

## 曲折的建站过程

~~说到这个，不得不吐槽一下我这令人愤怒的电脑。从我大一上学期配C语言环境开始，再到虚拟机和linux ssh远端连接，这玩意就没让我省心过。~~
吐槽结束，现在是问题时间。
### 一

由于初次上手个人网站搭建，所知甚少。而且最开始时，没有如此系统的教程，都是用的零七碎八的教程，难免遇到各种问题。

- npm node hexo 安装完成后出现类似command not found的问题，用Google搜索、在stackoverflow上找相关问题，都没能解决这个问题，最后尝试了csdn上的一个说法，即设置环境变量,在path下面加上npm node hexo等的路径，成功解决问题。~~（多少有点zz了）~~
路径问题占据了我建站的绝大多数时间。

- 设置部署仓库和分支出现 "FATAL YAMLException: can not read a block mapping entry; a multiline key may not be an implicit key (107:14)" 的问题。这个问题是因我在更改hexo源码根目录下的_config.yml中的deploy部分直接复制粘贴引起的。参考下图，在repo:以及branch:的冒号后面要加上' '空格。
![代码](https://oceanwang.top/personal-website-7/9.png)

- 目前尚未解决的问题。GitHub Actions自动部署，按照教程上传workflow文件到github上，但是目前还没成功，也没查原因。