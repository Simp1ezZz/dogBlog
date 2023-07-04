# Java基础面试题

## 1. equals()和hashcode()的关系

equals方法一般用来比较对象是否相等，hashcode方法一般用来返回对象的hash码值

equals方法具有以下四个特性：

1. 自反性：对于任意不为`null`的引用值`x`，`x.equals(x)`的值一定为`true`;

2. 对称性：对于任意均不为`null`的引用值`x`和`y`,`x.equals(y)`结果为`true`时，`y.equals(x)`的结果也必然为`true`；

3. 传递性：对于任意均不为`null`的引用值`x、y、z`，如果`x.equals(y)`为`true`，`y.equals(z)`为`true`，那么`x.equals(z)`的结果也必然为`true`；

4. 一致性：对任意不为`null`的引用值`x`和`y`，如果`x`和`y`的信息没有修改过，那么多次调用`x.equals(y)`时，要都返回相同的值;


一般规定，重写equals方法也必须重写hashcode方法，因为equals和hashcode方法都被用于判断两个对象是否相等，要保持相同的逻辑。

## 2. == 和 equals()的区别

1. 对于基本数据类型，可以用`==`号来判断两个值是否相等，基本数据类型并非类，没有equals方法。
2. 对于引用数据类型，`==`号用来判断两个引用地址是否相等，而`equals()`方法则可以根据不同的方法实现，通过对象属性等来判断两个对象是否相等。
3. 如果没有重写，默认的`equals()`方法内部其实就是调用`==`来判断两个对象的地址是否相等。
4. 一些常用的类其实已经重写了`equals()`方法，比如`String`类，就是通过判断字符串的每个字符是否全部一样来判断两个字符串是否相等的。

## 3. 重载和重写的区别

1. 重载和重写都是针对于方法来说的
2. 重载发生在同一个类内，类内可以有同名的方法，但是方法的参数列表个数不同或者参数列表的类型不同或者参数列表的顺序不同，仅仅有不同的返回值类型不可以重载。
3. 重写发生在类与类之间，子类继承父类可以重写父类的方法。
   1. 重写要求方法名、参数列表完全相同
   2. 重写方法的返回值必须小于等于父类的返回值，即父类为返回值为`Number`，重写的方法返回值可以为`Integer`但是不能为`String`。
   3. 重写方法抛出的异常也要小于等于父类，比如父类抛出`IllegalArgumentException`,那子类可以抛出`NumberFormatException`但不能抛出`RuntimeException`。
   4. 重写方法的访问修饰符必须大于等于父类,且private的方法无法被重写。比如父类的方法是`protected`的，那子类可以为`public`，反之不可以。

## 4. List和Set的区别

1. `List`和`Set`都是接口，都继承自`Collection`接口。
2. `List`是有插入顺序的，里面的元素可以重复，可以存在多个`Null`值，可以通过迭代器iterator访问到各个元素或者通过下标直接访问。
3. `Set`是无序的，元素不可重复，仅可以存在一个Null值，只能通过迭代器iterator遍历元素。
4. 通常我们说的Set是无序的其实对应的是其下的HashSet实现，Set其实也可以保证其插入顺序或者自然顺序，比如`LinkedHashSet`就是插入有序的，`TreeSet`就是有自然顺序的。（自然顺序指comparator接口保证的顺序，比如String就是a-z这种）
5. HashSet是通过`hashcode()`和`equals()`方法来确定元素是否重复的，两个hashcode不同，则不重复，如果hashcode相同，会再调用equals方法最终判断是否重复。底层其实是一个HashMap，把插入元素作为HashMap的key，一个用于占位的虚拟值`PRESENT`作为value，因此它可以存入一个Null值。

## 5. ArrayList和LinkedList的区别

1. ArrayList和LinkedList都实现了List接口，都是插入有序且元素可重复的。
2. ArrayList底层是数组，而LinkedList底层是链表。
3. 数组对于随机读取快，随即插入删除慢，因为需要移动元素，而链表对于随机插入删除快，随机读取慢，因为需要从头到尾遍历。
4. 实际上如果仅仅在末尾插入删除，那ArrayList并不慢，而且大多数使用情况就是在末尾插入。
   1. LinkedList不仅实现了List接口，还实现了Deque接口，即它还可以作为一个双端队列来使用。

## 6. 聊一聊HashMap

1. HashMap是一个Key-Value形式的集合，在工作中十分常用。
2. HashMap在JDK1.8和JDK1.8之前在实现上有一些不同，简单来说主要有一下几点：
   1. jdk1.7的时候底层采用的是数组+链表的结构来实现的，而jdk1.8采用的是数组+链表+红黑树来实现，之所以要引入红黑树是为了在链表过长时加速查询的效率。
   2. jdk1.7的时候对于链表的插入采用的是头插法，而在1.8采用的是尾插法，头插法有时会存在循环链表的问题，而改为了尾插法。
   3. jdk1.8相较于jdk1.7，简化了散列函数
3. HashMap`put`元素流程（jdk1.8）：
   1. 计算key的hash值，通过对key的hashcode进行无符号右移16位后再按位异或`(key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16)`
   2. 判断当前的hashmap是否是空的，如果为空，则进行初始化。
   3. 

## fail-fast与fail-safe

## 红黑树简述

## CopyOnWriteArrayList原理

1. ArrayList在多线程写入的时候会有并发安全问题，同时写的话可能会造成数据的覆盖
2. CopyOnWriteArrayList内部加了可重入锁，防止了并发写入时的问题。
3. 同时CopyOnWriteArrayList会在写入数据之前将原数组复制一份，大小+1，并在这个新数组上进行操作，操作完成后将原数组替换为新数组。这样就可以在写入的同时不影响读数据，提高性能。

