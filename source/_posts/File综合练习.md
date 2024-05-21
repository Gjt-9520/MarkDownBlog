---
title: "File综合练习"
date: 2024-04-06
description: ""
cover: https://github.com/Gjt-9520/Resource/blob/main/Aimage-135/Aimage115.jpg?raw=true
tags: ["Java SE","IO","练习"]
category: "学习笔记"
updated: 2024-04-07
 
top_group_index: 
---

## 创建文件

在当前模块下的aaa文件夹中创建一个a.txt文件

```java
import java.io.File;
import java.io.IOException;

public class Test {
    public static void main(String[] args) throws IOException {
        File file = new File("src\\aaa");
        boolean mkdirs = file.mkdirs();
        if (mkdirs) {
            System.out.println("创建文件夹成功");
        } else {
            System.out.println("创建文件夹失败");
        }
        File src = new File(file, "a.txt");
        boolean newFile = src.createNewFile();
        if (newFile) {
            System.out.println("创建文件成功");
        } else {
            System.out.println("创建文件失败");
        }
    }
}
```

## 单个文件夹查找文件

1. 定义一个方法找某一个文件夹中,是否有以avi结尾的电影(不需要考虑子文件夹)

```java
import java.io.File;

public class Test {
    public static void main(String[] args) {
        File file = new File("src\\aaa");
        System.out.println(findAVI(file));
    }

    public static boolean findAVI(File file) {
        File[] files = file.listFiles();
        if (files != null) {
            for (File f : files) {
                if (f.isFile() && f.getName().endsWith(".avi")) {
                    return true;
                }
            }
        }
        return false;
    }
}
```

2. 定义一个方法找某一个文件夹中,是否有以avi结尾的电影(需要考虑子文件夹)

```java
import java.io.File;

public class Test {
    public static void main(String[] args) {
        File file = new File("src\\aaa");
        findAVI(file);
    }

    public static void findAVI(File file) {
        File[] files = file.listFiles();
        if (files != null) {
            for (File f : files) {
                if (f.isFile()) {
                    String name = f.getName();
                    if (name.endsWith(".avi")) {
                        System.out.println(f);
                    }
                } else {
                    findAVI(f);
                }
            }
        }
    }
}
```

## 删除多级文件夹

```java
import java.io.File;

public class Test {
    public static void main(String[] args) {
        File file = new File("src\\aaa");
        removeAll(file);
    }

    public static void removeAll(File file) {
        File[] files = file.listFiles();
        if (files != null) {
            for (File f : files) {
                if (f.isFile()) {
                    boolean delete = f.delete();
                } else {
                    removeAll(f);
                }
            }
        }
        boolean delete = file.delete();
    }
}
```

## 统计各种文件的总大小

统计一个文件夹中的每种文件的总大小并打印(考虑子文件夹)

```java
import java.io.File;
import java.util.LinkedHashMap;

public class Test {
    static LinkedHashMap<String, Long> sizeMap = new LinkedHashMap<>();

    public static void main(String[] args) {
        File file = new File("src\\aaa");
        sizeFiles(file);
        sizeMap.forEach((key, size) -> System.out.println(key + "文件的总大小:" + size + "字节"));
    }

    public static void sizeFiles(File file) {
        File[] files = file.listFiles();
        if (files != null) {
            for (File f : files) {
                if (f.isFile()) {
                    String key = f.getName().split("\\.")[1];
                    if (sizeMap.containsKey(key)) {
                        long size = sizeMap.get(key) + f.length();
                        sizeMap.put(key, size);
                    } else {
                        sizeMap.put(key, 0L);
                    }
                } else {
                    sizeFiles(f);
                }
            }
        }
    }
}
```

## 统计各种文件数量

统计一个文件夹中的每种文件的个数并打印(考虑子文件夹)

```java
import java.io.File;
import java.util.LinkedHashMap;

public class Test {
    static LinkedHashMap<String, Integer> countMap = new LinkedHashMap<>();

    public static void main(String[] args) {
        File file = new File("src\\aaa");
        countFiles(file);
        countMap.forEach((key, value) -> System.out.println(key + "文件数量:" + value + "个"));
    }

    public static void countFiles(File file) {
        File[] files = file.listFiles();
        if (files != null) {
            for (File f : files) {
                if (f.isFile()) {
                    String[] split = f.getName().split("\\.");
                    // 例如:"a.txt"考虑计数,"a.a.txt"考虑计数,"aaa"(无后缀)不考虑计数
                    if (split.length >= 2) {
                        String key = split[split.length - 1];
                        if (countMap.containsKey(key)) {
                            int value = countMap.get(key) + 1;
                            countMap.put(key, value);
                        } else {
                            countMap.put(key, 1);
                        }
                    }
                } else {
                    countFiles(f);
                }
            }
        }
    }
}
```