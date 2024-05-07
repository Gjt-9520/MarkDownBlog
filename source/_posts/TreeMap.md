---
title: "TreeMap"
date: 2024-03-24
description: ""
cover: https://github.com/Gjt-9520/Resource/blob/main/Aimage-135/Aimage107.jpg?raw=true
tags: ["Java SE","集合"]
category: "学习笔记"
updated: 2024-03-25
swiper_index: 
top_group_index: 
---

# 特点

特点都是**由键决定的:可排序**、不重复、无索引                      
这里的可排序指的是对键进行排序                         
默认是按照键的从小到大进行排序,也可以规定键的排序规则                  

## 底层原理

基于**红黑树**的数据结构实现排序的,增删改查性能都较好

## 排序规则

1. 对于数值类型:Integer、Double默认按照从小到大的顺序进行排序
2. 对于字符、字符串类型:按照字符在ASCII码表中的数字升序进行排序

# 两种比较方式

**使用原则:默认使用第一种,如果第一种不能满足当前需求,就使用第二种**

**如果两种比较方式都存在,系统会优先调用第二种比较方式**

## 默认排序/自然排序

JavaBean类实现Comparable接口指定比较规则

`return this.getAge() - o.getAge();`                 
其中this表示要添加的元素,o表示已经在红黑树中存在的元素

返回值:
1. 负数:认为要添加的元素是小的,存左边
2. 正数:认为要添加的元素是大的,存右边
3. 0:认为要添加的元素已经存在,舍弃

## 比较器排序

创建TreeSet对象的时候,传递比较器Comparetor指定规则

练习:

键:整数表示id              
值:字符串表示商品名称                 
要求:按照id的升序排列,按照id的降序排列

```java
import java.util.Comparator;
import java.util.LinkedHashMap;
import java.util.TreeMap;
import java.util.function.BiConsumer;

public class Test {
    public static void main(String[] args) {
        // 默认是升序,不传递比较器Comparator指定规则也是升序
        TreeMap<Integer, String> map1 = new TreeMap<>(new Comparator<Integer>() {
            @Override
            public int compare(Integer o1, Integer o2) {
                return o1 - o2;
            }
        });
        map1.put(1, "冬瓜");
        map1.put(2, "西瓜");
        map1.put(3, "黄瓜");
        map1.put(4, "苦瓜");
        map1.forEach(new BiConsumer<Integer, String>() {
            @Override
            public void accept(Integer integer, String s) {
                // 打印结果:"1,冬瓜"
                // 打印结果:"2,西瓜"
                // 打印结果:"3,黄瓜"
                // 打印结果:"4,苦瓜"
                System.out.println(integer + "," + s);
            }
        });
        System.out.println();

        TreeMap<Integer, String> map2 = new TreeMap<>(new Comparator<Integer>() {
            @Override
            public int compare(Integer o1, Integer o2) {
                return o2 - o1;
            }
        });
        map2.put(1, "冬瓜");
        map2.put(2, "西瓜");
        map2.put(3, "黄瓜");
        map2.put(4, "苦瓜");
        map2.forEach(new BiConsumer<Integer, String>() {
            @Override
            public void accept(Integer integer, String s) {
                // 打印结果:"4,苦瓜"
                // 打印结果:"3,黄瓜"
                // 打印结果:"2,西瓜"
                // 打印结果:"1,冬瓜"
                System.out.println(integer + "," + s);
            }
        });
    }
}
```

练习:

键:学生对象
值:籍贯
要求:按照学生年龄的升序排列,年龄一样按照姓名的字母排列,同姓名年龄视为同一个人

```java
import java.util.Objects;

public class Student {
    private String name;
    private int age;

    public Student() {
    }

    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    @Override
    public String toString() {
        return "Student{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}
```

```java
import java.util.*;
import java.util.function.BiConsumer;

public class StudentTest {
    public static void main(String[] args) {
        Student s1 = new Student("zhansan", 23);
        Student s2 = new Student("lisi", 24);
        Student s3 = new Student("wangwu", 24);
        Student s4 = new Student("wangwu", 24);
        TreeMap<Student, String> map = new TreeMap<>(new Comparator<Student>() {
            @Override
            public int compare(Student o1, Student o2) {
                int ageSort = o1.getAge() - o2.getAge();
                return ageSort == 0 ? o1.getName().compareTo(o2.getName()) : ageSort;
            }
        });
        map.put(s1, "北京");
        map.put(s2, "上海");
        map.put(s3, "广州");
        map.put(s4, "深圳");

        // Lambda表达式
        map.forEach((student, value) ->
                // 打印结果:"姓名:zhansan,年龄:23,籍贯:北京"
                // 打印结果:"姓名:lisi,年龄:24,籍贯:上海"
                // 打印结果:"姓名:wangwu,年龄:24,籍贯:深圳"
                System.out.println("姓名:" + student.getName() + ",年龄:" + student.getAge() + ",籍贯:" + value));
    }
}
```

