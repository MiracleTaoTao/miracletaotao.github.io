## 1 普通数组使用Arrays.sort方法排序

在Arrays工具类中，**sort函数**可以对普通数组进行排序，如以下代码所示：
```java
  int[] hello = {18888,8888,5888,13888};
  Arrays.sort(hello);
  System.out.println(Arrays.toString(hello));//[5888, 8888, 13888, 18888]
		
  String[] heroName = {"成吉思汗","狄仁杰","孙尚香","虞姬"};
  Arrays.sort(heroName);
  System.out.println(Arrays.toString(heroName));//[孙尚香, 成吉思汗, 狄仁杰, 虞姬]
```
上面的代码分别对**int类型的数组**和**String类型的数组**进行了排序，对这个排序的算法感兴趣的同学可以看[这里](https://docs.oracle.com/javase/8/docs/api/java/util/Arrays.html#sort-int:A-)，**那么我们自定义的对象数组能不能也用此方法进行排序呢？**

首先我们可以新建一个类Hero1，给它定义两个属性：name和price，分别对应王者荣耀中英雄的名字和价格。然后在main函数中定义一个Hero1类型的对象数组heros1，接着调用**Arrays.sort(Object[] o)**方法。

```java
public class Hero1{
	private String name;	
	private int price;
    //省略set,get方法和构造函数	
}
```

```java
	Hero1[] heros1 = {
			new Hero1("成吉思汗",18888),new Hero1("狄仁杰",8888),
			new Hero1("孙尚香",5888),new Hero1("虞姬",13888)
		};
	Arrays.sort(heros1);//程序运行到这里抛出java.lang.ClassCastException异常
	System.out.println(Arrays.toString(heros1));
```
通过运行程序，我们可以很快发现程序抛出了**java.lang.ClassCastException**异常

![类型转换异常](https://upload-images.jianshu.io/upload_images/15803937-dba5ce8eeca56e76.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**那么为什么我们自定义的对象数组不能用sort方法排序呢？**通过查看源码和所抛出的异常，可以发现所有使用[sort](https://docs.oracle.com/javase/8/docs/api/java/util/Arrays.html#sort-java.lang.Object:A-)([Object](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html "class in java.lang")[] a)方法进行排序的对象都必须实现Comparable<T>接口。我们通过查看String的源码也可以观察到String类也实现了Comparable<T>接口。

```java
public final class String
    implements java.io.Serializable, Comparable<String>, CharSequence {}
```
## 2 自定义对象排序通过实现Comparable接口进行排序
在Hero1类中实现Comparable接口，然后重写它唯一的compareTo方法。

```java
public class Hero1 implements Comparable<Hero1>{

	private String name;
	
	private int price;
        
    //省略set,get方法和构造函数	

	@Override
	public int compareTo(Hero1 o) {
	    //这里如果不处理系统会抛出空指针异常
		if(o == null) {
			throw new RuntimeException("对象为空！");
		}
		
		if(this.price > o.price) {
			return 1;
		}else if(this.price < o.price) {
			return -1;
		}
		return 0;
	}
}
```
Hero1类实现Comparable接口后排序正常实现。
```java
	Hero1[] heros1 = {
			new Hero1("成吉思汗",18888),new Hero1("狄仁杰",8888),
			new Hero1("孙尚香",5888),new Hero1("虞姬",13888)
		};
	Arrays.sort(heros1);
	System.out.println(Arrays.toString(heros1));//[Hero1 [name=孙尚香, price=5888], Hero1 [name=狄仁杰, price=8888], Hero1 [name=虞姬, price=13888], Hero1 [name=成吉思汗, price=18888]]
```
## 3 自定义类不实现Comparable接口也有办法进行排序
**Comparable接口**要求自定义类主动去实现，但按照面向对象原则，**代码需要对修改关闭，对扩展开发**，那么在不破坏自定义类结构的前提下，我们是否有方法对类进行排序呢？

答案是肯定的，我们可以新建一个自定义的排序类，这个类实现Comparator接口，然后通过调用Arrays.[sort](https://docs.oracle.com/javase/8/docs/api/java/util/Arrays.html#sort-T:A-java.util.Comparator-)(T[] a, [Comparator](https://docs.oracle.com/javase/8/docs/api/java/util/Comparator.html "interface in java.util")<? super T> c)方法进行排序。

新建一个Hero2类，属性和Hero1一模一样，但Hero2类没有实现Comparable接口。
```java
public class Hero2{
	private String name;
	private int price;
	//省略构造函数及set，get方法
}
```
接着新建一个自定义的排序类，这个类需要实现**Comparator**接口，然后在里面重写compare方法，逻辑和之前一样，但这个函数的入参为两个对象，同样是第一个对象大则返回1，第二个对象大则返回-1，相等返回0。
```java
public class CustomComparator implements Comparator<Hero2>{
	@Override
	public int compare(Hero2 o1, Hero2 o2) {
		if(o1 == null || o2 == null) {
			throw new RuntimeException("对象为空！");
		}
		
		if(o1.getPrice() > o2.getPrice()) {
			return 1;
		}else if(o1.getPrice() < o2.getPrice()) {
			return -1;
		}
		return 0;
	}
}
 ```
建好自定义的排序类后便可调用Arrays.[sort](https://docs.oracle.com/javase/8/docs/api/java/util/Arrays.html#sort-T:A-java.util.Comparator-)(T[] a, [Comparator](https://docs.oracle.com/javase/8/docs/api/java/util/Comparator.html "interface in java.util")<? super T> c)方法进行排序。

```java
	Hero2[] heros2 = {
			new Hero2("成吉思汗",18888),new Hero2("狄仁杰",8888),
			new Hero2("孙尚香",5888),new Hero2("虞姬",13888)
		};
		
	Arrays.sort(heros2, new CustomComparator());
	System.out.println(Arrays.toString(heros2));//[Hero1 [name=孙尚香, price=5888], Hero1 [name=狄仁杰, price=8888], Hero1 [name=虞姬, price=13888], Hero1 [name=成吉思汗, price=18888]]
```
对象数组排序的讲解就到这了，欢迎大家访问我的专题[编程-Java基础](https://www.jianshu.com/nb/33193706)，尽管目前文章还比较少，但我会慢慢更新下去的。

