# 面试题回顾,目前更新到2021年9月1日

# 一. OC语法
## 面向对象
### 1. 一个NSObject对象占用多少内存？
1. 系统分配了16个字节给NSObject对象（通过malloc_size函数获得）
2. 但NSObject对象内部只使用了8个字节的空间（64bit环境下，可以通过class_getInstanceSize函数获得）

### 2. 对象的isa指针指向哪里？
1. instance对象的isa指向class对象
2. class对象的isa指向meta-class对象
3. meta-class对象的isa指向基类的meta-class对象

### 3. OC的类信息存放在哪里？
1. 成员变量的具体值，存放在instance对象
2. 对象方法、属性、成员变量、协议信息，存放在class对象中
3. 类方法，存放在meta-class对象中

### Class内存结构是怎样的
![objc_class的结构](https://github.com/JW-chenjingwei/iOSInterview/blob/main/objc_class%E7%9A%84%E7%BB%93%E6%9E%84.png)
## KVO
### 1. iOS用什么方式实现对一个对象的KVO？(KVO的本质是什么？)
1. 利用RuntimeAPI动态生成一个子类，并且让instance对象的isa指向这个全新的子类
2. 当修改instance对象的属性时，会调用Foundation的_NSSetXXXValueAndNotify函数
3. willChangeValueForKey:
   父类原来的setter
   didChangeValueForKey:
4. 内部会触发监听器（Oberser）的监听方法( observeValueForKeyPath:ofObject:change:context:）

### 2. 如何手动触发KVO？
手动调用willChangeValueForKey:和didChangeValueForKey:

### 3. 直接修改成员变量会触发KVO么？
不会触发KVO

## KVC
### 1.直接修改成员变量会触发KVO么？
不会触发KVO

### 2. KVC实现原理

##  Category
### 1. Category的实现原理
1. Category编译之后的底层结构是struct category_t，里面存储着分类的对象方法、类方法、属性、协议信息
2. 在程序运行的时候，runtime会将Category的数据，合并到类信息中（类对象、元类对象中）

### 2. Category和Class Extension的区别是什么？
1. Class Extension在编译的时候，它的数据就已经包含在类信息中
2. Category是在运行时，才会将数据合并到类信息中

### 3.Category中有load方法吗？load方法是什么时候调用的？load 方法能继承吗？
有load方法
load方法在runtime加载类、分类的时候调用
load方法可以继承，但是一般情况下不会主动去调用load方法，都是让系统自动调用

### 4. load、initialize方法的区别什么？它们在category中的调用的顺序？以及出现继承时他们之间的调用过程？
1. load:类加载到内存时调用
2. initialize :类第一次被使用\第一次被方法调用时会调用

### 5. Category能否添加成员变量？如果可以，如何给Category添加成员变量？
不能直接给Category添加成员变量，但是可以间接实现Category有成员变量的效果

## Block
### 1. block的原理是怎样的？本质是什么？
封装了函数调用以及调用环境的OC对象

### 2. __block的作用是什么？有什么使用注意点？

block的属性修饰词为什么是copy？使用block有哪些使用注意？
block一旦没有进行copy操作，就不会在堆上
使用注意：循环引用问题

block在修改NSMutableArray，需不需要添加__block？