练习:

需求:字符串"aababcabcdabcde"                
请统计字符串中每一个字符出现的次数,并按照以下的格式输出                  
输出字符串:"a(5)b(4)c(3)d(2)e(1)"

```java
import java.util.TreeMap;
import java.util.StringJoiner;

public class Test {
    public static void main(String[] args) {
        String str = "aababcabcdabcde";
        TreeMap<Character, Integer> map = new TreeMap<>();
        for (int i = 0; i < str.length(); i++) {
            char c = str.charAt(i);
            // 查看字符是否已经添加到集合当中
            if (map.containsKey(c)) {
                // 如果已添加,统计其字符已经出现的次数
                Integer count = map.get(c);
                // 字符又出现了一次
                count++;
                // 将当前字符的出现次数再次存入map集合中
                map.put(c, count);
            } else {
                // 如果未添加,将其添加到map集合中
                map.put(c, 1);
            }
        }

//        // 遍历map集合并打印
//        // 利用StringJoiner进行拼接
//        StringJoiner sb1 = new StringJoiner("", "", "");
//        map.forEach((character, integer) ->
//                // 按照指定格式进行拼接
//                sb1.add(character + "").add("(").add(integer + "").add(")")
//        );
//        // 打印结果:"a(5)b(4)c(3)d(2)e(1)"
//        System.out.println(sb1);

        // 遍历map集合并打印
        // 利用StringBuilder进行拼接
        StringBuilder sb2 = new StringBuilder();
        map.forEach((character, integer) ->
                // 按照指定格式进行拼接
                sb2.append(character).append("(").append(integer).append(")")
        );
        // 打印结果:"a(5)b(4)c(3)d(2)e(1)"
        System.out.println(sb2);
    }
}
```

# 底层原理

## TreeMap中每一个节点的内部属性

`K key;` -- 键              
`V value;` -- 值              
`Entry<K,V> left;` -- 左子节点               
`Entry<K,V> right;` -- 右子节点            
`Entry<K,V> parent;` -- 父节点              
`boolean color;` -- 节点的颜色             

## TreeMap类中中要知道的一些成员变量

```java
public class TreeMap<K,V>{
    // 比较器对象
    private final Comparator<? super K> comparator;
	// 根节点
    private transient Entry<K,V> root;
	// 集合的长度(节点的个数)
    private transient int size = 0;
}
```

## 空参构造

```java
// 空参构造就是没有传递比较器对象
public TreeMap() {
    comparator = null;
}
```
	
## 带参构造

```java
// 带参构造就是传递了比较器对象
public TreeMap(Comparator<? super K> comparator) {
    this.comparator = comparator;
}
```	
	
## 添加元素

```java
public V put(K key, V value) {
    return put(key, value, true);
}
```

参数一:键                 
参数二:值                         
参数三:当键重复的时候,是否需要覆盖值                  
true:覆盖                 
false:不覆盖                   
 
