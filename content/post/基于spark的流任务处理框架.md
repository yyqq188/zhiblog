---
title: "基于spark的流任务处理框架"
date: 2023-07-07T16:50:52+08:00
draft: true
toc: true
---


> 让实时开发任务变得简单

## 前提
本框架是基于lambda架构,需要同时保留hive的批处理,和sparkstreaming的流处理两个部分.

流处理任务只是处理当天的实时任务,当天之前的数据按照历史任务,由hive处理

## 起源
目前实时项目是基于sparkstreaming框架开发，用scala语言做开发语言。

1. 面临大量的业务逻辑开发工作量，且工程代码和业务代码耦合严重，开发效率低下;

2. 后续人员对业务不熟悉，代码中有很多临时添加的逻辑等，导致项目的维护也面临很多困难；

3. scala语言本身的应用范围和使用人员相比sql或java来讲很受局限，导致实时项目很难找到合适的后续开发人员。



目前整体设计思路已经确定
数据从hive到hbase的批+流✅
数据从hbase开始建立二级索引✅
建立二级索引的逻辑✅

接下来重点考虑验证
- 数据质量管理
|参考|涉及到的技术|
|---|---|
|apache griffin|spark ,livy, es|
- 数据血缘分析
|参考|涉及到的技术|
|---|---|
|apache atlas|hbase ,solr, kafka|
- OLAP
clickhouse , presto,impala