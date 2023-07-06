---
title: 001-Java从白银到大地球-Collection
date: 2021-05-15 13:26:55
tags: 
 - Java
 - 从白银到大地球
 - 集合
 - Collection
category: 学习
cover: https://my-pic-picgo.oss-cn-shanghai.aliyuncs.com/202307052245539.webp
---

# Java从白银到大地球-Collection

{% note info flat %}集合是Java中最经常用得东西，也是面试基本必问的知识点，需要我们重点掌握{% endnote %}

## 1. 集合（Collection）

​	在日常生活中，我们用衣柜来存放衣服，用冰箱来存放菜饭等等，这种`用来存放具有相似属性的东西的容器`我们就可以称之为一个`集合`。

​	我们可以从衣柜里面取出衣服，把衣服放到衣柜里，也可以在衣柜里挑选我们想要的衣服。相应的，我们的集合理应也具有这样的最基础的功能：`集合中的元素可供读取、添加以及删除`。

---

### 1.1 为什么需要集合

​	集合是用来存放一组数据的，在C语言中，我们都是用`数组`来存一组数据，通过指针来取数据，看似使用数组也可以完成这样的操作呀。但是Java是一门面向对象的语言，我们存放的数据基本上都是对象，当然Java中对象数组也可以用来存对象，但是数组的长度是不可变的，这就造成使用上的麻烦。

### 1.2 集合与数组的区别

- 数组的长度不可变，而集合的长度是可变的
- 数组可以存储基本类型和引用类型，而集合只能存储引用类型*(可以向集合内存储基本类型，但是实际上存储基本类型时会自动装箱成相应的引用类型)*

### 1.3 Java中集合的功能及分类

- 集合可以用来存储元素，但是存储以及使用时的功能需求却有所不同。
    - 存放的元素是否可以重复
    - 存放的元素是否可以按序排列
    - 是否可以任意取出元素
    - ...........

- 在Java中根据这些不同的功能需求，提供了很多不同的集合类，这些类虽然具体的功能不同，使用的数据结构不同，但是却拥有相同的共性---`可以存放删除以及取出元素`

- 将这一共性向上抽取，就有了`Collection`，所有集合类都继承自Collection。

