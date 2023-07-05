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

spark的logicPlan解析
```shell
Project [levelid#6L, userage#7L, userid#8, username#9, levelid#20, levelname#21]
Filter (username#9 = mack)
Join Inner, (levelid#6L = cast(levelid#20 as bigint))
SubqueryAlias `user`
Relation[levelid#6L,userage#7L,userid#8,username#9] json
SubqueryAlias `level`
Relation[levelid#20,levelname#21] json
```

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
