---
title: "定时任务Scheduled注解的使用"
date: 2024-10-04
description: ""
cover: https://github.com/Gjt-9520/MarkDownBlog/blob/main/source/coverImages/Aimage-135/Aimage15.jpg?raw=true
tags: ["JavaSE"]
category: "实用"
updated: 2024-10-05

top_group_index:
---

​        1、在启动类上加上@EnableScheduling注解,用于开启对定时任务的支持

```java
@SpringBootApplication
@EnableScheduling
public class TimerApp {
    public static void main( String[] args ) {
        SpringApplication.run(TimerApp.class, args);
    }
}
```

​        2、在需要定时执行的方法上加上@Scheduled注解

```java
@Component
public class ScheduleTimer {
    @Autowired
    private ITestService testService;
    
    @Scheduled(cron = "0 * * * * ? ")
    public void executeTask() {
        testService.test();
    }
}
```

该注解常用的几个参数如下：

* cron         

  接收一个cron表达式，cron表达式是一个字符串   cron表达式在线生成地址 https://cron.ciding.cc/

* #### fixedDelay  

   上一次执行完毕时间点之后多长时间再执行

* #### fixedRate

  上一次开始执行时间点之后多长时间再执行。