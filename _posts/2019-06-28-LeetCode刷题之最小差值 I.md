---
layout:     post           
title:      LeetCode刷题之最小差值 I
date:       2019-06-28
author:     码农先生
catalog: true
tags:
    - LeetCode
    - 算法
    - 审题


---

# LeetCode刷题之大的国家

> 本文在[CSDN](https://blog.csdn.net/m0_37344350)同步更新

> 难度：简单
> 链接：https://leetcode-cn.com/problems/smallest-range-i


来看题：

![image.png](https://upload-images.jianshu.io/upload_images/15803937-1d7a45e2a46eecf5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image.png](https://upload-images.jianshu.io/upload_images/15803937-3d8a646b176d1db3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image.png](https://upload-images.jianshu.io/upload_images/15803937-052c685a38a84862.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这道题就是我在朋友圈里吐槽“**审题半小时，做题两分钟**”的那道。

![image.png](https://upload-images.jianshu.io/upload_images/15803937-a27d1a27e877f0b9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image.png](https://upload-images.jianshu.io/upload_images/15803937-967c411ddd27cd4c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

可能是很久没做算法题了，反应有点迟钝，看了半天才看懂这道题的意思。

以示例3为例，我们把题干抄一遍：
给定了一个整数数组A = [1,3,6]，对于A中的每一个元素A[i]，我们可以选择任意x满足-3<=x<=3，并将x加到A[i]中，在此过程之中，我们得到一些数组B。返回B的最大值和B的最小值之间可能存在的最小差值。

这样一看，题目的意思很明显，A中的每个元素可以在-3和3之间任选一个元素想加，然后形成一个新的数组B。比如A中的元素都选择1想加，则新的B数组则为[2,4,6]；如果A中的元素都选择2相加，则新的B数组则为[3,5,7]，可以看出这两个新数组中最大值减去最小值，结果都是4。

但如果A数组的元素分别加上2,0,-3，我们可以看到B数组变成了[3,3,3]，这样就得到了示例3的正确答案0。

因此我们只要知道数组A的最大值和最小值相差多少，然后与2*K比较，如果这个差值比2*K还要大，那我们无论怎么想加得到的B数组的最大值和最小值都不可能相等，此时最佳的B数组最大值和最小值的差值为max[A]-min[A]-2*K；但如果max[A]-min[A]要小于2*K的话，我们通过加加减减可以得到一个元素全部一样的B数组，此时max[B]-min[B]当然是等于0喽。

所以解答步骤为：
1. 排序
2. 比较并给出答案

我的解答：
```java
    public int smallestRangeI(int[] A, int K) {
        Arrays.sort(A);
        return A[A.length-1] - A[0] > 2*K ? A[A.length-1] - A[0] - 2*K : 0 ;
    }
```

接下来的几期会分析一下常见的排序算法，特别是快速排序和归并排序，当然，对于java源码里的Arrays.sort()方法也会进行分析，来看看sun公司最后使用的排序算法性能到底怎么样。

先给大家几张维基百科的图片感受下，熟悉这些排序的朋友可以在脑海里模拟一下排序。

快速排序：
![Sorting_quicksort_anim.gif](https://upload-images.jianshu.io/upload_images/15803937-773e337ea77164db.gif?imageMogr2/auto-orient/strip)

冒泡排序：
![Bubble_sort_animation.gif](https://upload-images.jianshu.io/upload_images/15803937-bd4df3a02fec7298.gif?imageMogr2/auto-orient/strip)

选择排序：
![Selection_sort_animation.gif](https://upload-images.jianshu.io/upload_images/15803937-52d0d363fbbfca0e.gif?imageMogr2/auto-orient/strip)

归并排序：
![Merge_sort_animation2.gif](https://upload-images.jianshu.io/upload_images/15803937-36c0464bd9d5704c.gif?imageMogr2/auto-orient/strip)

插入排序：
![Insertion_sort_animation.gif](https://upload-images.jianshu.io/upload_images/15803937-483f4317918cb6bb.gif?imageMogr2/auto-orient/strip)

希尔排序：
![Sorting_shellsort_anim.gif](https://upload-images.jianshu.io/upload_images/15803937-75dfbaf13bfdcb06.gif?imageMogr2/auto-orient/strip)

堆排序：
![Sorting_heapsort_anim.gif](https://upload-images.jianshu.io/upload_images/15803937-1f1a93530fabee19.gif?imageMogr2/auto-orient/strip)

下期更精彩，我们下期见，我们下期见~

欢迎关注我的微信公众号：一辈子的码农先生，接下来会有非常多的干货总结，这也是我对自己几年工作的一种总结和交代。谢谢大家！

![微信公众号.jpg](https://github.com/MiracleTaoTao/miracletaotao.github.io/blob/master/_posts/image/my-QR-code2.jpg?raw=true)