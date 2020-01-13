---
layout:     post           
title:      java项目没有main函数也能输出“HelloWorld”？
date:       2020-01-14
author:     码农先生
catalog: true
tags:
    - java
    - corejava
    - jdk6



---

# java项目没有main函数也能输出“HelloWorld”？

> 本文在[CSDN](https://blog.csdn.net/m0_37344350)同步更新

入了java坑的小伙伴都知道，Java项目需要main函数才能运行，main函数是java程序的入口。

下面这段代码大家已经熟到不能再熟了，可以说闭着眼睛都能敲出来......

```java
public class HelloWorld{
	public static void main(String[] args){
		System.out.println("Hello,World!");
	}
}
```

因为临近过年，今天我在公司事情不多，便拿起之前带到公司的**《Java核心技术卷一（中文第九版）》**看了起来，在**初始化块**章节看到了一个很有意思的例子（**4.6.7 初始化块 第131页**）：

![有意思的例子](https://upload-images.jianshu.io/upload_images/15803937-497784ad925ac882.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**java项目没有main函数也能输出“HelloWorld”？**

为了验证这个点，我敲下了以下代码：
```java
public class HelloWorld{
	static{
			System.out.println("Hello,World!");
			System.exit(0);
	}
}
```

开心地打开了命令行

![编译](https://upload-images.jianshu.io/upload_images/15803937-4d0ad45880b3a688.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

编译通过！

![运行](https://upload-images.jianshu.io/upload_images/15803937-7a47178a4ab09c5d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

可是一运行，控制台报错了！！！

于是我连忙把我前几天买的宝贝疙瘩**《Java核心技术卷一（中文第十一版）》**拿了出来（**PS：最新版真的很贵......**）。

![corejava11](https://upload-images.jianshu.io/upload_images/15803937-7938c6327f1986f3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

哦！原来我的版本是**jdk 8** ，这个问题在**jdk 8**中已经修复了，于是我将jdk版本调整到**jdk 7**

![jdk 7 编译运行](https://upload-images.jianshu.io/upload_images/15803937-6b8372d6301b033e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

可以看出，还是不行，于是我又将jdk版本换成**jdk 6**

![jdk 6 编译运行](https://upload-images.jianshu.io/upload_images/15803937-fbac601822762a85.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

可以看出，这个没有main函数的类在控制台上打印出了“Hello,World！”。

因此，可以得出，在jdk 6以前，没有main函数java程序也能在控制台中打印“HelloWorld”，JVM对main函数的检查也没有那么严格。

其实《corejava》第九版是面对java 7 出版的，里面新录入了很多java 7的新特性，但这个例子显然在java 7中就已经被修复。因此，这些权威书籍也会有错误。所谓**尽信书，不如无书**便是这个道理。自己实践一下才能印象更深！

![corejava9](https://upload-images.jianshu.io/upload_images/15803937-4d33c9d7d6ade4bd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

技术在不断发展，我们开发人员只有不断向前学习，才能不被行业所淘汰。

欢迎关注我的微信公众号：**一辈子的码农先生**，接下来会有非常多的干货总结，这也是我对自己几年工作的一种总结和交代。谢谢大家！

![我的公众号二维码.jpg](https://upload-images.jianshu.io/upload_images/15803937-cf5cc8e020d67f43.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)