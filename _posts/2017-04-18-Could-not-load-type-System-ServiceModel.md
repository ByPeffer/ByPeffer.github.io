---
layout: post
title:  "未能从程序集“System.ServiceModel, Version=3.0.0.0, Culture=neutral……”中加载类型的问题解决"
categories: ASP.NET
tags:  IIS ASP.NET 程序集
author: Peffer
---

* content
{:toc}

在Windows 2012中部署的web程序，打开报错
`未能从程序集“System.ServiceModel, Version=3.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089”中加载类型“System.ServiceModel.Activation.HttpModule”。
`





![ 未能从程序集“System.ServiceModel……](/static/images/Could-not-load-type-System-ServiceModel-01.png)

以前出现这个问题，一般都是重新注册下iis就可以
```c
C:\Windows\Microsoft.NET\Framework\v4.0.30319\aspnet_regiis.exe -i -enable
```
在cmd中执行这个命令后居然跟以前的提示信息不一样，有错误!!!∑(ﾟДﾟノ)ノ 

```c
用于在本地计算机上安装和卸载 ASP.Net 的管理实用工具。
版权所有(C) Microsoft Corporation。保留所有权利。
开始安装 ASP.NET (4.0.30319.33440)。
此操作系统版本不支持此选项。管理员应使用“打开或关闭 Windows 功能”对话框、“服
务器管理器”管理工具或 dism.exe 命令行工具安装/卸载包含 IIS8 的 ASP.NET 4.5。有
关更多详细信息，请参见 http://Go.microsoft.com/fwlink/?LinkID=216771。
ASP.NET (4.0.30319.33440)安装完毕
```
![ 未能从程序集“System.ServiceModel……](/static/images/Could-not-load-type-System-ServiceModel-02.png)

重新打开程序跑一遍，仍然打不开。

原因分析：我的操作系统是64位的，网站的应用程序池选择的是“DefaultAppPool”。它的.NET CLR 版本是v4.0，托管管道模式是集成。据了解，64位操作系统托管管道模式要选择经典模式。

解决办法：在IIS中设置网站应用程序池为“.NET v4.5 Classic”。它的.NET CLR版本是v4.0，托管管道模式是经典。然后在此应用程序池的高级设置中设置“启用 32 位应用程序”的值为“True”，保存即可。