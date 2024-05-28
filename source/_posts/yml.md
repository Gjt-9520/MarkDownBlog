---
title: "yml"
date: 2024-06-13
description: ""
cover: https://github.com/Gjt-9520/Resource/blob/main/Bimage-135/Bimage45.jpg?raw=true
tags: ["yml","properties"]
category: "学习笔记"
updated: 2024-06-14
  
top_group_index: 
---

# 配置文件

![常见配置文件对比](../images/常见配置文件对比.png)

**SpringBoot支持`properties`和`yml`,通常推荐使用`yml`**

# 参数配置化

`@Value("${配置文件中的key}")`注解常用于外部配置的属性注入

范例:

配置文件中:

```properties
# 自定义阿里云OSS配置信息
aliyun.oss.endpoint=
aliyun.oss.accessKeyId=
aliyun.oss.accessKeySecret=
aliyun.oss.bucketName=
```

参数调用时:

```java
package com.jinzhao.utils;

import com.aliyun.oss.OSS;
import com.aliyun.oss.OSSClientBuilder;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;
import org.springframework.web.multipart.MultipartFile;

import java.io.*;
import java.util.UUID;

// 阿里云OSS 工具类
@Component
public class AliOSSUtils {
    @Value("${aliyun.oss.endpoint}")
    private String endpoint;
    @Value("${aliyun.oss.accessKeyId}")
    private String accessKeyId;
    @Value("${aliyun.oss.accessKeySecret}")
    private String accessKeySecret;
    @Value("${aliyun.oss.bucketName}")
    private String bucketName;

    // 实现上传图片到OSS
    public String upload(MultipartFile file) throws IOException {
        // 获取上传的文件的输入流
        InputStream inputStream = file.getInputStream();

        // 避免文件覆盖
        String originalFilename = file.getOriginalFilename();
        String fileName = UUID.randomUUID() + originalFilename.substring(originalFilename.lastIndexOf("."));

        // 上传文件到 OSS
        OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
        ossClient.putObject(bucketName, fileName, inputStream);

        // 文件访问路径
        String url = endpoint.split("//")[0] + "//" + bucketName + "." + endpoint.split("//")[1] + "/" + fileName;
        // 关闭OSSClient
        ossClient.shutdown();
        // 把上传到OSS的路径返回
        return url;
    }
}
```

## yml配置文件

![yml基本语法](../images/yml基本语法.png)

## yml数据格式

