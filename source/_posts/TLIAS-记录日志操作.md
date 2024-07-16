---
title: "TLIAS-记录日志操作"
date: 2024-06-17
description: ""
cover: https://github.com/Gjt-9520/Resource/blob/main/Bimage-135/Bimage48.jpg?raw=true
tags: ["SpringBoot"]
category: "项目"
updated: 2024-06-18
  
top_group_index: 
---

- [TLIAS项目-Gitee仓库](https://gitee.com/gjt_1538048299/tlias)

# 记录日志操作

将TLIAS部门管理、员工管理中增、删、改相关接口的操作日志记录到数据库表中

日志信息包含:操作人、操作时间、执行方法的全类名、执行方法名、方法运行时参数、返回值、方法执行时长

创建操作日志表:

```sql
-- 创建操作日志表
create table operate_log
(
    id            int unsigned primary key auto_increment comment 'ID',
    operate_user  int unsigned comment '操作人ID',
    operate_time  datetime comment '操作时间',
    class_name    varchar(100) comment '操作的类名',
    method_name   varchar(100) comment '操作的方法名',
    method_params varchar(1000) comment '方法参数',
    return_value  varchar(2000) comment '返回值',
    cost_time     bigint comment '方法执行耗时, 单位:ms'
) comment '操作日志表';
```

Log接口:自定义注解

```java
package com.jinzhao.annotation;

import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

// 自定义注解

// 生效时间
@Retention(RetentionPolicy.RUNTIME)
// 生效范围
@Target(ElementType.METHOD)
public @interface Log {
}
```

OperateLogMapper类:

```java
package com.jinzhao.mapper;

import com.jinzhao.pojo.OperateLog;
import org.apache.ibatis.annotations.Mapper;

@Mapper
public interface OperateLogMapper {
    // 插入日志数据
    void insert(OperateLog log);
}
```

xml配置文件:

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.jinzhao.mapper.OperateLogMapper">
    <!--记录操作日志-->
    <insert id="insert">insert into operate_log (operate_user, operate_time, class_name, method_name, method_params,
                                                 return_value, cost_time)
                        values (#{operateUser}, #{operateTime}, #{className}, #{methodName}, #{methodParams},
                                #{returnValue}, #{costTime})</insert>
</mapper>
```

LogAspect类:切面类

```java
package com.jinzhao.aop;

import com.alibaba.fastjson2.JSONObject;
import com.jinzhao.mapper.OperateLogMapper;
import com.jinzhao.pojo.OperateLog;
import com.jinzhao.utils.JwtUtils;
import io.jsonwebtoken.Claims;
import jakarta.servlet.http.HttpServletRequest;
import lombok.extern.slf4j.Slf4j;
import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.Around;
import org.aspectj.lang.annotation.Aspect;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

import java.time.LocalDateTime;
import java.util.Arrays;

// 切面类
// 部门管理、员工管理增、删、改操作日志记录
@Slf4j
@Component
@Aspect
public class LogAspect {
    @Autowired
    private OperateLogMapper operateLogMapper;

    @Autowired
    private HttpServletRequest httpServletRequest;

    @Around("@annotation(com.jinzhao.annotation.Log))")
    public Object recordLog(ProceedingJoinPoint joinPoint) throws Throwable {
        // 获取操作人ID,即获取当前登录员工的ID
        // 通过获取请求头中的JWT令牌,解析令牌
        String jwt = httpServletRequest.getHeader("token");
        Claims claims = JwtUtils.parseJWT(jwt);
        Integer operateUser = (Integer) claims.get("id");

        // 获取操作时间
        LocalDateTime operateTime = LocalDateTime.now();

        // 获取操作类名
        String className = joinPoint.getTarget().getClass().getName();

        // 获取操作方法名
        String methodName = joinPoint.getSignature().getName();

        // 获取操作方法参数
        String methodParams = Arrays.toString(joinPoint.getArgs());

        // 方法开始时间
        long begin = System.currentTimeMillis();

        // 调用原始目标方法运行
        Object result = joinPoint.proceed();

        // 方法结束时间
        long end = System.currentTimeMillis();

        // 获取操作方法返回值
        String returnValue = JSONObject.toJSONString(result);

        // 获取操作耗时
        Long costTime = end - begin;

        // 记录操作日志
        OperateLog operateLog = new OperateLog(null, operateUser, operateTime, className, methodName,
                methodParams, returnValue, costTime);
        operateLogMapper.insert(operateLog);

        log.info("AOP记录操作日志:{}", operateLog);

        return result;
    }
}
```

DeptController类:

```java
package com.jinzhao.controller;

import com.jinzhao.annotation.Log;
import com.jinzhao.service.DeptService;
import com.jinzhao.pojo.Dept;
import com.jinzhao.pojo.Result;
import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@Slf4j
@RestController
@RequestMapping("/depts")
public class DeptController {
    @Autowired
    private DeptService deptService;

    @GetMapping
    public Result listDept() {
        // 日志记录
        log.info("查询全部部门数据");
        // 部门列表查询
        List<Dept> deptList = deptService.list();
        return Result.success(deptList);
    }

    @Log
    @DeleteMapping("/{id}")
    public Result deleteDept(@PathVariable Integer id) {
        // 日志记录
        log.info("根据Id删除部门,{}", id);
        // 根据Id删除部门
        deptService.delete(id);
        return Result.success();
    }

    @Log
    @PostMapping
    public Result addDept(@RequestBody Dept dept) {
        // 日志记录
        log.info("添加部门,{}", dept.getName());
        // 添加部门
        deptService.add(dept);
        return Result.success();
    }

    @GetMapping("/{id}")
    public Result getDeptById(@PathVariable Integer id) {
        // 日志记录
        log.info("根据Id查询部门,{}", id);
        // 根据Id查询部门
        Dept dept = deptService.getById(id);
        return Result.success(dept);
    }

    @Log
    @PutMapping
    public Result updateDept(@RequestBody Dept dept) {
        // 日志记录
        log.info("修改部门名称,{}", dept.getName());
        // 修改部门
        deptService.update(dept);
        return Result.success();
    }
}
```

EmpController类:

```java
package com.jinzhao.controller;

import com.jinzhao.annotation.Log;
import com.jinzhao.pojo.Emp;
import com.jinzhao.pojo.PageBean;
import com.jinzhao.pojo.Result;
import com.jinzhao.service.EmpService;
import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.format.annotation.DateTimeFormat;
import org.springframework.web.bind.annotation.*;

import java.time.LocalDate;
import java.util.List;

@Slf4j
@RestController
@RequestMapping("/emps")
public class EmpController {
    @Autowired
    private EmpService empService;

    @GetMapping
    public Result listEmp(String name,
                          Short gender,
                          @DateTimeFormat(pattern = "yyyy-MM-dd") LocalDate begin,
                          @DateTimeFormat(pattern = "yyyy-MM-dd") LocalDate end,
                          @RequestParam(defaultValue = "1") Integer page,
                          @RequestParam(defaultValue = "10") Integer pageSize) {
        // 日志记录
        log.info("分页查询,参数:{},{},{},{},{},{}", name, gender, begin, end, page, pageSize);
        // 根据当前页码、每页展示记录数进行分页查询,返回数据列表、总记录数
        // 根据条件进行分页查询
        PageBean pageBean = empService.list(name, gender, begin, end, page, pageSize);
        return Result.success(pageBean);
    }

    @Log
    @DeleteMapping("/{ids}")
    public Result deleteEmp(@PathVariable List<Integer> ids) {
        // 日志记录
        log.info("删除员工,{}", ids);
        // 批量删除员工
        empService.delete(ids);
        return Result.success();
    }

    @Log
    @PostMapping
    public Result addEmp(@RequestBody Emp emp) {
        // 日志记录
        log.info("添加员工,{}", emp.toString());
        // 添加员工
        empService.add(emp);
        return Result.success();
    }

    @GetMapping("/{id}")
    public Result getById(@PathVariable Integer id) {
        // 日志记录
        log.info("根据Id查询员工:{}", id);
        // 根据Id查询员工
        Emp emp = empService.getById(id);
        return Result.success(emp);
    }

    @Log
    @PutMapping
    public Result updateEmp(@RequestBody Emp emp) {
        // 日志记录
        log.info("修改员工,{}", emp);
        // 修改员工
        empService.update(emp);
        return Result.success();
    }
}
```