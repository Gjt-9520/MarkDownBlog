---
title: "Java集合面试题"
date: 2024-09-09
description: ""
cover: https://github.com/Gjt-9520/MarkDownBlog/blob/main/source/coverImages/Bimage-135/Bimage125.jpg?raw=true
tags: ["JavaSE"]
category: "面试"
updated: 2024-09-10
  
top_group_index: 
---

# 集合类有哪些？以及日常开发中使用的集合类？

Java中的集合类主要分为两大类：Collection接口和Map 接口。前者是存储对象的集合类，后者存储的是键值对(key-value)。

Collection接口下又分为List、Set和Queue接口，每个接口有其具体实现类。

List下有ArrayList(基于动态数组，查询速度快，插入、删除慢)，LinkedList(基于双向链表，插入、删除快，查询速度慢)，Vector(线程安全的动态数组，类似于ArrayList，但开销较大)。

Set下有HashSet(基于哈希表，元素无序，不允许重复)，LinkedHashSet(基于链表和哈希表，维护插入顺序，不允许重复)，TreeSet(基于红黑树，元素有序，不允许重复)。

Queue下有PriorityQueue(基于优先队列，元素有序)，LinkedList(可以作为常规队列使用，先进先出)。

Map接口下有很多接口，比如HashMap，LinkedHashMap，TreeMap，Hashtable、ConcurrentHashMap。

日常开发中使用的集合类主要有ArrayList、HashMap、HashSet、ConcurrentHashMap等。

# 数组和链表的区别？

1. 数组是内存中连续的空间，而链表可以不连续。

2. 数组长度固定，如果需要扩展数组，需要重新开辟一片空间使用；而链表可以直接用指针指向不同的地址，扩展方便。

3. 在读取多的场最下适合用数组，数组可以直接用下标访问，而在插入和删除操作多的场景下适合用链表。

4. 链表需要额外的空间存储指针，占用的空间会比数组更多。

# List接口有哪些实现类？

1. ArrayList

基于动态数组，查询速度快 O(1)，插入删除慢 O(n)。

2. LinkedList

基于双向链表，插入删除快 O(1)，查询速度慢 O(n)。

3. Vector

基于线程安全的动态数组，类似于ArrayList，所有方法都加了synchronized锁，所以同步开销较大，性能较低。

4. CopyOnWriteArrayList

# CopyOnWriteArrayList是什么？

CopyOnWriteArrayList是基于线程安全的动态数组，但所有可变操作（如add、set等）都会创建一个新的数组。

写时复制技术：在写操作时，复制一个副本进行修改，然后将修改后的副本替换原来的数据结构。

写操作涉及复制整个数组，开销大，性能低；但读取操作完全无锁。因此适合读多写少的场景。

# ArrayList的扩容机制？

当ArrayList中的元素数量超过其当前容量时，会触发扩容机制。

默认情况下，ArrayList的初始容量为10。

当发生扩容时，ArrayList会创建一个新的数组，其容量为原数组的1.5倍，然后将原数组中的元素复制到新数组中。

复制过程是通过Arrays.copyof()方法实现的。

建议在创建ArrayList时，设置初始容量，减少扩容，提高性能。

# 如何移除list中的一个元素？

1. 使用Iterator，顺序向下，如果找到元素，则使用remove方法进行移除。

2. 倒序遍历List，如果找到元素，则使用remove方法进行移除。

3. 正序遍历List，如果找到元素，则使用remove方法进行移除，然后进行索引“自减”。

范例：

```java
package com.gujintao.others;

import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;

/**
 * @program: MvcProject
 * @description: 测试移除list中的一个元素
 * @author: gujintao
 * @create: 2024-10-26 14:19
 **/
public class TestRemoveList {
    public static void main(String[] args) {
        List<String> list = new ArrayList<>();
        list.add("A");
        list.add("B");
        list.add("C");
        list.add("D");
        // 打印结果：[A, B, C, D]
        System.out.println(list);

        // 通过迭代器遍历list
        Iterator<String> iterator = list.iterator();
        while (iterator.hasNext()) {
            String next = iterator.next();
            if (next.equals("A")) {
                iterator.remove();
            }
        }
        // 打印结果：[B, C, D]
        System.out.println(list);

        // 通过倒序遍历list
        for (int i = list.size() - 1; i >= 0; i--) {
            if (list.get(i).equals("B")) {
                list.remove(i);
            }
        }
        // 打印结果：[C, D]
        System.out.println(list);


        // 通过正序遍历list，索引自减防止跳过下一个元素
        for (int i = 0; i < list.size(); i++) {
            if (list.get(i).equals("C")) {
                list.remove(i);
                i--;
            }
        }
        // 打印结果：[D]
        System.out.println(list);
    }
}
```

