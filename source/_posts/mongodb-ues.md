---
title: MongoDB学习笔记
date: 2017-03-27 15:07:03
tags:
  - MongoDB
  - NoSQL
categories: MongoDB
---

本篇文章介绍学习 MongoDB 的一些常用命令,希望能帮助大家.

<!--more-->

## 常用操作

- `Help`查看命令提示
  - `db.help()`;
  - `db.yourColl.help()`;
  - `db.youColl.find().help()`;
- 切换/创建数据库
  - `use yourDB`; 当创建一个集合(`table`)的时候会自动创建当前数据库
- 查询所有数据库
  - `show dbs`;
- 删除当前使用数据库
  - `db.dropDatabase()`;
- 从指定主机上克隆数据库
  - 从指定主机上克隆数据库
- 从指定的机器上复制指定数据库数据到某个数据库
  - `db.copyDatabase("mydb", "temp", "127.0.0.1")`;将本机的`mydb`的数据复制到`temp`数据库中
- 修复当前数据库
  - `db.repairDatabase()`;
- 查看当前使用的数据库
  - `db.getName()`;
- 显示当前 db 状态
  - `db.stats()`;
- 当前 db 版本
  - `db.version()`;
- 查看当前 db 的链接机器地址
  - `db.getMongo()`;

## Collection 聚集集合操作

- 创建一个聚集集合（`table`）
  - `db.createCollection(“collName”, {size: 20, capped: 5, max: 100})`;
- 得到指定名称的聚集集合（`table`）
  - `db.getCollection("account")`;
- 得到当前 db 的所有聚集集合
  - `db.getCollectionNames()`;
- 显示当前 db 所有聚集索引的状态
  - `db.printCollectionStats()`;

## 用户相关操作

- 添加一个用户
  - `db.addUser("name")`;
  - `db.addUser("userName", "pwd123", true)`; 添加用户、设置密码、是否只读
- 数据库认证、安全模式
  - `db.auth("userName", "123123")`;
- 显示当前所有用户
  - `show users`;
- 删除用户
  - `db.removeUser("userName")`;

## 聚集集合查询

- 查询所有记录
  - `db.userInfo.find()`; 相当于：`select* from userInfo`;默认每页显示 20 条记录，当显示不下的情况下,可以用 it 迭代命令查询下一页数据。注意：键入 it 命令不能带“;”但是你可以设置每页显示数据的大小,用 DBQuery.shellBatchSize= 50;这样每页就显示 50 条记录了。
- 查询去掉后的当前聚集集合中的某列的重复数据
  - `db.userInfo.distinct("name")`;会过滤掉 name 中的相同数据,相当于：`select distict name from userInfo`;
- 查询`age = 22`的记录
  - `db.userInfo.find({"age": 22})`; 相当于：`select * from userInfo where age = 22`;
- 查询`age > 22`的记录
  - `db.userInfo.find({age: {$gt: 22}})`;
- 查询`age < 22`的记录
  - `db.userInfo.find({age: {$lt: 22}})`;
- 查询`age >= 25`的记录
  - `db.userInfo.find({age: {$gte: 25}})`;
- 查询`age >= 23` 并且 `age <= 26`
  - `db.userInfo.find({age: {$gte: 23, $lte: 26}})`;
- 查询`name`中包含 `mongo`的数据
  - `db.userInfo.find({name: /mongo/})`;
- 查询`name`中以`mongo`开头的
  - `db.userInfo.find({name: /^mongo/})`;
- 查询指定列`name`、`age`数据
  - `db.userInfo.find({}, {name: 1, age: 1})`;当然`name`也可以用`true`或`false`,当用`ture`的情况下河`name:1`效果一样，如果用`false`就是排除`name`，显示`name`以外的列信息。
- 查询指定列`name`、`age`数据, `age > 25`.
  - `db.userInfo.find({age: {$gt: 25}}, {name: 1, age: 1})`;
- 按照年龄排序
  - 升序：`db.userInfo.find().sort({age: 1})`;
  - 降序：`db.userInfo.find().sort({age: -1})`;
- 查询`name = zhangsan, age = 22`的数据
  - `db.userInfo.find({name: 'zhangsan', age: 22})`;
- 查询前 5 条数据
  - `db.userInfo.find().limit(5)`;
- 查询 10 条以后的数据
  - `db.userInfo.find().skip(10)`;
- 查询在 5-10 之间的数据
  - `db.userInfo.find().limit(10).skip(5)`;可用于分页，limit 是 pageSize，skip 是第几页\*pageSize
- or 与 查询
  - `db.userInfo.find({$or: [{age: 22}, {age: 25}]})`;
- 查询第一条数据
  - `db.userInfo.findOne()`;
  - `db.userInfo.find().limit(1)`;
- 查询某个结果集的记录条数
  - `db.userInfo.find({age: {$gte: 25}}).count()`;如果要返回限制之后的记录数量，要使用 count(true)或者 count(非 0)
  - `db.users.find().skip(10).limit(5).count(true)`;
- 按照某列进行排序
  - `db.userInfo.find({sex: {$exists: true}}).count()`;

## 索引

- 创建索引
  - `db.userInfo.ensureIndex({name: 1})`;
  - `db.userInfo.ensureIndex({name: 1, ts: -1})`;
- 查询当前聚集集合所有索引
  - `db.userInfo.getIndexes()`;
- 查看总索引记录大小
  - `db.userInfo.totalIndexSize()`;
- 读取当前集合的所有 index 信息
  - `db.users.reIndex()`;
- 删除指定索引
  - `db.users.dropIndex("name_1")`;
- 删除所有索引
  - `db.users.dropIndexes()`;
