---
title: "IO流综合练习"
date: 2024-04-18
description: ""
cover: https://github.com/Gjt-9520/Resource/blob/main/Aimage-135/Aimage127.jpg?raw=true
tags: ["JavaSE","IO","练习"]
category: "笔记"
updated: 2024-04-19
 
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
            String str2 = str1.replace(",", "").replace(",</br>", "");
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
            String str2 = str1.replace(",", "").replace(",</br>", "");
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

# IO流版随机点名

## 随机点名

TXT文件中事先准备好100个学生姓名,每个学生的名字独占一行,格式为"姓名-性别-年龄",要求:实现随机点名

```java
import java.io.*;
import java.util.ArrayList;
import java.util.Collections;
import java.util.Scanner;

public class Test {
    public static void main(String[] args) throws IOException {
        Scanner sc = new Scanner(System.in);
        File file = new File("C:\\Users\\gujintao\\Desktop\\RandomName.txt");
        BufferedReader br = new BufferedReader(new FileReader(file));
        ArrayList<String> nameList = new ArrayList<>();
        String read;
        while ((read = br.readLine()) != null) {
            nameList.add(read);
        }
        br.close();
        while (true) {
            System.out.print("是否点名(输入1点名,输入其他退出):");
            String putIn = sc.nextLine();
            if (!putIn.equals("1")) {
                System.out.println("退出点名!");
                break;
            } else {
                Collections.shuffle(nameList);
                System.out.println(nameList.get(0));
                nameList.remove(0);
                if (nameList.size() == 0) {
                    System.out.println("所有人已点名!");
                    break;
                }
            }
        }
    }
}
```

## 带作弊功能的随机点名

TXT文件中事先准备好100个学生姓名,每个学生的名字独占一行,格式为"姓名-性别-年龄"      

要求:
1. 实现随机点名
2. 第3次点名必定是张三同学

```java
import java.io.*;
import java.util.ArrayList;
import java.util.Collections;
import java.util.Scanner;

public class Test {
    public static void main(String[] args) throws IOException {
        Scanner sc = new Scanner(System.in);
        File file = new File("C:\\Users\\gujintao\\Desktop\\RandomName.txt");
        // 定义File对象存储点名次数
        File countFile = new File("C:\\Users\\gujintao\\Desktop\\Count.txt");
        BufferedReader br = new BufferedReader(new FileReader(file));
        ArrayList<String> nameList = new ArrayList<>();
        String read;
        while ((read = br.readLine()) != null) {
            nameList.add(read);
        }
        while (true) {
            System.out.print("是否点名(输入1点名,输入其他退出):");
            String putIn = sc.nextLine();
            if (!putIn.equals("1")) {
                System.out.println("退出点名!");
                break;
            } else {
                BufferedReader countBr = new BufferedReader(new FileReader(countFile));
                String countStr = countBr.readLine();
                countBr.close();
                int count = Integer.parseInt(countStr);
                if (count != 3) {
                    Collections.shuffle(nameList);
                    System.out.println("第" + count + "次点名:" + nameList.get(0));
                    nameList.remove(0);
                } else {
                    System.out.println("第3次点名:张三-男-23");
                }
                count++;
                BufferedWriter bw = new BufferedWriter(new FileWriter(countFile));
                bw.write(Integer.toString(count));
                bw.close();
                if (nameList.size() == 0) {
                    System.out.println("所有人已点名!");
                    break;
                }
            }
        }
        br.close();
    }
}
```

## 有概率且不重复的随机点名

TXT文件中事先准备好200个学生姓名,格式为"姓名-性别-年龄",其中100个男生,100个女生           

要求:
1. 70%的概率随机到男生,30%的概率随机到女生                   
2. 总共随机100万次,统计结果