# HashMap的扩容原理？

HashMap是基于哈希表的数据结构，用于存储键值对。

核心是将哈希值映射到数组索引的位置。

处理哈希冲突：Java8之前是数组+链表；Java8之后是数组+链表+红黑树。

当链表长度超过8，且数据总量超过64时，链表才会转成红黑树；否则仅仅是链表长度超过8，数据总量不超过64，只会触发扩容；当链表长度降到6以下，红黑树会重新退化成链表。

HashMap的初始容量为16，负载因子为0.75。这个组合是性能和空间之间找到的平衡。

当存储的元素数量超过16×0.75=12个时，就会触发扩容，容量×2。

# HashMap put的原理？

1. 取key的hashcode进行高位运算，返回hash值。

2. 如果hash数组为空，直接扩容。

3. 如果hash数组不为空，对hash进行取模运算计算，得到key-value在数组中的存储位置i
    如果table[i] == null，直接插入Node<key,value>
    如果table[i] != null，判断是否为红黑树。
    如果是红黑树，则判断TreeNode是否已存在，如果存在则直接返回oldnode并更新；不存在则直接插入红黑树。
    如果是链表，则判断Node是否已存在，如果存在则直接返回oldnode并更新；不存在则直接插入链表尾部。

# 为什么HashMap的底层数组长度为何总是2的n次方？

主要是使存入的数据分布均匀，减少冲突。

# 使用HashMap时，有哪些提升性能的技巧？

1. 合理设置HashMap的初始容量，避免扩容。

2. 适当调整HashMap的负载因子(默认是0.75，负载因子×容量=扩容阈值)。

3. hashCode均匀分布，以便快速查找键值对的位置，避免哈希冲突(多个键值对存在一个哈希桶里)。

4. 使用合适的数据结构：LinkedHashMap(插入顺序存储)、ConcurrentHashMap(高并发线程安全)、TreeMap(可以排序)。

# 什么是Hash碰撞？怎么解决？

Hash碰撞是指不同的数据通过哈希算法计算后，得到相同的哈希值, 映射到了哈希表的同一个位置发生“碰撞”。

解决方案：

1. 拉链法(链地址法): 将哈希表的每一个槽的位置变成一个链表,将哈希值相同的键存储到同一个链表中。

2. 开放寻址法: 如果出现碰撞,就寻找哈希表下一个可用的位置。

3. 再哈希法(双重哈希): 在出现碰撞时,使用第二个哈希函数计算新的索引位置,减少碰撞的概率。

# 为什么JDK1.8对HashMap进行了红黑树的改动？

JDK1.8中对HashMap引|入红黑树的改动，是为了优化在哈希冲突严重时的性能。

处理哈希冲突：Java8之前是数组+链表；Java8之后是数组+链表+红黑树。

通过将链表转换为红黑树，HashMap的查找、插入和删除操作的最坏时间复杂度从O(n)降低到O(log n)，提高了在极端情况下的效率，确保了更稳定的性能表现。

当链表长度超过8，且数据总量超过64时，链表才会转成红黑树；否则仅仅是链表长度超过8，数据总量不超过64，只会触发扩容；当链表长度降到6以下，红黑树会重新退化成链表。

# JDK1.8对HashMap的改动，除了红黑树还有哪些其他改动？

1. hash函数改成将key的hash码的高16位和低16为进行异或。

2. rehash扩容将数组长度改成2的次方，扩容为原来的2倍。

3. 使用尾插法代替头插法，避免出现多线程操作成环问题。

4. 插入时机改成先插入再判断size是否大于阈值大于再扩容。

# HashMap和HashTable的区别？

1. HashTable是线程安全，HashMap是非线程安全。

2. HashMap可以使用null作为key，而HashTable则不允许null作为key。

3. HashMap继承了AbstractMap，HashTable继承Dictionary抽象类。

4. HashMap的初始容量为16，HashTable初始容量为11，两者的填充因子默认都是0.75。HashMap扩容时是当前容量翻倍即:capacity×2，Hashtable扩容时是容量翻倍+1即:capacity×(2+1)。

HashTable已经弃用，ConcurrentHashMap就是用来替代HashTable的。

# HashMap和HashSet的区别？

1. HashMap

基于键值对的存储结构，每个元素由一个键和对应的值的组成。

保证键的唯一性，而值可以重复。

可以通过键来快速查找，插入和删除元素。

可以存储键值对，其中键和值可以是不同类型的对象。

2. HashSet

基于哈希表的存储结构，只存储元素的值。但实际上它是基于HashMap来实现的。

其中HashSet的元素作为HashMap的键存储，而所有的值都指向一个同一个特殊值，通常是null。

