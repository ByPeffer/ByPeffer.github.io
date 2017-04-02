---
layout: post
title:  "VS2015卸载后无法修改安装路径的解决方法"
categories: 开发工具
tags:  VS2015 重新安装 更改路径
author: Peffer
---

* content
{:toc}

VS2015出现了点问题，打算重新安装，结果死活选择不了安装路径,尝试了删除注册表也不知道删除哪一个，搜索出来很多关于VS的，网上查，大多也是说重装系统的，可是重装系统太麻烦了。




![ VS2015重新安装无法选择安装路径](/static/images/vsAddress01.jpg)

最后找到解决方法：
用管理员身份打开cmd，运行下面代码:

    V:\\vs_professional.exe /uninstall /force

`注：V:\\vs_professional.exe是vs2015的安装包路径`

解决方法来自：[Change visual studio 2015 enterprise installation path](https://social.msdn.microsoft.com/Forums/vstudio/en-US/114e18df-ffc5-442d-86c1-3b2ad4b28e36/change-visual-studio-2015-enterprise-installation-path?forum=vssetup)