- 集合类有很多，但是常用的集合类并不多，常用集合类及关系如下图

    ![常用集合类](https://my-pic-picgo.oss-cn-shanghai.aliyuncs.com/202307070054927.webp)

---

### 1.4 Collection接口

#### 1.4.1 接口中的全部方法 

![Collection接口方法](https://my-pic-picgo.oss-cn-shanghai.aliyuncs.com/202307070054062.webp)

#### 1.4.2 基础方法简单介绍

1. 添加元素

    ```java
    boolean add(Object obj)	//向集合内添加一个元素
    boolean addAll(Collection c)	//向集合内添加一个集合的全部元素
    ```

2. 删除元素

    ```java
    void clear()	//清空该集合，即删除该集合内全部元素
    boolean remove(Object obj)	//删除元素obj
    boolean removeAll(Collection c) //删除一个集合的元素，只要有其中一个元素被删除，就返回true
    ```

3. 判断

    ```java
    boolean isEmpty()	//判断该集合内是否有元素
    boolean contains(Object obj)	//判断该集合内是否有该元素
    boolean containsAll(Collection c) 	//判断该集合内是否有c集合的全部元素，只有含有全部元素才返回true
    ```

4. 获取元素

    ```java
    Iterator<E> iterator() //获取迭代器，然后使用迭代器可以遍历集合，获取元素
    ```

5. 长度

    ```java
    int size()	//获取集合大小，即集合中元素个数
    ```

6. 交集

    ```java
    boolean retainAll(Collection c)	//保留该集合中包含在指定集合c中的元素，换句话说，也就是删除该集合中不在集合c中的元素，即两个集合取交集，结果保留在该集合中，如果该集合内元素有改变，则返回true
    ```

7. 转换成数组

    ```java
    Object[] toArray()	//返回包含此集合内所有元素的数组，如果该集合的迭代器可以保证元素返回有序，那么会以相同的顺序返回。
    ```

---

### 1.5 迭代器(Iterator)

- 查看源码我们发现Collection接口继承自Iterable接口

![Collection接口继承自Iterable接口](https://my-pic-picgo.oss-cn-shanghai.aliyuncs.com/202307070054406.webp)

- 点进Iterable接口，我们发现有一个iterator()方法，该方法返回一个Iterator

![Iterable接口](https://my-pic-picgo.oss-cn-shanghai.aliyuncs.com/202307070054276.webp)

- 继续点进Iterator接口，发现该接口有三个基础方法，但是三个方法均没有具体实现，只能找一找Collection接口的下的类有没有实现该方法的。

![Iterator接口及基础方法](https://my-pic-picgo.oss-cn-shanghai.aliyuncs.com/202307070054722.webp)

- 在ArrayList类里发现了一个内部类实现了Iterator接口，通过实现方法来看，迭代器实际上就是在遍历集合中的元素

![ArrayList中实现了Iterator的内部类](https://my-pic-picgo.oss-cn-shanghai.aliyuncs.com/202307070054203.webp)

- 迭代器中方法简介
    - boolean hasNext()：迭代器中是否还有元素，一般用于循环条件判断。
    - Object next()：返回迭代中的下一个元素，如果没有下一个元素则抛出`NoSuchElementException`异常。
    - void remove()：从迭代器中移除此次迭代中的元素，`每次只能在调用next方法之后调用一次该方法`,如果在调用next方法之前调用了该方法，或者是在一次迭代中多次调用了该方法，会抛出`IllegalStateException`异常。有些集合类不支持该方法，则会抛出`UnsupportedOperationException`异常

> 所以说遍历集合可以通过集合的Iterator迭代器来遍历，而迭代器的实现都是由相应集合以内部类的方式来实现的。

---

## 2. List集合简介

​	在Collection介绍中我们已经说了，Collection下常用的就两类集合，List和Set，这里我们简单介绍一下List集合

### 2.1 List集合特点

- 点进List的源码，我们可以看到注释的前两段就说明了List集合的特点：`有序`和`可重复`

    ![List集合全部方法及JavaDoc](https://my-pic-picgo.oss-cn-shanghai.aliyuncs.com/202307070055028.webp)

- List相较于Collection，多了很多方法，但基本上都是`根据下标来获取添加和修改元素,以及获取某个元素的下标`。

- 多了一个`listIterator()`方法，该方法返回了`ListIterator`接口，该接口继承自`Iterator`,点进源码，发现相比于Iterator，该接口又多了一些方法，让迭代器可以`向前遍历,添加元素,修改元素,获取元素下标`

    ![](https://my-pic-picgo.oss-cn-shanghai.aliyuncs.com/202307070055656.webp)

- 多了一个`sort(Comparator c)`方法，用来根据指定的比较器Comparator来对列表进行排序

### 2.2 List集合常用子类

- ArrayList：底层数据结构为数组，线程不安全
- LinkedList：底层数据结构为双向链表，线程不安全
- Vector：底层数据结构为数组，线程安全

> 这里只简单列举出三个常用子类，具体细节之后的文章中会分别详细介绍

---

## 3. Set集合简介

### 3.1 Set集合特点

- 从源码中可以发现，Set集合并没有比Collection集合多出方法，且注释中直接说明了Set集合的特点：`元素是不可重复的`

![](https://my-pic-picgo.oss-cn-shanghai.aliyuncs.com/202307070055127.webp)

### 3.2 Set集合常用子类

- HashSet：底层数据结构是哈希表，其实就是每个元素为一个链表的数组。
- TreeSet：底层数据结构是红黑树（一种平衡二叉树），可保证元素的排列顺序。
- LinkedHashSet：底层数据结构由哈希表和链表组成。

---

## 4. 总结

- Collection接口作为集合的父类，提供了一些增删改查基础方法，而对应的子类又根据自己不同的功能需求，来实现这些方法或者添加新的方法，通过迭代器可以遍历集合中的元素，每种集合都有提供对应于自己数据结构及功能的迭代器。

- 这篇文章只是简单介绍了Collection接口及其下子类，之后的文章将详细介绍每个常用子类。

- 每个不同功能需求的集合放在一起，自底向上一层层提取出通用属性及方法，最后就得到了最基础的父类，而我们学习就是要自顶向下，先学习基础公共内容，在向下一层一层，深入细节。

---

**我是SimpleZzz，一个正努力改变自己的打工人。**