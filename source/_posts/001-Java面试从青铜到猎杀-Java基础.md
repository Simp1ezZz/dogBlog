---
title: 001-Java面试从青铜到猎杀-Java基础
top_img: https://my-pic-picgo.oss-cn-shanghai.aliyuncs.com/index.jpg
comments: true
cover: https://my-pic-picgo.oss-cn-shanghai.aliyuncs.com/apex_bronze.webp
toc: true
toc_number: false
toc_style_simple: false
date: 2023-07-08 16:17:22
updated: 2023-07-08 16:17:22
tags: 
  - Java
  - 面试
  - Java基础
categories:
  - 学习
keywords: Java基础
description: 
---

# 001-Java面试从青铜到猎杀-Java基础

## 1. Java中基础数据类型

Java中基础数据类型有8种：

- 6种数字类型
  - 4种整形： `byte`,`short`,`int`,`long`
  - 2种浮点型：`float`,`double`
- 1种布尔类型: `boolean`
- 1种字符类型: `char`

| 基本类型 | 占用位数 | 占用字节数 | 默认值  |
| -------- | -------- | ---------- | ------- |
| byte     | 8        | 1          | 0       |
| short    | 16       | 2          | 0       |
| int      | 32       | 4          | 0       |
| long     | 64       | 8          | 0L      |
| float    | 32       | 4          | 0F      |
| double   | 64       | 8          | 0D      |
| char     | 16       | 2          | 'u0000' |
| boolean  | 1        |            | false   |

## 2. 重载与重写的区别

### 重载

- 重载发生在同一个类中，或者子父类之间。
- 方法名必须相同，参数类型，参数数量，参数顺序中有一个或多个不同。
- 返回值和访问修饰符可以不同。

### 重写

- 重写发生在子类继承父类的方法。
- 重写时方法名，参数列表必须相同。
- 返回值类型要小于等于父类。 eg:父类返回值是`List`类型，那子类重写父类方法，可以返回`ArrayList`类型，但不能返回`Collection`类型。要注意，如果父类的返回值类型是`void`或基本数据类型，那重写时就需要跟父类保持一致；只有父类的返回值类型是引用类型，重写时子类才可以返回该引用类型的子类。
- 抛出的异常要小于等于父类。eg: 父类方法抛出了`RuntimeException`,那子类重写父类方法时就不能抛出`NullPointException`,而可以抛出`Exception`
- 访问修饰符的范围要大于等于父类。 eg: 父类方法的访问修饰符为`protected`,那子类重写父类方法时就不能是`private`或`default`的，而可以是`public`的。
- `private`/`final`/`static`修饰的方法不能被重写，父类方法为`static`，子类中可以声明一个与父类方法名，参数列表相同的`static`方法，但并非是重写的。

## 3. 介绍一下面向对象

### 封装

封装就是把一个类的属性隐藏在内部，不允许直接方法类的属性，只提供外部访问的方法来操作属性。有了封装，我们可以不用关心类内部如何执行而直接调用提供的方法来实现我们需要的操作。如果某个属性不想让外界访问，我们就可以不提供其访问的方法，但如果一个类完全没有提供给外界访问的方法，那么这个类也就没有意义了。

### 继承

不同的类对象之间有一些共同点，也有其独有的特性。那么这些共同点就可以成为一个公共的父类，给不同的类去继承，不同的类只需要实现自己独有的特性即可，共同点均继承自父类。通过继承，我们可以快速的创建新的类，提高代码的复用性及程序的可维护性。

- 子类可以拥有父类的全部属性和方法（即使是私有属性或私有方法），但是私有属性和方法却不能被子类访问到，只是拥有。
- 子类可以新增自己独有的属性和方法，即对父类的扩展。
- 子类可以对父类的方法重新实现，即重写（多态）

### 多态

一个对象具有多种状态，表现为父类引用指向子类实例，比如

```java
List<String> list = new ArrayList<>();
```

## 4. 接口和抽象类的区别与联系

### 共同点

- 都不能被实例化
- 都可以有未实现的方法
- 都可以有默认实现的方法（`JDK 1.8`之后接口中可以有`default`方法）

### 区别

- 只能继承一个类，但却可以实现多个接口。

- 接口中的属性只能是`public static final`类型的，必须有初始值且不能被修改，一般省略掉这些修饰，如下，而抽象类的属性默认为`default`，可以和普通类一样随意定义或赋值。

  ```java
  public interface TestInterface {
      String a = ""; 	// 相当于 public static final String a = "";
  }
  ```

- 接口一般是对类的行为做出约束，实现这个接口就代表你有这个接口的能力；而抽象类一般是对代码的复用，强调的是所属关系。

## 5. final关键字的作用

