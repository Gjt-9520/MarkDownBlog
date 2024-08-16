---
title: "MyBatisPlus代码生成器"
date: 2024-08-11
description: ""
cover: https://github.com/Gjt-9520/MarkDownBlog/blob/main/source/coverImages/Bimage-135/Bimage100.jpg?raw=true
tags: ["MyBatisPlus"]
category: "数据库"
updated: 2024-08-12
  
top_group_index: 
---

[MyBatisPlus代码生成器](https://baomidou.com/guides/code-generator/#%E6%B7%BB%E5%8A%A0%E4%BE%9D%E8%B5%96)

# 依赖引入

```xml
<dependencies>

    <!-- 日志相关配置 -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter</artifactId>
        <exclusions>
            <exclusion>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-logging</artifactId>
            </exclusion>
        </exclusions>
    </dependency>

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-log4j2</artifactId>
    </dependency>

    <!-- MyBatisPlus的SpringBoot启动器 -->
    <dependency>
        <groupId>com.baomidou</groupId>
        <artifactId>mybatis-plus-boot-starter</artifactId>
    </dependency>

    <!-- MyBatisPlus代码生成器 -->
    <dependency>
        <groupId>com.baomidou</groupId>
        <artifactId>mybatis-plus-generator</artifactId>
    </dependency>

    <!-- SpringBoot的Freemark模板引擎 -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-freemarker</artifactId>
    </dependency>

    <!-- 数据库连接池 -->
    <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>druid-spring-boot-starter</artifactId>
    </dependency>

    <!-- 数据库驱动包 -->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
    </dependency>

    <!-- Lombok -->
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
    </dependency>

    <!-- 只用Swagger注解,缩小依赖的lib包 -->
    <dependency>
        <groupId>io.swagger</groupId>
        <artifactId>swagger-annotations</artifactId>
        <version>1.5.20</version>
    </dependency>

    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-web</artifactId>
    </dependency>

</dependencies>
```

# 代码生成器

```java
import com.baomidou.mybatisplus.annotation.DbType;
import com.baomidou.mybatisplus.annotation.FieldFill;
import com.baomidou.mybatisplus.generator.AutoGenerator;
import com.baomidou.mybatisplus.generator.config.*;
import com.baomidou.mybatisplus.generator.config.po.TableFill;
import com.baomidou.mybatisplus.generator.config.rules.NamingStrategy;

import java.util.Arrays;

public class CodeGenerator {
    // 输出目录
    private static final String OUTPUT_DIR = "/xuecheng-plus-generator/src/main/java";
    // 父包名
    private static final String PARENT_PACKAGE_NAME = "com.xuecheng";
    // 模块名
    private static final String MODULE_NAME = "content";

    // 数据库地址
    private static final String DATA_SOURCE_URL = "192.168.101.65:3306/xcplus_content";
    // 数据库用户名
    private static final String DATA_SOURCE_USER_NAME  = "root";
    // 数据库密码
    private static final String DATA_SOURCE_PASSWORD  = "mysql";

    // 生成的表
    private static final String[] TABLE_NAMES = new String[]{
        "course_base",
        "course_market",
        "course_teacher",
        "course_category",
        "teachplan",
        "teachplan_media",
        "course_publish",
        "course_publish_pre"
    };

    // 一般情况下要先生成DTO类,然后修改此参数再生成PO类
    private static final Boolean IS_DTO = false;

    public static void main(String[] args) {
        // 代码生成器
        AutoGenerator autoGenerator = new AutoGenerator();
        autoGenerator.setTemplateEngine(new FreemarkerTemplateEngine()); // 设置模板引擎
        
        // 全局配置
        GlobalConfig globalConfig = new GlobalConfig();
        globalConfig.setOutputDir(System.getProperty("user.dir") + OUTPUT_DIR); // 设置输出目录
        globalConfig.setFileOverride(true); // 设置文件存在时是否覆盖
        globalConfig.setAuthor("GJT"); // 设置作者
        globalConfig.setOpen(false); // 设置生成后是否自动打开目录
        globalConfig.setSwagger2(false); // 设置是否生成Swagger注解
        globalConfig.setServiceName("%sService"); // 设置Service接口名后缀
        globalConfig.setIdType(IdType.AUTO); // 设置主键策略
        globalConfig.setBaseResultMap(true); // 设置是否生成BaseResultMap
        globalConfig.setBaseColumnList(true); // 设置是否生成BaseColumnList
        autoGenerator.setGlobalConfig(globalConfig); // 设置全局配置

        if (IS_DTO) {
			globalConfig.setSwagger2(true);
			globalConfig.setEntityName("%sDTO");
		}

        // 数据源配置
        DataSourceConfig dataSourceConfig = new DataSourceConfig();
        dataSourceConfig.setDbType(DbType.MYSQL); // 设置数据库类型
        dataSourceConfig.setUrl("jdbc:mysql://" + DATA_SOURCE_URL + "?serverTimezone=UTC&useUnicode=true&useSSL=false&characterEncoding=utf8"); // 数据库连接URL
        dataSourceConfig.setDriverName("com.mysql.cj.jdbc.Driver"); // 数据库驱动类名
        dataSourceConfig.setUsername(DATA_SOURCE_USER_NAME); // 数据库用户名
        dataSourceConfig.setPassword(DATA_SOURCE_PASSWORD); // 数据库密码
        autoGenerator.setDataSource(dataSourceConfig); // 设置数据源配置

        // 包配置
        PackageConfig packageConfig = new PackageConfig();
        packageConfig.setParent(PARENT_PACKAGE_NAME); // 设置父包名
        packageConfig.setModuleName(MODULE_NAME); // 设置模块名
        packageConfig.setMapper("mapper"); // 设置Mapper接口所在的子包名
        packageConfig.setXml("mapper"); // 设置Mapper XML文件所在的子包名
        packageConfig.setEntity("model.po"); // 设置实体类所在的子包名
        packageConfig.setService("service"); // 设置Service所在的子包名
        packageConfig.setServiceImpl("service.impl"); // 设置ServiceImpl所在的子包名
        packageConfig.setController("controller"); // 设置Controller所在的子包名
        autoGenerator.setPackageInfo(packageConfig); // 设置包配置

        // 模板配置
        TemplateConfig templateConfig = new TemplateConfig();
        autoGenerator.setTemplate(templateConfig); // 设置模板配置

        // 策略配置
        StrategyConfig strategyConfig = new StrategyConfig();
        strategyConfig.setInclude(TABLE_NAMES); // 指定需要生成代码的表名
        strategyConfig.setNaming(NamingStrategy.underline_to_camel); // 设置表名转类名策略
        strategyConfig.setColumnNaming(NamingStrategy.underline_to_camel); // 设置列名转属性名策略
        strategyConfig.setEntityLombokModel(true); // 设置实体类使用Lombok模型
        strategyConfig.setRestControllerStyle(true); // 设置Controller使用REST风格
        strategyConfig.setControllerMappingHyphenStyle(true); // 设置Controller映射地址使用连字符风格
        strategyConfig.setTablePrefix(MODULE_NAME + "_"); // 设置表名前缀
        strategyConfig.setEntityBooleanColumnRemoveIsPrefix(true); // 设置实体类Boolean属性使用getXXX()方法
        strategyConfig.setTableFillList(Arrays.asList( // 设置表填充字段
            new TableFill("create_date", FieldFill.INSERT),
            new TableFill("change_date", FieldFill.INSERT_UPDATE),
            new TableFill("modify_date", FieldFill.UPDATE)
        ));
        autoGenerator.setStrategy(strategyConfig); // 设置策略配置
                
        // 执行生成
        autoGenerator.execute();
    }
}
```