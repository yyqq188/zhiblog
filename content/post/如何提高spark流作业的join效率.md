---
title: "如何提高spark流作业的join效率"
date: 2023-07-04T16:48:43+08:00
draft: true
toc : true
---


spark流作业的join,会涉及很多的IO操作,这也是限制spark流作业执行效率的最大障碍.
围绕着这一问题,有下面的几点需要优化:
## 尽量将被关联的数据放在内存中
{{< note info >}}
1. 可以将数据全部cache到spark的executor中
2. 将数据cache到redis中

缺点: cache的数据量不能太大
{{< /note >}}
## 减少每次join的io次数
{{< note info >}}
1. 优化join逻辑,例如如果一个批次的spark流数据去lookup Hbase的话,如果从逻辑上看,这个批次的数据,都只根据一个固定的值去join的话,
就只需要请求一次hbase就可以,而不是这一个批次的数据都分配请求Hbase
2. 将关联逻辑以协处理器的方式放到hbase服务器端,这样spark任务就可以用很少的io请求得到需要的数据
{{< /note >}}


---

## spark的logicPlan解析

```java
public static void main(String[] args) throws Exception {
        // 创建 SparkSession
        SparkSession spark = SparkSession.builder()
                .appName("StructuredStreamingExample")
                .master("local[*]")
                .getOrCreate();
        Dataset<Row> user = spark.read().json(user.json");
        Dataset<Row> level = spark.read().json("level.json");


        user.createOrReplaceTempView("user");
        level.createOrReplaceTempView("level");
        Dataset<Row> sql = spark.sql("select * from " +
                "user join level on user.levelid= level.levelid where user.username=='mack'");
//        sql.explain(true);
//        System.out.println("================");
//        System.out.println(sql.logicalPlan().verboseStringWithSuffix());
//        System.out.println("================");
//        System.out.println(sql.logicalPlan().p(1).verboseStringWithSuffix());
//        System.out.println("================");
//        System.out.println(sql.logicalPlan().p(2).verboseStringWithSuffix());
//        System.out.println("================");
//        System.out.println(sql.logicalPlan().p(2).childrenResolved());
//        System.out.println("================");
//        System.out.println(sql.logicalPlan().p(3).verboseStringWithSuffix());
//        System.out.println("================");
//        System.out.println(sql.logicalPlan().p(3).childrenResolved());
//        System.out.println("================");
//        System.out.println(sql.logicalPlan().p(4).verboseStringWithSuffix());

        for (int i = 0; i < 70; i++) {
            try{
                if(sql.logicalPlan().p(i).childrenResolved()){
                    System.out.println(sql.logicalPlan().p(i).verboseStringWithSuffix());
                }
            }catch (Exception e){
            }
        }
    }
```

从上面的解析sql中来分析下spark的logicPlan的解析规则
```sql
select * from user join level on user.levelid= level.levelid where user.username=='mack'
```

```shell
# project中就是select的内容
Project [levelid#6L, userage#7L, userid#8, username#9, levelid#20, levelname#21]
# filter中就是where的内容
Filter (username#9 = mack)
# join 这一行代表的就是join on 的关联字段
Join Inner, (levelid#6L = cast(levelid#20 as bigint))
# 关联表名一
SubqueryAlias `user`
# 关联表一的所有字段
Relation[levelid#6L,userage#7L,userid#8,username#9] json
# 关联表名二
SubqueryAlias `level`
# 关联表二的所有字段
Relation[levelid#20,levelname#21] json
```

<mark>lambda架构</mark>

## 解析步骤
### 第一步
把每个表的数据分别抽出一条数据,封装为json
### 第二步
利用spark来读取json文件,并注册成临时表
### 第三步
写join的sql语句,分析其中的join where select条件

以上是完成了sql的解析工作,利用的是sparksql自带的解析器

## 分析步骤
### 第一步
根据分析的join,找到被关联的表的关联字段
### 第二步
外部传入被关联表的主键字段
### 第三步
根据关联字段和主键生成spark程序,该spark程序是一方面读取hive或hbase的,
一方面是读取kafka的topic,最终是生成实时变动的hbase新的索引表
这是生成索引的关键步骤

有了解析分析后生成的实时索引表


本质上就是生成实时索引表  也是个通用的模版
然后每次都是去join的时候,先join二级索引表
最好是将这个二级join的步骤放到协处理器中 写个通用的模版
然后配合helper 也是个通用的模版
加上异步操作