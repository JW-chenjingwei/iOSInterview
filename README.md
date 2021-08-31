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

### 4. Class内存结构是怎样的
![objc_class的结构](https://github.com/JW-chenjingwei/iOSInterview/blob/main/objc_class%E7%9A%84%E7%BB%93%E6%9E%84.png)
## KVO
### 1. iOS用什么方式实现对一个对象的KVO？(KVO的本质是什么？)
1. 利用RuntimeAPI动态生成一个子类，并且让instance对象的isa指向这个全新的子类 `NSKVONotifying_类名`
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
1. 通过Runtime加载某个类的所有Category数据
2. 把所有Category的方法、属性、协议数据，合并到一个大数组中,后面参与编译的Category数据，会在数组的前面
3. 将合并后的分类数据（方法、属性、协议），插入到类原来数据的前面


### 2. Category和Class Extension的区别是什么？
1. Class Extension在编译的时候，它的数据就已经包含在类信息中
2. Category是在运行时，才会将数据合并到类信息中

### 3.Category中有load方法吗？load方法是什么时候调用的？load 方法能继承吗？
有load方法
load方法在runtime加载类、分类的时候调用
load方法可以继承，但是一般情况下不会主动去调用load方法，都是让系统自动调用

### 4. load、initialize方法的区别什么？它们在category中的调用的顺序？以及出现继承时他们之间的调用过程？
1. +load方法会在runtime加载类、分类时调用
2.每个类、分类的+load，在程序运行过程中只调用一次
3. 调用顺序
   先调用类的+load
   按照编译先后顺序调用（先编译，先调用）
   调用子类的+load之前会先调用父类的+load
3.2再调用分类的+load
   按照编译先后顺序调用（先编译，先调用）

4. +initialize方法会在类第一次接收到消息时调用
调用顺序
先调用父类的+initialize，再调用子类的+initialize
(先初始化父类，再初始化子类，每个类只会初始化1次)

5. +initialize和+load的很大区别是，+initialize是通过objc_msgSend进行调用的，所以有以下特点
如果子类没有实现+initialize，会调用父类的+initialize（所以父类的+initialize可能会被调用多次）
如果分类实现了+initialize，就覆盖类本身的+initialize调用


### 5. Category能否添加成员变量？如果可以，如何给Category添加成员变量？
不能直接给Category添加成员变量，但是可以间接实现Category有成员变量的效果

### 6. Category的底层结构
![Category的底层结构](https://github.com/JW-chenjingwei/iOSInterview/blob/main/Category%E7%9A%84%E5%BA%95%E5%B1%82%E7%BB%93%E6%9E%84.png)

## Block
### 1. block的原理是怎样的？本质是什么？
封装了函数调用以及调用环境的OC对象

### 2. __block的作用是什么？有什么使用注意点？

block的属性修饰词为什么是copy？使用block有哪些使用注意？
block一旦没有进行copy操作，就不会在堆上
使用注意：循环引用问题

block在修改NSMutableArray，需不需要添加__block？