- 修饰类时该类不能被继承
- 修饰方法时该方法不能被重写
- 修饰属性（变量）时该变量不能被修改，为常量
  - 如果该变量是基本数据类型，那么其为常量，不可被修改
  - 如果该变量是引用数据类型，因为实际存储的是对象的引用，所以是该引用不能被修改，即不能指向其他对象，但引用指向的对象是可以被修改的。

## 6. 深拷贝、浅拷贝及引用拷贝

### 引用拷贝

引用拷贝不创建新的对象，直接将要拷贝对象的地址值进行拷贝，如下：

```java
User user = new User("simplezzz","3");
User copyUser = user;	// 此处即为引用拷贝
```

### 浅拷贝

浅拷贝先会在堆上创建一个新的对象，将要拷贝对象中的属性赋值进去，但如果是引用类型的属性，那么只会将该属性的地址值赋值过去，而非创建一个新的对象。

浅拷贝的实现很简单，直接实现`Cloneable`接口并重写`clone`方法，拷贝时直接调用该类的`clone`方法即可，`clone`方法在`Object`类中是`protected`的，重写时需要改为`public`。

```java
public class Person implements Cloneable{
    // 当然对于引用类型String的name，拷贝后地址值也不会改变
	private String name;
    
    private Integer age;
    
    private Student son;

    @Override
    public Person clone() {
        try {
            return (Person) super.clone();
        } catch (CloneNotSupportedException e) {
            throw new AssertionError();
        }
    }
}
```

### 深拷贝

深拷贝与浅拷贝相同，都会创建一个新的对象，不同的是如果待拷贝对象中有引用类型的属性时，会再次创建一个新的对象，而不是直接地拷贝地址值。

深拷贝实现起来有多种方法

- 多层浅拷贝，在`clone`方法中对引用类型再次执行一次浅拷贝并赋值，层层循环。

    ```java
    public class Person implements Cloneable{
        public String name;

        public Integer age;

        public Student son;

        @Override
        public Person clone() {
            try {
                Person person = (Person) super.clone();
                person.son = son.clone();
                return person;
            } catch (CloneNotSupportedException e) {
                throw new AssertionError();
            }
        }
    }

    public class Student implements Cloneable{

        public String name;

        public School school;

        @Override
        public Student clone() {
            try {
                Student student = (Student) super.clone();
                student.school = school.clone();
                return student;
            } catch (CloneNotSupportedException e) {
                throw new AssertionError();
            }
        }
    }

    public class School implements Cloneable{
        public String schoolName;

        @Override
        public School clone() {
            try {
                return (School) super.clone();
            } catch (CloneNotSupportedException e) {
                throw new AssertionError();
            }
        }
    }
    ```

- 利用序列化进行深拷贝,拷贝的对象及其引用的属性都要实现`Serializable`接口

  ```java
  public class CloneUtils {
   
  	public static <T extends Serializable> T clone(T obj) {
  		T cloneObj = null;
  		try {
  			//写入字节流
  			ByteArrayOutputStream out = new ByteArrayOutputStream();
  			ObjectOutputStream obs = new ObjectOutputStream(out);
  			obs.writeObject(obj);
  			obs.close();
   
  			//分配内存，写入原始对象，生成新对象
  			ByteArrayInputStream is = new ByteArrayInputStream(out.toByteArray());
  			ObjectInputStream os = new ObjectInputStream(is);
  			cloneObj = (T) os.readObject();
  			os.close();
  		} catch (Exception e) {
  			e.printStackTrace();
  		}
  		return cloneObj;
  	}
  }
  
  public class Student implements Serializable {	// 也要实现序列化接口
      public String name;
  }
  
  public class Person implements Serializable {
      public String name;
      public Student son;
  
      public static void main(String[] args) {
          Person person = new Person();
          person.name = "simplezzz";
          Student student = new Student();
          student.name = "张三";
          person.son = student;
          Person clonedPerson = CloneUtils.clone(person);	// 深拷贝
          System.out.println(clonedPerson.son==person.son);	//false
      }
  }
  ```
  

## 7. ==和equals()的区别

- 两者都是用来比较是否相等的

- `==`如果比较的是基本数据类型，那么比较的就是值是否相等；如果比较的是引用数据类型，那么比较的就是其地址值是否相等。（其实都是比较值是否相等，只不过引用数据类型存储的值是引用地址值）

- `equals()`无法用于基本数据类型，只能用来判断两个对象是否相等。`Object`类提供了默认的`equals()`方法，实现的就是用`==`来判断，若不重写`equals()`方法，那`==`和`equals()`是等同的。

  ```java
  // Object类默认的equals()方法
  public boolean equals(Object obj) {
  	return (this == obj);
  }
  ```

