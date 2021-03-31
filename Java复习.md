# JAVA
## Java的语法特征
    封装，继承，多态
    封装，把一个对象的属性私有化，提供一些可以被外界访问的属性的方法。
    继承，使用已存在的类的定义作为基础建立新类的技术，新类的定义可以增加新的数据或新的功能，也可以用父类的功能，但不能选择性的继承父类
    多态，在程序运行时才能确定引用变量所指向的具体类型和通过该引用变量发出的方法调用。两种形式实现多态，继承（多个子类对同一个方法的重写）和接口（实现接口并覆盖接口中的同一方法）
    * 重载 同样的方法，参数不同
    * 重写 子类继承父类相同的方法，输入相同，但可以有不同的响应，也就是重新写方法，参数和方法名要相同。
## String StringBuffer StringBuilder
    string的对象是不可变的，因为用final修饰的数组来保存字符串。
    stringBuffer对方法加了同步锁，是线程安全的，stringbuilder没加，所以非线程安全。
## 接口和抽象类的区别
    抽象是对类的抽象，接口是对行为的抽象。
    接口的方法默认public，接口中只有static、final变量。
    一个类可以实现多个接口，但只能实现一个抽象类。
## ==与equals
    ==的作用是判断两个对象的地址是不是相等。
    equals（）判断两个对象是否相等 string中的quals方法是被重写过的
## hashcode与equals
    重写equals必须重写hashcode（）
    hashcode（）用来获取哈希码，用来确定该对象在哈希表中的索引位置。Java中的任何类都有hashcode（）
    两个对象有相同的hashicode它们不一定是相等的。equals方法被覆盖过，hashcode方法也必须被覆盖。
    hashcode（）的默认行为是对堆上对象产生独特值
## ArrayList和LinkedList区别
    都是不同步的，都不保证线程安全。底层数据结构不同，array是object数组，linked是双向链表。
    查询和插入效率不同。
    arraylist空间浪费在列表结尾要预留一定空间，linkedlist每个元素都消耗更多空间。
## Hashmap和Hashtable的区别
    HashMap是非线程安全的，HashTable是线程安全的。要保证线程安全用concurrentHashmap。
    HashMap效率比Hashtable高。
    HashMap可以有null键值
    HashMap初始容量16每次扩充变为两倍
## HashMap和Hashset的区别
    hashset基于hashmap实现
    set仅存储对象。调用add（）方法向set中添加元素