```java
private V put(K key, V value, boolean replaceOld) {
    // 获取根节点的地址值,赋值给局部变量t
    Entry<K,V> t = root;
    // 判断根节点是否为null
    // 如果为null,表示当前是第一次添加,会把当前要添加的元素,当做根节点
    // 如果不为null,表示当前不是第一次添加,跳过这个判断继续执行下面的代码
    if (t == null) {
        // 方法的底层,会创建一个Entry对象,把他当做根节点
        addEntryToEmptyMap(key, value);
        // 表示此时没有覆盖任何的元素
        return null;
    }
    // 表示两个元素的键比较之后的结果
    //负数:认为要添加的元素是小的,存左边
    //正数:认为要添加的元素是大的,存右边
    //0:认为要添加的元素已经存在,舍弃
    int cmp;
    // 表示当前要添加节点的父节点
    Entry<K,V> parent;
    
    // 表示当前的比较规则
    // 如果我们是采取默认的自然排序,那么此时comparator记录的是null,cpr记录的也是null
    // 如果我们是采取比较去排序方式,那么此时comparator记录的是就是比较器
    Comparator<? super K> cpr = comparator;
    // 表示判断当前是否有比较器对象
    // 如果传递了比较器对象,就执行if里面的代码,此时以比较器的规则为准
    // 如果没有传递比较器对象,就执行else里面的代码,此时以自然排序的规则为准
    if (cpr != null) {
        do {
            parent = t;
            cmp = cpr.compare(key, t.key);
            if (cmp < 0)
                t = t.left;
            else if (cmp > 0)
                t = t.right;
            else {
                V oldValue = t.value;
                if (replaceOld || oldValue == null) {
                    t.value = value;
                }
                return oldValue;
            }
        } while (t != null);
    } else {
        // 把键进行强转,强转成Comparable类型的
        // 要求:键必须要实现Comparable接口,如果没有实现这个接口
        // 此时在强转的时候,就会报错
        Comparable<? super K> k = (Comparable<? super K>) key;
        do {
            // 把根节点当做当前节点的父节点
            parent = t;
            // 调用compareTo方法,比较根节点和当前要添加节点的大小关系
            cmp = k.compareTo(t.key);
            if (cmp < 0)
                // 如果比较的结果为负数
                // 那么继续到根节点的左边去找
                t = t.left;
            else if (cmp > 0)
                // 如果比较的结果为正数
                // 那么继续到根节点的右边去找
                t = t.right;
            else {
                // 如果比较的结果为0,会覆盖
                V oldValue = t.value;
                if (replaceOld || oldValue == null) {
                    t.value = value;
                }
                return oldValue;
            }
        } while (t != null);
    }
    // 就会把当前节点按照指定的规则进行添加
    addEntry(key, value, parent, cmp < 0);
    return null;
}	

private void addEntry(K key, V value, Entry<K, V> parent, boolean addToLeft) {
    Entry<K,V> e = new Entry<>(key, value, parent);
    if (addToLeft)
        parent.left = e;
    else
        parent.right = e;
    // 添加完毕之后,需要按照红黑树的规则进行调整
    fixAfterInsertion(e);
    size++;
    modCount++;
}

private void fixAfterInsertion(Entry<K,V> x) {
    // 因为红黑树的节点默认就是红色的
    x.color = RED;
    // 按照红黑规则进行调整
    // parentOf:获取x的父节点
    // parentOf(parentOf(x)):获取x的祖父节点
    // leftOf:获取左子节点
    while (x != null && x != root && x.parent.color == RED) {
        // 判断当前节点的父节点是祖父节点的左子节点还是右子节点
        // 目的:为了获取当前节点的叔叔节点
        if (parentOf(x) == leftOf(parentOf(parentOf(x)))) {
            // 表示当前节点的父节点是祖父节点的左子节点
            // 那么下面就可以用rightOf获取到当前节点的叔叔节点
            Entry<K,V> y = rightOf(parentOf(parentOf(x)));
            if (colorOf(y) == RED) {
                // 叔叔节点为红色的处理方案
                // 把父节点设置为黑色
                setColor(parentOf(x), BLACK);
                // 把叔叔节点设置为黑色
                setColor(y, BLACK);
                // 把祖父节点设置为红色
                setColor(parentOf(parentOf(x)), RED);
                // 把祖父节点设置为当前节点
                x = parentOf(parentOf(x));
            } else {    
                // 叔叔节点为黑色的处理方案
                // 表示判断当前节点是否为父节点的右子节点
                if (x == rightOf(parentOf(x))) {
                    // 把父节点设置为当前节点
                    x = parentOf(x);
                    // 左旋
                    rotateLeft(x);
                }
                // 将父节点设置为黑色
                setColor(parentOf(x), BLACK);
                // 把祖父节点设置为红色
                setColor(parentOf(parentOf(x)), RED);
                // 把祖父节点作为支点右旋
                rotateRight(parentOf(parentOf(x)));
            }
        } else {
            // 表示当前节点的父节点是祖父节点的右子节点
            // 那么下面就可以用leftOf获取到当前节点的叔叔节点
            Entry<K,V> y = leftOf(parentOf(parentOf(x)));
            // 叔叔节点为红色的处理方案
            if (colorOf(y) == RED) {
                // 把父节点设置为黑色
                setColor(parentOf(x), BLACK);
                // 把叔叔节点设置为黑色
                setColor(y, BLACK);
                // 把祖父节点设置为红色
                setColor(parentOf(parentOf(x)), RED);
                // 把祖父节点设置为当前节点
                x = parentOf(parentOf(x));
            } else {
                // 叔叔节点为黑色的处理方案
                // 表示判断当前节点是否为父节点的左子节点
                if (x == leftOf(parentOf(x))) {
                    // 把父节点设置为当前节点
                    x = parentOf(x);
                    // 右旋
                    rotateRight(x);
                }
                // 将父节点设置为黑色
                setColor(parentOf(x), BLACK);
                // 把祖父节点设置为红色
                setColor(parentOf(parentOf(x)), RED);
                // 把祖父节点作为支点左旋
                rotateLeft(parentOf(parentOf(x)));
            }
        }
    }
    // 把根节点设置为黑色
    root.color = BLACK;
}
```

# 常见面试问题

1. **TreeMap添加元素的时候,键是否需要重写hashCode和equals方法?**              

此时是不需要重写的                   

2. **TreeMap和HashMap谁的效率更高?**                               

如果是最坏情况,添加了8个元素,这8个元素形成了链表,此时TreeMap的效率要更高;              
但是这种情况出现的几率非常的少,一般而言,还是HashMap的效率要更高                     