```java
package src.test4;

import java.io.BufferedReader;
import java.io.File;
import java.io.FileReader;
import java.io.IOException;
import java.util.ArrayList;
import java.util.Collections;

public class Test {
    public static void main(String[] args) throws IOException {
        File file = new File("C:\\Users\\gujintao\\Desktop\\RandomName.txt");
        int count = 1000000;
        BufferedReader br = new BufferedReader(new FileReader(file));
        ArrayList<String> nameList = new ArrayList<>();
        String read;
        while ((read = br.readLine()) != null) {
            nameList.add(read);
        }
        ArrayList<String> boyNameList = new ArrayList<>();
        ArrayList<String> girlNameList = new ArrayList<>();
        for (String s : nameList) {
            if (s.split("-")[1].equals("男")) {
                boyNameList.add(s);
            } else {
                girlNameList.add(s);
            }
        }
        ArrayList<Integer> numberList = new ArrayList<>();
        Collections.addAll(numberList, 1, 1, 1, 1, 1, 1, 1, 0, 0, 0);
        int boyCount = 1;
        int girlCount = 1;
        System.out.println("开始点名");
        System.out.println("---------------");
        for (int i = 0; i < count; i++) {
            Collections.shuffle(numberList);
            String name;
            if (numberList.get(0) == 1) {
                Collections.shuffle(boyNameList);
                name = boyNameList.get(0);
                System.out.println(name);
                boyCount++;
            } else {
                Collections.shuffle(girlNameList);
                name = girlNameList.get(0);
                System.out.println(name);
                girlCount++;
            }
        }
        System.out.println("---------------");
        System.out.println("男生点名次数:" + boyCount + " 概率:" + boyCount / (double) count);
        System.out.println("女生点名次数:" + girlCount + " 概率:" + girlCount / (double) count);
    }
}
```

## 不重复的随机点名

TXT文件中事先准备好100个学生姓名,每个学生的名字独占一行,格式为"姓名-性别-年龄"                 

要求:
1. 被点到的学生不会再被点到
2. 如果班级里所有的学生都点完了,需要开启新一轮点名    

```java
import java.io.*;
import java.util.ArrayList;
import java.util.Collections;
import java.util.Scanner;

public class Test {
    public static void main(String[] args) throws IOException {
        File file = new File("C:\\Users\\gujintao\\Desktop\\RandomName.txt");
        File backup = new File("C:\\Users\\gujintao\\Desktop\\Backup.txt");
        readName(file, backup);
    }

    // 点名
    public static void readName(File file, File backup) throws IOException {
        Scanner sc = new Scanner(System.in);
        BufferedReader br = new BufferedReader(new FileReader(file));
        ArrayList<String> nameList = new ArrayList<>();
        // 读出数据并添加到集合中
        String read1;
        while ((read1 = br.readLine()) != null) {
            nameList.add(read1);
        }
        // 优先保存备份
        writeName(backup, nameList);
        while (true) {
            System.out.print("是否点名(输入1点名,输入其他退出):");
            String putIn1 = sc.nextLine();
            if (!putIn1.equals("1")) {
                System.out.println("退出点名!");
                System.exit(0);
            } else {
                Collections.shuffle(nameList);
                if (nameList.size() != 0) {
                    System.out.println(nameList.get(0));
                    // 在集合中删除已点名的同学
                    nameList.remove(0);
                    // 在文件中删除已点名的同学
                    writeName(file, nameList);
                }
            }
            // 所有人已点名,此时文件和集合都为空
            if (nameList.size() == 0) {
                System.out.println("所有人已点名!");
                System.out.print("是否开始新一轮的点名(输入1重新开始,输入其他退出):");
                String putIn2 = sc.nextLine();
                if (!putIn2.equals("1")) {
                    System.out.println("退出点名!");
                    System.exit(0);
                } else {
                    // 将备份数据重新添加到文件中
                    BufferedReader newFileBr = new BufferedReader(new FileReader(backup));
                    String read2;
                    while ((read2 = newFileBr.readLine()) != null) {
                        nameList.add(read2);
                    }
                    writeName(file, nameList);
                    // 递归新一轮的点名
                    readName(file, backup);
                }
            }
        }
    }

    // 集合写入文件
    public static void writeName(File file, ArrayList<String> list) throws IOException {
        BufferedWriter bw = new BufferedWriter(new FileWriter(file, false));
        for (String s : list) {
            bw.write(s);
            bw.newLine();
        }
        bw.close();
    }
}
```

## 带权重的随机点名

TXT文件中事先准备好80个学生姓名,每个学生的名字独占一行,格式为"姓名-性别-年龄-权重"                            

