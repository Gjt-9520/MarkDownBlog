---
title: "Properties"
date: 2024-04-19
description: ""
cover: https://github.com/Gjt-9520/Resource/blob/main/Aimage-135/Aimage128.jpg?raw=true
tags: ["Java SE","集合"]
category: "学习笔记"
updated: 2024-04-20
swiper_index: 
top_group_index: 
---

# Properties

Properties是一个双列集合,拥有Map集合的所有特点

# 特点

**有一些特有的方法,可以把集合中的数据,按照键值对的形式写到配置文件中;也可以把配置文件中的数据,读取到集合中来**

# 常用方法

细节:**尽管Properties中可以添加任意类型的数据,但是一般只会往里面添加字符串类型的数据**

范例:

```java
import java.util.Map;
import java.util.Properties;
import java.util.Set;

public class Test {
    public static void main(String[] args) {
        Properties prop = new Properties();
        prop.put("AAA", "1");
        prop.put("BBB", "2");
        prop.put("CCC", "3");

        Set<Object> keys = prop.keySet();
        for (Object key : keys) {
            Object value = prop.get(key);
            // 打印结果:"AAA:1"
            // 打印结果:"CCC:3"
            // 打印结果:"BBB:2"
            System.out.println(key + ":" + value);
        }

        prop.forEach((key, value) -> System.out.println(key + ":" + value));

        Set<Map.Entry<Object, Object>> entries = prop.entrySet();
        for (Map.Entry<Object, Object> entry : entries) {
            System.out.println(entry.getKey() + ":" + entry.getValue());
        }
    }
}
```

# 特有方法

## store保存

范例:

```java
import java.io.*;
import java.util.Properties;

public class Test {
    public static void main(String[] args) {
        Properties prop = new Properties();
        prop.put("AAA", "1");
        prop.put("BBB", "2");
        prop.put("CCC", "3");

        try {
            FileOutputStream fos = new FileOutputStream("C:\\Users\\gujintao\\Desktop\\untitled\\File\\AAA.properties");
            prop.store(fos, "Test");
            fos.close();
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }
}
```

效果:

```markdown
#Test
#Tue Apr 30 17:58:57 CST 2023
AAA=1
CCC=3
BBB=2
```

## load加载

范例:

```java
import java.io.*;
import java.util.Properties;

public class Test {
    public static void main(String[] args) {
        try {
            FileInputStream fis = new FileInputStream("C:\\Users\\gujintao\\Desktop\\untitled\\File\\AAA.properties");
            prop.load(fis);
            fis.close();
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
        // 打印结果:"{AAA=1, CCC=3, BBB=2}"
        System.out.println(prop);
    }
}
```