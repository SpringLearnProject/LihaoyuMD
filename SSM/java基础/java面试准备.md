# java面试准备

## 基础数据结构：

![image-20200825135638735](C:\Users\lidxk\AppData\Roaming\Typora\typora-user-images\image-20200825135638735.png)

stack：堆栈

Queue：队列

![image-20200825135835842](C:\Users\lidxk\AppData\Roaming\Typora\typora-user-images\image-20200825135835842.png)



## 集合：

==集合==：用于存储无重复数据的高效数据结构：

==映射表==：功能类似于目录，使用键值对进行快速查询和获取值的功能。>

> 集合的Java源代码继承关系==i==inteable->==i==collection->==i==set->==c==hashset
>
> 

### List：

list接口定义了两个方法：get（int index）：获得指定索引位置的元素

set：（int index,Object obj）:将集合中指定索引位置的对象修改为指定的对象。

> list接口实现类：ArrayList与LinkedList

1. ArrayList：实现可变的数组，允许保存所有的元素，根据索引快速访问，==缺点==是向指定的索引位置插入对象或删除对象的速度较慢。
2. LinkeList：链表结构，==优点==：方便插入删除，但访问效率低

### Collection

![image-20200825141031848](C:\Users\lidxk\AppData\Roaming\Typora\typora-user-images\image-20200825141031848.png)

### List

![image-20200825155403649](C:\Users\lidxk\AppData\Roaming\Typora\typora-user-images\image-20200825155403649.png)



### Set

HashSet：由哈希表（hashMap）支持，不保证顺序不变

TreeSet：不光实现Set接口，还实现了java.util.SortedSet接口，可以实现Set集合在遍历集合时按照顺序递增排序，也可以指定比较器递增，

### Map：

map没有继承Collection接口，其提供的是Key和Value的关系映射，key不相同且只能映射一个value,

![image-20200901185720347](C:\Users\lidxk\AppData\Roaming\Typora\typora-user-images\image-20200901185720347.png)

![image-20200901190258646](C:\Users\lidxk\AppData\Roaming\Typora\typora-user-images\image-20200901190258646.png)



## i/o：

## 多线程:

## 并发：

## 集合与映射表：

## 基本算法：

## 树+图：

## 网络编程：

## 数据库：

### 关系型：mysql

### 非关系型：redis+nosql

## 异常与日志：