保证了元素的唯一性，不允许有重复元素。

只能通过元素的值来访问和操作。

# 什么是LinkedHashMap？

LinkedHashMap是HashMap的扩展版本，继承自HashMap，并且保留了键值对的插入顺序和访问顺序。

也是基于哈希表实现的，具有快速的查找、插入和删除性能。内部通过维护一个双向链表来记录元素的插入顺序（默认按照插入顺序排列，可修改accessOrder）和访问顺序。

使用场景：

1. 缓存实现：可以根据访问顺序移除最久未使用的元素，常用于LRU缓存。

2. 数据存储：需要保持元素插入顺序的场景。

# 什么是TreeMap？

TreeMap实现了NavigableMap接口，能够对map的key进行排序。

底层是一个数组，但是数组里面的元素是红黑树，数据存在这颗红黑树上面，key不能重复，但是value能重复。

# 什么是IdentityHashMap？

IdentityHashMap是对Hashmap的一种实现，它使用引用相等性验证key是否相等。

使用==来比较键是否相等，==比较的是对象的内存地址equals比较的是对象的内容，所以只有当两个对象的引用相等的时候才会判断key相等。

并且IdentityHashMap不是通过hashCode来计算哈希值的，而是通过System.identityHashcode来计算哈希值。

Identity是线程不安全的。

使用场景：

1. 想缓存对象且想避免使用equals方法带来的开销。

2. 希望通过对象的引用而不是内容的比较时使用。

# 什么是WeakHashMap？

WeakHashMap是Java中的一个特殊的Map实现，它使用弱引用(Weak Reference)作为键。

弱引用允许垃圾回收器在没有其他强引用指向该对象时回收它的内存。因此，当一个键不再被其他对象引用时，会自动删除与该键WeakHashMap相关联的条目。

常用于缓存(内存敏感场景)的实现。当缓存中的键不再被其他地方引用时，可以自动移除相应的缓存条目，节省内存，防止内存泄漏。

# ConcurrentHashMap在JDK1.8的改动？

1. 数据结构

JDK1.7时，ConcurrentHashMap使用的是Segment(分段锁)+HashEntry数组+链表的数据结构。

JDK1.8及其之后，使用的是数组+链表/红黑树的数据结构(与HashMap类似)。

2. 锁的类型与粒度

JDK1.7的分段锁(Segment)继承了ReentrantLock，Segment容量默认16，不会扩容，也就是默认只能有16个线程同时访问当前数据结构。

JDK1.8使用的是synchronized+CAS来保证线程安全的。对于空结点，使用CAS执行添加操作；对于非空结点则使用synchronized保证线程安全。

3. 渐进式扩容

JDK1.8后引入了渐进式扩容，降低了大数据量扩容情况下的开销。基本过程就是: 当我们的元素个数达到数组容量×负载因子(默认0.75)时，就会进行2倍扩容。

先创建一个容量为原数组2倍的新数组。然后后续每次有线程对当前数据结构进行操作(新增、修改等)，都会帮忙迁移部分的数组槽位上的数据(使用tranferlndex进行标记)，直到旧数组的数据完全迁移到新数组为止。

# ConcurrentHashMap的get方法是否需要加锁?

不需要。

因为get方法是读取数据操作,不对资源做处理,所以只要去保证我们每次读取的数据都是最新的即可，不需要加锁。

ConcurrentHashMap中get方法对于数组Node结点,是通过Unsafe方法getObjectVolatile来保证可见性的。而对于链表还有红黑树节点，是通过volatile关键字去修饰val和next结点来保证可见性。

# 为什么ConcurrentHashMap不支持键或值为null？而HashMap支持？

1. 避免多线程环境下产生歧义。

使用get方法查询的时候，无法确定是key键不存在还是value值为null。

2. 在多线程环境下，插入null值的同时另外一个线程 也进行查询就会出现竞争问题。

3. 可以避免对null值的特殊处理。

4. 在并发修改或删除时，避免出现不一致性和线程安全问题。

HashMap支持键或值为null的原因：

HashMap是单线程，所以能用containsKey方法判断有没有该key。但是并发下ConcurentHashMap。如果调用containsKey之前没有这个key，期望返回false，但是调用之前，可能有线程插入了这个key，结果返回tue了。

# 你遇到过ConcurrentModificationException吗?它是如何产生的?

在遍历集合对象时，修改集合的内容，会抛出这个异常。

它是JAva中的一种运行时异常，是为了检测并发修改问题。

解决方法：

1. 使用线程安全的集合，如Vector、CopyOnWriteArrayList、ConcurrentHashMap等或者使用Collections.SynchronizedList()方法对线程不安全的集合进行包装。

2. 使用synchronized关键字或ReentranLock锁显式同步。