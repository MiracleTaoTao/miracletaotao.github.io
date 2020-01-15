---
layout:     post           
title:      真香的try-with-Resources
date:       2020-01-15
author:     码农先生
catalog: true
tags:
    - java
    - corejava
    - jdk7



---

# 真香的try-with-Resources

![真香.png](https://upload-images.jianshu.io/upload_images/15803937-432d5bfc651135cd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> 本文在[CSDN](https://blog.csdn.net/m0_37344350)同步更新



**java 7 之前的关流写法**

我们在项目中处理文件时，总会有很多的io流需要关闭，而关闭流的代码写起来比较繁琐，比如下面这种写法



```java
	/**
	 * 打印文件内容到控制台
	 * @param filePath
	 */
	public static void commonTryCatch(String filePath){
		  FileInputStream fis = null;
		  InputStreamReader isr = null;
		  BufferedReader br = null;
		  
		  try{
			  fis = new FileInputStream(filePath);
			  isr = new InputStreamReader(fis,"GBK");
			  br = new BufferedReader(isr);
			  
			  String line = "";
			  while((line = br.readLine()) != null){
				  System.out.println(line);
			  }
		  }catch(Exception e){
			  // to do 记录日志，方便定位
			  throw new RuntimeException("读取文件失败");//抛出封装过的自定义业务异常
		  }finally{
			  try{
				  if(fis != null){
					  fis.close();
				  }
			  }catch(Exception e){
				// to do 记录日志，方便定位
			  }
			  try{
				  if(isr != null){
					  isr.close();
				  }
			  }catch(Exception e){
				// to do 记录日志，方便定位
			  }
			  try{
				  if(br != null){
					  br.close();
				  }
			  }catch(Exception e){
				// to do 记录日志，方便定位
			  }
		  }
	}
```


事实上，在java 7 之前这种写法是最标准的写法:

- 首先在finally中关闭资源
- 然后在关闭时判断流对象是否为空，防止发生空指针异常
- 最后在关闭流的时候也有可能会出现异常，故在关流的时候也应该将异常捕捉并打印相关的业务日志



java 7优化后的写法**

在java 7 中，java给了一种非常简洁的写法；



```java
	/**
	 * 打印文件内容到控制台
	 * @param filePath
	 */
	public static void tryWithResources(String filePath){
		  try(FileInputStream fis = new FileInputStream(filePath);
				  InputStreamReader isr = new InputStreamReader(fis,"GBK");
				  BufferedReader br = new BufferedReader(isr);){
			  
			  String line = "";
			  while((line = br.readLine()) != null){
				  System.out.println(line);
			  }
		  }catch(Exception e){
			  // to do 记录日志，方便定位
			  throw new RuntimeException("读取文件失败");//抛出封装过的自定义业务异常
		  }
	}
```

**语法：**

```java
try(Resource res = ...){
  work with res
}
```
try块退出时，会自动调用res.close()



大家可以看下corejava书中的描述

![corejava异常处理](https://upload-images.jianshu.io/upload_images/15803937-eab2755d7b4698f8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



尽管这个特性在java 7 就出现了，但我发现身边的同事用得很少，可能是java 7这个新特性比较低调吧，不过我可**不允许这么香的特性被丢在角落里！**所以有了这篇文章。



用过这个特性后的我只能说：“真香！”



![真香](https://upload-images.jianshu.io/upload_images/15803937-24ed45ff94e5e7c5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



所以，以后关闭资源尽可能使用**try-with-Resources**的方式，代码简洁明了，一点也不拖泥带水。

欢迎关注我的微信公众号：**一辈子的码农先生**，接下来会有非常多的干货总结，既为沉淀自己，也为帮助他人，感谢关注！


![我的公众号二维码.jpg](https://upload-images.jianshu.io/upload_images/15803937-cf5cc8e020d67f43.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)