- 一些类有默认重写的`equals`方法，比如`String`类，会去比较实际的值是否相等。

## 8. equals()和hashCode()

### hashcode()是什么

`hashCode()`的作用是用来获取对象的哈希码（散列码），一个`int`类型的数字。`hashCode()`定义在`Object`类中，所以所有的对象都有`hashCode()`方法。`hashCode()`方法在`Object`类中是`native`的本地方法。

```java
@IntrinsicCandidate
public native int hashCode();
```

> ⚠`hashcode()`方法在Oracle OpenJDK8中默认使用的是"使用线程局部状态来实现Marsaglia's shift-xor"的一种随机数算法，并非是对象在内存中的地址值或地址值转换而来。不同 JDK/VM 可能不同在 **Oracle OpenJDK8** 中有六种生成方式 (其中第五种是返回地址), 通过添加 VM 参数: -XX:hashCode=4 启用第五种。参考源码:
>
> - https://hg.openjdk.org/jdk8u/jdk8u/hotspot/file/87ee5ee27509/src/share/vm/runtime/globals.hpp（1127行）
> - https://hg.openjdk.org/jdk8u/jdk8u/hotspot/file/87ee5ee27509/src/share/vm/runtime/synchronizer.cpp（537行开始）

### 为什么需要hashCode()

在一些容器中，比如说`HashSet`,加入一个对象的流程是这样的：

1. 当元素加入`HashSet`时，会先调用其`hashCode`方法来判断对象插入的位置，同时也会来判断该位置上是否有其他对象。
2. 如果没有其他对象，那么`HashSet`就会认为没有重复的对象，可以插入。
3. 如果此时出现两个对象的`hashcode`相同，那么就会去调用对象的`equals()`方法，来判断这两个对象是否真正相同。如果相同，那么就认为这个对象重复了，不让其加入的操作成功；如果不相同，则将其重新散列到其他位置上。

其实`hashCode()`和`equals()`方法都是用来判断对象是否相同的。

相比于`equals()`方法，`hashCode()`方法在某些集合(比如`HashSet`,`HashMap`)中判断元素是否在容器中的效率更高，因为散列之后直接判断该位置是否有元素即可，而如果全用`equals()`,那就得遍历整个集合了。

`hashCode()`返回的是一个哈希函数，其不可避免的会有哈希冲突，所以判断是否重复时只能确定元素不在集合中，而不能确定元素是否重复，需要`equals()`精确判断。

### 为什么重写equals()方法时必须重写hashCode()方法

- 因为`hashCode()`和`equals()`方法都是用来判断对象是否相等的，如果两个对象的`equals()`方法判断相等，那么`hashCode()`方法返回的也必须是相等的（**反之不会，因为会有hash冲突**）。
- 重写`equals()`方法时，如果没有重写`hashCode()`方法，那么在一些集合中(比如`HashSet`)，就会出现二者`equals`本是相等的，但`hashCode`不同，导致可以重复插入了。

## 9. String、StringBuilder、StringBuffer

- 可变性：`String`是不可变的，其存放实际字符串的字符数组`value`被修饰为`private final`,而`StringBuilder`和`StringBuffer`继承自`AbstractStringBuilder`类，但是他的字符数组`value`并不是`final`的，而且提供有相应的修改字符串的方法，比如`append`。
- 线程安全性：`String`由于是不可变的，所以是线程安全的，而`StringBuffer`对于操作字符串的方法都添加了`synchronized`锁，所以是线程安全的，而`StringBuilder`没有添加锁，所以是非线程安全的。
- 性能：对于`String`来说，因为每次对于其的"改变"都是创建一个新的`String`对象，再将原指针指向该对象，所以性能最弱。而`StringBuilder`和`StringBuffer`都是对字符串本身的操作，在大数据量操作字符串的情况下性能要远远高于`String`。因为`StringBuffer`加了同步锁，所以性能会略有损失，在没有线程安全的问题下比`StringBuilder`损失10%~15%的性能。

> 我们知道被`final`修饰的类不能被继承，被修饰的方法不能被重写，***被修饰的变量如果是基本数据类型，则不能被修改，但如果是引用数据类型，是不能再指向其他对象(引用地址不能修改)***,但是我们可以修改其引用的对象。
>
> `String`类不可修改不仅仅只是因为存放数据的字符数组被修饰为了`final`，而且访问修饰符为`private`，且没有提供方法去修改这个字符数组中的内容。
>
> 又`String`类被设计为`final`的，导致其不可继承，禁止了通过子类继承来提供方法修改该字符串。

```java
public final class String
    implements java.io.Serializable, Comparable<String>, CharSequence,
Constable, ConstantDesc {

    @Stable
    private final byte[] value;

    ...

}
```