要求:                    
每次被点到的学生,再次被点到的概率在原先的基础上降低一半             
举例:80个学生,点名5次,每次都点到小A,概率变化情况如下:                      
第1次每人概率:1.25%                   
第2次小A概率:0.625%,其他学生概率:1.2579%                         
第3次小A概率:0.3125%,其他学生概率:1.261867%                           
第4次小A概率:0.15625%,其他学生概率:1.2638449%                 
第5次小A概率:0.078125%,其他学生概率:1.26483386%             
    
```java
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

        // 获取随机的姓名集合,格式:"姓名(唯一)-性别-年龄-权重"
        // 权重=1/人数总和
        ArrayList<String> nameList = getNameInfos(40, 40, familyNameList, boyNameList, girlNameList);

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

    // 获取随机的姓名集合,格式:"姓名(唯一)-性别-年龄-权重"
    // 参数:男生姓名数量,女生姓名数量,姓的集合,男生名的集合,女生名的集合
    public static ArrayList<String> getNameInfos(int boyCount, int girlCount, ArrayList<String> familyNameList, ArrayList<String> boyNameList, ArrayList<String> girlNameList) {
        // 生成不重复的名字
        HashSet<String> boyHashSet = new HashSet<>();
        HashSet<String> girlHashSet = new HashSet<>();
        Random random = new Random();
        // 权重
        double weight = (double) 1 / (boyCount + girlCount);
        for (int i = 0; i < boyCount; i++) {
            String familyName = familyNameList.get(random.nextInt(familyNameList.size()));
            String boyName = boyNameList.get(random.nextInt(boyNameList.size()));
            String age = Integer.toString(random.nextInt(10) + 18);
            String boy = familyName + boyName + "-男-" + age + "-" + weight;
            boyHashSet.add(boy);
        }
        for (int i = 0; i < girlCount; i++) {
            String familyName = familyNameList.get(random.nextInt(familyNameList.size()));
            String girlName = girlNameList.get(random.nextInt(girlNameList.size()));
            String age = Integer.toString(random.nextInt(8) + 18);
            String girl = familyName + girlName + "-女-" + age + "-" + weight;
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
        // System.out.println(list);
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
        // System.out.println(list);
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
            String str2 = str1.replace(",", "").replace(",</br>", "");
            char[] charArr = str2.toCharArray();
            // 将处理好的数据转换成单个姓的字符串存入集合中
            for (char c : charArr) {
                list.add(String.format("%s", c));
            }
        }
        // System.out.println(list);
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

```java
import java.io.*;
import java.util.ArrayList;
import java.util.Arrays;

public class Test {
    public static void main(String[] args) throws IOException {
        File file = new File("C:\\Users\\gujintao\\Desktop\\RandomName.txt");

        // 读取文件数据
        BufferedReader br = new BufferedReader(new FileReader(file));
        ArrayList<Student> list = new ArrayList<>();
        String read;
        while ((read = br.readLine()) != null) {
            String[] splitArr = read.split("-");
            Student student = new Student(splitArr[0], splitArr[1], Integer.parseInt(splitArr[2]), Double.parseDouble(splitArr[3]));
            list.add(student);
        }
        br.close();

        // 计算权重的总和
        double weightSum = 0;
        for (Student student : list) {
            weightSum += student.getWeight();
        }

        // 计算每一个人的实际占比
        double[] weightArr = new double[list.size()];
        int arrIndex = 0;
        for (Student student : list) {
            weightArr[arrIndex] = student.getWeight() / weightSum;
            arrIndex++;
        }

        // 计算每一个人的权重占比
        for (int i = 1; i < weightArr.length; i++) {
            weightArr[i] += weightArr[i - 1];
        }

        // 随机抽取一个学生
        double number = Math.random();
        // 判断number在weightArr中的位置
        // 二分查找返回值:-插入点-1
        // 则numberIndex = -(二分查找返回值)-1
        int numberIndex = -Arrays.binarySearch(weightArr, number) - 1;

        // 修改当前学生的权重
        Student student = list.get(numberIndex);
        student.setWeight(student.getWeight() / 2);
        
        // 数据写回文件
        BufferedWriter bw = new BufferedWriter(new FileWriter(file));
        for (Student stu : list) {
            bw.write(stu.toString());
            bw.newLine();
        }
        bw.close();
    }
}
```