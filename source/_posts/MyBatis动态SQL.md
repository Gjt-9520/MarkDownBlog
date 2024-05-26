---
title: "动态SQL"
date: 2024-06-08
description: ""
cover: https://github.com/Gjt-9520/Resource/blob/main/Bimage-135/Bimage41.jpg?raw=true
tags: ["MyBatis"]
category: "学习笔记"
updated: 2024-06-09
  
top_group_index: 
---

# 动态SQL

动态SQL:随着用户的输入或外部条件的变化而变化的SQL语句

# `<if>`

作用:用于判断条件是否成立,使用test属性进行条件判断,如果条件为true,则拼接SQL

范例:

```sql
<if test="name!=null">
    name like concat('%',#{name},'%')
</if>
```

# `<where>`

作用:
1. 只会在子元素有内容的情况下才插入where子句
2. 自动的去除子句开头的and/or

范例:

```xml
<select id="list3" resultType="com.jinzhao.pojo.Emp">
    select *
    from emp
    <where>
        <if test="name != null">
            name like concat('%', #{name}, '%')
        </if>
        <if test="gender != null">
            and gender = #{gender}
        </if>
        <if test="begin != null and end != null">
            and entry_date between #{begin} and #{end}
        </if>
    </where>
    order by update_time desc
</select>
```

# `<set>`

作用:动态的在行首插入set关键字,并会删除额外的逗号(用在update语句中)

范例:

```java
<update id="update2">
    update emp
    <set>
        <if test="username != null">username=#{username},</if>
        <if test="name != null">name=#{name},</if>
        <if test="gender != null">gender=#{gender},</if>
        <if test="image != null">image=#{image},</if>
        <if test="job != null">job=#{job},</if>
        <if test="entryDate != null">entry_date=#{entryDate}</if>
        <if test="deptId != null">dept_id=#{deptId},</if>
        <if test="updateTime != null">update_time=#{updateTime}</if>
    </set>
    where id = #{id}
</update>
```

# `<foreach>`






# `<sql>`


# `<include>`