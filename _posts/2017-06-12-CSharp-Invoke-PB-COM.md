---
layout: post
title:  "C#利用反射调用PB编译的COM组件"
categories: C#
tags:  C# 反射 PB COM组件 Invoke
author: Peffer
---

* content
{:toc}

# 问题：
C#利用反射调用（后期绑定）PB编译的COM组件

C#调用COM组件可以在VS工程中直接添加引用，这种方式写起来很方便，但是当COM组件经常更新，这样处理起来倒不如后期绑定适用了。


#### 1.根据COM组件的ProgID，得到COM组件公开的类型

```c#
Type comType = Type.GetTypeFromProgID(“jmjkk.n_sys_sbjc”);
```

#### 2.创建COM组件提供的类型的对象

```c#
object comObj = System.Activator.CreateInstance(comType);
```
#### 3.调用执行方法
类型和对象都用了，利用反射调用方法很简单,比如调用test方法，参数inParams:

```c#
object[] args = new object[1];
args[0] = inParams;
Method method = comType.GetMethod(“test”);
if (method != null){
    method.invoke(comObj, args);
}

```
然而，并没有这么顺利，method一直为null。
查找文档，发现.Net COM组件和非.Net COM组件得到的comType是不一样的，如果COM组件为.Net COM组件，上述反射调用方法没问题；如果COM组件是其它语言编写的，运行时得不到该COM类型的元数据，得到的comType将是所有未知类型COM组件的统一分装类型System.__ComObject，System.__ComObject类并不包含你想调用的组件的方法，所以comType.GetMethod(“method_name”)拿不到要给定名称的成员方法。
#### 正确姿势
非.Net COM组件得到comType和comObj后，使用comType.InvokeMember方法。comType.InvokeMember方法详细可参考[ MSDN中Type.InvokeMember 方法](https://msdn.microsoft.com/zh-cn/library/66btctbe(v=vs.110).aspx)

```c#
object[] args = new object[1];
args[0] = inParams;
object returnObj = comType.InvokeMember(“test”
, BindingFlags.InvokeMethod
, null
, comObj
, args);

```
