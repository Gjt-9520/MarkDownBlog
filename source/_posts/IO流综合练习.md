---
title: "IO流综合练习"
date: 2024-04-18
description: ""
cover: https://github.com/Gjt-9520/Resource/blob/main/Aimage-135/Aimage127.jpg?raw=true
tags: ["Java SE","IO","练习","Hutool"]
category: "学习笔记"
updated: 2024-04-19
swiper_index: 
top_group_index: 
---

# 随机姓名

要求:在以下的各个网站上爬取数据,生成随机名字并写出到本地

[获取姓氏](https://hanyu.baidu.com/shici/detail?pid=0b2f26d4c0ddb3ee693fdb1137ee1b0d&from=kg0)
[获取男生名字](http://www.haoming8.cn/baobao/10881.html)
[获取女生名字](http://www.haoming8.cn/baobao/7641.html)

## 自己造轮子

步骤:
1. 爬取网站数据,把网站上所有的数据拼接成一个字符串
2. 通过相关的数据处理,把其中符合要求的数据获取出来,并存入集合中
3. 获取随机的姓名集合,格式:"姓名(唯一)-性别-年龄"
4. 写出数据

```java
package src.test1;

import java.io.*;
import java.net.URL;
import java.net.URLConnection;
import java.util.ArrayList;
import java.util.Collections;
import java.util.HashSet;
import java.util.Random;
import java.util.regex.Matcher;
import java.util.regex.Pattern;


public class Test {
    public static void main(String[] args) throws IOException {
        // 定义变量记录网址
        String familyNameNet = "https://hanyu.baidu.com/shici/detail?pid=0b2f26d4c0ddb3ee693fdb1137ee1b0d&from=kg0";
        String boyNameNet = "http://www.haoming8.cn/baobao/10881.html";
        String girlNameNet = "http://www.haoming8.cn/baobao/7641.html";
        
        // 爬取网站数据,把网站上所有的数据拼接成一个字符串
        String familyNameStr = webCrawler(familyNameNet);
        String boyNameStr = webCrawler(boyNameNet);
        String girlNameStr = webCrawler(girlNameNet);
        
        // 通过相关的数据处理,把其中符合要求的数据获取出来,并存入集合中
        ArrayList<String> familyNameList = getFamilyNameList(familyNameStr);
        ArrayList<String> boyNameList = getBoyNameList(boyNameStr);
        ArrayList<String> girlNameList = getGirlNameList(girlNameStr);
        
        // 获取随机的姓名集合,格式:"姓名(唯一)-性别-年龄"
        ArrayList<String> nameList = getNameInfos(50, 50, familyNameList, boyNameList, girlNameList);
        
        // 打乱姓名顺序
        // Collections.shuffle(nameList);
        
        // 写出数据
        writeNameInfos("C:\\Users\\gujintao\\Desktop", nameList);
    }

    // 写出数据
    // 参数:写出目的地,数据集合
    public static void writeNameInfos(String src, ArrayList<String> list) throws IOException {
        File file = new File(src, "RandomName.txt");
        BufferedWriter bw = new BufferedWriter(new FileWriter(file));
        for (String s : list) {
            bw.write(s);
            bw.newLine();
        }
        bw.close();
    }

    // 获取随机的姓名集合,格式:"姓名(唯一)-性别-年龄"
    // 参数:男生姓名数量,女生姓名数量,姓的集合,男生名的集合,女生名的集合
    public static ArrayList<String> getNameInfos(int boyCount, int girlCount, ArrayList<String> familyNameList, ArrayList<String> boyNameList, ArrayList<String> girlNameList) {
        // 生成不重复的名字
        HashSet<String> boyHashSet = new HashSet<>();
        HashSet<String> girlHashSet = new HashSet<>();
        Random random = new Random();
        for (int i = 0; i < boyCount; i++) {
            String familyName = familyNameList.get(random.nextInt(familyNameList.size()));
            String boyName = boyNameList.get(random.nextInt(boyNameList.size()));
            String age = Integer.toString(random.nextInt(10) + 18);
            String boy = familyName + boyName + "-男-" + age;
            boyHashSet.add(boy);
        }
        for (int i = 0; i < girlCount; i++) {
            String familyName = familyNameList.get(random.nextInt(familyNameList.size()));
            String girlName = girlNameList.get(random.nextInt(girlNameList.size()));
            String age = Integer.toString(random.nextInt(8) + 18);
            String girl = familyName + girlName + "-女-" + age;
            girlHashSet.add(girl);
        }
        ArrayList<String> nameInfos = new ArrayList<>();
        nameInfos.addAll(boyHashSet);
        nameInfos.addAll(girlHashSet);
        return nameInfos;
    }

    // 获取女孩名的集合
    // 参数:女孩名的字符串
    private static ArrayList<String> getGirlNameList(String str) {
        ArrayList<String> list = new ArrayList<>();
        // 定义正则表达式获取初步数据
        String regex = "彤舞(.+)含卉";
        // 获取正则表达式的对象
        Pattern pattern = Pattern.compile(regex);
        // 获取文本匹配器的对象
        Matcher matcher = pattern.matcher(str);
        while (matcher.find()) {
            // 获取到每一个符合正则表达式的字符串
            String str1 = matcher.group();
            // 将初步数据进行处理
            String str2 = str1.replace("</p><p>", " ");
            String[] strArr = str2.split(" ");
            // 将处理好的数据转换成单个名字的字符串存入集合中
            Collections.addAll(list, strArr);
        }
        return list;
    }

    // 获取男孩名的集合
    // 参数:男孩名的字符串
    private static ArrayList<String> getBoyNameList(String str) {
        ArrayList<String> list = new ArrayList<>();
        StringBuilder sb = new StringBuilder();
        // 定义正则表达式获取初步数据
        String regex = "(月星(.+)文永)|(泽瀚(.+)博庭)";
        // 获取正则表达式的对象
        Pattern pattern = Pattern.compile(regex);
        // 获取文本匹配器的对象
        Matcher matcher = pattern.matcher(str);
        while (matcher.find()) {
            // 获取到每一个符合正则表达式的字符串
            String str1 = matcher.group();
            // 将初步数据进行处理
            sb.append(str1).append("、");
        }
        String[] strArr = sb.toString().split("、");
        // 将处理好的数据转换成单个名字的字符串存入集合中
        Collections.addAll(list, strArr);
        return list;
    }

    // 获取姓的集合
    // 参数:姓的字符串
    private static ArrayList<String> getFamilyNameList(String str) {
        ArrayList<String> list = new ArrayList<>();
        // 定义正则表达式获取初步数据
        String regex = "赵钱孙李(.+)第五言福";
        // 获取正则表达式的对象
        Pattern pattern = Pattern.compile(regex);
        // 获取文本匹配器的对象
        Matcher matcher = pattern.matcher(str);
        while (matcher.find()) {
            // 获取到每一个符合正则表达式的字符串
            String str1 = matcher.group();
            // 将初步数据进行处理
            String str2 = str1.replace("，", "").replace("。</br>", "");
            char[] charArr = str2.toCharArray();
            // 将处理好的数据转换成单个姓的字符串存入集合中
            for (char c : charArr) {
                list.add(String.format("%s", c));
            }
        }
        return list;
    }

    // 从网站中爬取数据,把数据拼接成字符串返回
    public static String webCrawler(String net) throws IOException {
        StringBuilder sb = new StringBuilder();
        // 创建URL对象
        URL url = new URL(net);
        // 链接网站
        URLConnection connection = url.openConnection();
        // 读取数据
        InputStreamReader isr = new InputStreamReader(connection.getInputStream());
        int read;
        while ((read = isr.read()) != -1) {
            sb.append((char) read);
        }
        isr.close();
        return sb.toString();
    }
}
```

## 利用Hutool包

细节:**Hutool的相对路径,不是相对于当前项目而言的,而是相对class文件而言的**

1. 通过`HttpUtil.get`方法,爬取网站数据,把网站上所有的数据拼接成一个字符串
2. 通过`ReUtil.findAll`方法,根据正则表达式,从字符串中获取初步数据
3. 通过`FileUtil.writeLines`方法,写出数据               

```java
package src.test2;

import cn.hutool.core.io.FileUtil;
import cn.hutool.core.util.ReUtil;
import cn.hutool.http.HttpUtil;

import java.util.*;

public class Test {
    public static void main(String[] args) {
        // 定义变量记录网址
        String familyNameNet = "https://hanyu.baidu.com/shici/detail?pid=0b2f26d4c0ddb3ee693fdb1137ee1b0d&from=kg0";
        String boyNameNet = "http://www.haoming8.cn/baobao/10881.html";
        String girlNameNet = "http://www.haoming8.cn/baobao/7641.html";
        
        // 爬取网站数据,把网站上所有的数据拼接成一个字符串
        String familyNameStr = HttpUtil.get(familyNameNet);
        String boyNameStr = HttpUtil.get(boyNameNet);
        String girlNameStr = HttpUtil.get(girlNameNet);
        
        // 通过相关的数据处理,把其中符合要求的数据获取出来,并存入集合中
        ArrayList<String> familyNameList = getFamilyNameList(familyNameStr);
        ArrayList<String> boyNameList = getBoyNameList(boyNameStr);
        ArrayList<String> girlNameList = getGirlNameList(girlNameStr);
        
        // 获取随机的姓名集合,格式:"姓名(唯一)-性别-年龄"
        ArrayList<String> nameList = getNameInfos(50, 50, familyNameList, boyNameList, girlNameList);
        
        // 打乱姓名顺序
        // Collections.shuffle(nameList);
        
        // 写出数据
        FileUtil.writeLines(nameList, "C:\\Users\\gujintao\\Desktop\\RandomName.txt", "UTF-8");
    }

    // 获取随机的姓名集合,格式:"姓名(唯一)-性别-年龄"
    // 参数:男生姓名数量,女生姓名数量,姓的集合,男生名的集合,女生名的集合
    public static ArrayList<String> getNameInfos(int boyCount, int girlCount, ArrayList<String> familyNameList, ArrayList<String> boyNameList, ArrayList<String> girlNameList) {
        // 生成不重复的名字
        HashSet<String> boyHashSet = new HashSet<>();
        HashSet<String> girlHashSet = new HashSet<>();
        Random random = new Random();
        for (int i = 0; i < boyCount; i++) {
            String familyName = familyNameList.get(random.nextInt(familyNameList.size()));
            String boyName = boyNameList.get(random.nextInt(boyNameList.size()));
            String age = Integer.toString(random.nextInt(10) + 18);
            String boy = familyName + boyName + "-男-" + age;
            boyHashSet.add(boy);
        }
        for (int i = 0; i < girlCount; i++) {
            String familyName = familyNameList.get(random.nextInt(familyNameList.size()));
            String girlName = girlNameList.get(random.nextInt(girlNameList.size()));
            String age = Integer.toString(random.nextInt(8) + 18);
            String girl = familyName + girlName + "-女-" + age;
            girlHashSet.add(girl);
        }
        ArrayList<String> nameInfos = new ArrayList<>();
        nameInfos.addAll(boyHashSet);
        nameInfos.addAll(girlHashSet);
        return nameInfos;
    }

    // 获取女孩名的集合
    // 参数:女孩名的字符串
    private static ArrayList<String> getGirlNameList(String str) {
        ArrayList<String> list = new ArrayList<>();
        // 定义正则表达式获取初步数据
        List<String> girlNameTempList = ReUtil.findAll("彤舞(.+)含卉", str, 0);
        for (String str1 : girlNameTempList) {
            String str2 = str1.replace("</p><p>", " ");
            String[] strArr = str2.split(" ");
            // 将处理好的数据转换成单个名字的字符串存入集合中
            Collections.addAll(list, strArr);
        }
        return list;
    }

    // 获取男孩名的集合
    // 参数:男孩名的字符串
    private static ArrayList<String> getBoyNameList(String str) {
        ArrayList<String> list = new ArrayList<>();
        StringBuilder sb = new StringBuilder();
        // 定义正则表达式获取初步数据
        List<String> boyNameTempList = ReUtil.findAll("(月星(.+)文永)|(泽瀚(.+)博庭)", str, 0);

        for (String str1 : boyNameTempList) {
            // 将初步数据进行处理
            sb.append(str1).append("、");
        }
        String[] strArr = sb.toString().split("、");
        // 将处理好的数据转换成单个名字的字符串存入集合中
        Collections.addAll(list, strArr);
        return list;
    }

    // 获取姓的集合
    // 参数:姓的字符串
    private static ArrayList<String> getFamilyNameList(String str) {
        ArrayList<String> list = new ArrayList<>();
        // 定义正则表达式获取初步数据
        List<String> familyNameTempList = ReUtil.findAll("赵钱孙李(.+)第五言福", str, 0);
        for (String str1 : familyNameTempList) {
            // 将初步数据进行处理
            String str2 = str1.replace("，", "").replace("。</br>", "");
            char[] charArr = str2.toCharArray();
            // 将处理好的数据转换成单个姓的字符串存入集合中
            for (char c : charArr) {
                list.add(String.format("%s", c));
            }
        }
        return list;
    }
}
```