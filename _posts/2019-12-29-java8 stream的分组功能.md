---
layout:     post           
title:      java8 stream的分组功能
date:       2019-12-29
author:     码农先生
catalog: true
tags:
    - java8
    - stream
    - group


---
# 经典排序之插入排序

> 本文在[CSDN](https://blog.csdn.net/m0_37344350)同步更新

最近，项目开发时遇到一个问题。根据业务要求，前端给后端上送的参数是一个列表（如List<Student> list），因此，后端也用了一个列表来接收。然而，等后端拿到数据后，我发现我需要对相同classId的数据进行统一处理。于是，我找到前端妹妹讨论，看她能不能帮忙把相同classId的数据封装成列表传给我。我好将接收参数修改成以下格式（List<Dto> list）：

```java
class Dto{
    String classId;
    List<Student> list;
}
```


<center>这时，前端妹妹评估了下改动程度，眼泪汪汪地看着我</center>
![小哥哥](https://github.com/MiracleTaoTao/miracletaotao.github.io/blob/master/_posts/2019-12-29-java8%20stream%E7%9A%84%E5%88%86%E7%BB%84%E5%8A%9F%E8%83%BD/%E5%B0%8F%E5%93%A5%E5%93%A5.jpg?raw=true)


<center>我瞬间明白了，我表现的机会到了！</center>
<center>我说道：这样吧！前端不动，后端来处理！</center>
![我来](https://github.com/MiracleTaoTao/miracletaotao.github.io/blob/master/_posts/2019-12-29-java8%20stream%E7%9A%84%E5%88%86%E7%BB%84%E5%8A%9F%E8%83%BD/%E6%88%91%E6%9D%A5.jpg?raw=true)

<center>后端不能说不行！</center>
![后端不能说不行](https://github.com/MiracleTaoTao/miracletaotao.github.io/blob/master/_posts/2019-12-29-java8%20stream%E7%9A%84%E5%88%86%E7%BB%84%E5%8A%9F%E8%83%BD/%E5%90%8E%E7%AB%AF%E4%B8%8D%E8%83%BD%E8%AF%B4%E4%B8%8D%E8%A1%8C.jpg?raw=true)

<center>仔细看了下数据，运用java 8 stream分组功能轻松解决。</center>
![轻松搞定](https://github.com/MiracleTaoTao/miracletaotao.github.io/blob/master/_posts/2019-12-29-java8%20stream%E7%9A%84%E5%88%86%E7%BB%84%E5%8A%9F%E8%83%BD/%E8%BD%BB%E6%9D%BE%E6%90%9E%E5%AE%9A.jpg?raw=true)




```java
public static void testStreamGroup(){
    List<Student> stuList = new ArrayList<Student>();
    Student stu1 = new Student("10001", "孙权", "1000101", 16, '男');
    Student stu2 = new Student("10001", "曹操", "1000102", 16, '男');
    Student stu3 = new Student("10002", "刘备", "1000201", 16, '男');
    Student stu4 = new Student("10002", "大乔", "1000202", 16, '女');
    Student stu5 = new Student("10002", "小乔", "1000203", 16, '女');
    Student stu6 = new Student("10003", "诸葛亮", "1000301", 16, '男');

    stuList.add(stu1);
    stuList.add(stu2);
    stuList.add(stu3);
    stuList.add(stu4);
    stuList.add(stu5);
    stuList.add(stu6);

    Map<String, List<Student>> collect = stuList.stream().collect(Collectors.groupingBy(Student::getClassId));
    for(Map.Entry<String, List<Student>> stuMap:collect.entrySet()){
         String classId = stuMap.getKey();
         List<Student> studentList = stuMap.getValue();
         System.out.println("classId:"+classId+",studentList:"+studentList.toString());
    }
}
```


```java
classId:10002,studentList:[Student [classId=10002, name=刘备, studentId=1000201, age=16, sex=男], Student [classId=10002, name=大乔, studentId=1000202, age=16, sex=女], Student [classId=10002, name=小乔, studentId=1000203, age=16, sex=女]]
classId:10001,studentList:[Student [classId=10001, name=孙权, studentId=1000101, age=16, sex=男], Student [classId=10001, name=曹操, studentId=1000102, age=16, sex=男]]
classId:10003,studentList:[Student [classId=10003, name=诸葛亮, studentId=1000301, age=16, sex=男]]
```

从上面的数据可以看出来，stuList被分成了三个组，每个组的key都是classId，而每个classId都对应一个学生列表，这样就很轻松地实现了数据的分离；此时，无论需要对数据进行怎样的处理都会很容易。

欢迎关注我的微信公众号：**一辈子的码农先生**，接下来会有非常多的干货总结，这也是我对自己几年工作的一种总结和交代。谢谢大家！

![我的公众号二维码.jpg](https://upload-images.jianshu.io/upload_images/15803937-cf5cc8e020d67f43.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)