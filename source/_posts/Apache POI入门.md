---
title: "Apache POI入门"
date: 2024-07-12
description: ""
cover: https://github.com/Gjt-9520/MarkDownBlog/blob/main/source/coverImages/Bimage-135/Bimage62.jpg?raw=true
tags: ["Apache POI"]
category: "笔记"
updated: 2024-07-13
  
top_group_index: 
---

# Apache POI

Apache POI是一个处理Miscrosoft Office各种文件格式的开源项目

简单来说就是,可以使用POI在Java程序中对Miscrosoft Office各种文件进行读写操作,一般情况下,POI都是用于操作Excel文件

应用场景:
- 银行网银系统导出交易明细
- 各种业务系统导出Excel报表
- 批量导入业务数据

# 入门案例

```java
package com.sky.test;

import org.apache.poi.xssf.usermodel.XSSFCell;
import org.apache.poi.xssf.usermodel.XSSFRow;
import org.apache.poi.xssf.usermodel.XSSFSheet;
import org.apache.poi.xssf.usermodel.XSSFWorkbook;

import java.io.File;
import java.io.FileInputStream;
import java.io.FileOutputStream;

public class POITest {

    /**
     * 通过POI创建Excel文件并且写入文件内容
     */
    public static void write() throws Exception {
        // 在内存中创建一个Excel文件
        XSSFWorkbook excel = new XSSFWorkbook();
        // 在Excel文件中创建一个Sheet页
        XSSFSheet sheet = excel.createSheet("info");

        // 在Sheet中创建行对象,行号从0开始
        XSSFRow row0 = sheet.createRow(0);
        // 在row中创建单元格,并且写入文件内容
        row0.createCell(0).setCellValue("姓名");
        row0.createCell(1).setCellValue("年龄");

        XSSFRow row1 = sheet.createRow(1);
        row1.createCell(0).setCellValue("张三");
        row1.createCell(1).setCellValue("23");

        XSSFRow row2 = sheet.createRow(2);
        row2.createCell(0).setCellValue("李四");
        row2.createCell(1).setCellValue("24");

        // 通过输出流将内存中的Excel文件写入到磁盘
        FileOutputStream fileOutputStream = new FileOutputStream(new File("C:\\Users\\gujintao\\Desktop\\excel.xlsx"));
        excel.write(fileOutputStream);

        // 关闭资源
        excel.close();
        fileOutputStream.close();
    }

    /**
     * 通过POI读取磁盘上已经存在的Excel文件
     *
     * @throws Exception
     */
    public static void read() throws Exception {
        FileInputStream fileInputStream = new FileInputStream(new File("C:\\Users\\gujintao\\Desktop\\excel.xlsx"));
        // 在内存中创建一个Excel文件
        XSSFWorkbook excel = new XSSFWorkbook(fileInputStream);
        // 读取Excel文件中的第一个Sheet页
        XSSFSheet sheet = excel.getSheetAt(0);

        // 获取Sheet页中最后一行的行号
        int lastRowNum = sheet.getLastRowNum();

        for (int i = 0; i <= lastRowNum; i++) {
            // 获得行
            XSSFRow row = sheet.getRow(i);
            // 获得单元格
            XSSFCell cell0 = row.getCell(0);
            XSSFCell cell1 = row.getCell(1);
            // 获得单元格对象内容
            String stringCellValue0 = cell0.getStringCellValue();
            String stringCellValue1 = cell1.getStringCellValue();
            System.out.println(stringCellValue0 + " " + stringCellValue1);
        }

        // 关闭资源
        excel.close();
        fileInputStream.close();
    }

    public static void main(String[] args) throws Exception {
        write();
        read();
    }
}
```