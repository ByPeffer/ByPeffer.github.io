---
layout: post
title:  "ASP.NET Gridview超出长度被撑变形"
categories: ASP.NET
tags:  ASP.NET Gridview Wrap ToolTip
author: Peffer
---

* content
{:toc}

# 问题：
Gridview绑定数据的时候，某列数据内容较多，由于项目页面显示要适应屏幕大小不能有滚动条，自动换行把高度撑变形了，设置不换行又把宽度给撑变形了。



# 解决思路：
超出固定长度用...代替，用ToolTip来提示全部信息。

# 解决方案：
####1.设置Css
```css
<style type="text/css">  
.mlength  
{  
display: block;  
width: 250px;  
overflow: hidden;    
white-space: nowrap;  
-o-text-overflow: ellipsis;  
text-overflow: ellipsis;  
}  
</style>  
```
####2.前台模版列设置
```CSharp
<asp:Label ID="Label1" runat="server" Text='<%# Eval("字段") %>' CssClass="mlength"  ToolTip='<%# Eval("字段") %>'></asp:Label> 
```

