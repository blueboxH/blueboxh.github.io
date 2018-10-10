---
title: 解决mybatis中resultMap使用association和collection错误去重的问题
date: 2018-10-09 20:21:20
urlName: Solve-the-problem-of-using-the-association-i-the-resultMap-of-mybatis
tags: 
  - mybatis 
  - association
  - resultMap
  - 错误去重
---

> 问题描述: 在使用mybatis查询数据的时候, 由于没有正确的使用`resultMap`, 导致sql语句查询的总条数比展示出来的数据条数要多. 表现出来就是使一些数据丢死了. 由于没有任何有用的日志打印出来, 用了半天才定位到是`resultMap`错误去重导致的问题.

<!-- more -->

原来的`resultMap`是这样的(与关键点无关的代码就不放出来了):

```xml
<resultMap id="MajorNodeBindDetailsResultMap" type="com.thgy.udevforums.business.majorNode.Model.MajorNodeBindDetailsVo">
  <result column="status" jdbcType="INTEGER" property="status" />
  <association property="majorNodeBindVo" resultMap="MajorNodeBindResultMap" />
  <association property="authenticationVo" resultMap="AuthenticationResultMap" />
  <association property="majorNodeApplyVo" resultMap="MajorNodeApplyResultMap" />
</resultMap>
```

由于正好`association`之外只有一个`status`字段, 且这个字段重复率极高, 才轻易发现了这个bug, 尽管几个`VO`里面的数据不一样, 但是检查数据是否重复的时候并没有把里面的数据作为判断依据, 导致数据丢失. 找了好多错误方向之后终于看到了[这篇文章](https://www.codetd.com/article/2850307), 虽然觉得这种解决方法并不完美, 因为我3个vo的map里面的字段都一个个拿出来会死人的~_~, 但是终于还是从标题找到了一个关键点`association“错误”去除重复数据` , 就再也不用漫无目的的去找答案了. 所以我去找了`resultMap`中`association`的使用方法, 在[https://blog.csdn.net/ilovejava_2010/article/details/8180521](https://blog.csdn.net/ilovejava_2010/article/details/8180521)中看到了以下关键信息, 加上`id`元素作为`resultMap`的唯一标识:

>  **重点提示:***id*元素在嵌套结果映射中扮演了非常重要的角色，您应该总是指定一个或多个属性来唯一标识这个结果集。事实上，如果您没有那样做，MyBatis也会工作，但是会导致严重性能开销。选择尽量少的属性来唯一标识结果，而使用主键是最明显的选择（即使是复合主键）。

修改之后的`resultMap`是这样的:

```xml
<resultMap id="MajorNodeBindDetailsResultMap" type="com.thgy.udevforums.business.majorNode.Model.MajorNodeBindDetailsVo">
  <id column="user_id" property="userId"/>
  <result column="status" jdbcType="INTEGER" property="status" />
  <association property="majorNodeBindVo" resultMap="MajorNodeBindResultMap" />
  <association property="authenticationVo" resultMap="AuthenticationResultMap" />
  <association property="majorNodeApplyVo" resultMap="MajorNodeApplyResultMap" />
</resultMap>
```

至此, 完美解决这个困扰了半天无头绪的bug, 当然, 也要顺手给`VO`中的`resultMap`加上`id`元素.



