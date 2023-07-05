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
1. 可以将数据全部caceh到spark的executor中
2. 将数据cache到redis中
缺点: cache的数据量不能太大
{{< /note >}}
## 减少每次join的io次数
{{< note info >}}
1. 优化join逻辑,例如如果一个批次的spark流数据去lookup Hbase的话,如果从逻辑上看,这个批次的数据,都只根据一个固定的值去join的话,
就只需要请求一次hbase就可以,而不是这一个批次的数据都分配请求Hbase
2. 将关联逻辑以协处理器的方式放到hbase服务器端,这样spark任务就可以用很少的io请求得到需要的数据
{{< /note >}}
