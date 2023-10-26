---
title: Mysql
date: '2023-09-28 16:57:52'
updated: '2023-10-26 17:20:54'
permalink: /post/mysql-1q3asg.html
comments: true
toc: true
---

# Mysql

# 基础篇：

数据库管理系统--->SQL--->数据库

## SQL分类：

DDL：定义，定义数据库对象（数据库、表、字段）

DML：操作CRUD

DQL：查询

DCL：控制,创建用户，控制数据库权限

‍

### DDL: 库和表的操作

* show databases;
* select database();   查询当前数据库
* create database name;
* drop database name;
* user database[user name;]

### DML:  增删改

* insert
* delete  from xxx where
* update  xxx  set xx=xx where

‍

### DQL:  查

select  

where 

聚合：count、sum、avg、max、min

分组：group by 字段 having  xxx

          执行顺序： where>聚合函数>having

排序：order by 字段 [asc,desc]

分页查询：limit a,b    [a起始索引，b结束索引]

​![image](assets/image-20231017161343-lhsitjc.png)​

‍

### DCL：【一般运维工程师来进行这部分的操作】

​![image](assets/image-20231017161702-93i09wj.png)​

​![image](assets/image-20231017162510-r6jygen.png)​

## **函数：**

1. 字符串函数：

    * concat  字符串拼接
    * lower     转小写
    * upper    转大写
    * trim       去除两端的空格
    * substring   截取字符
    * lpad   lpad('11',5,'-') --->"---11"   左填充
    * rpad   右填充
2. 数值函数：

    * ceil   向上取整
    * floor  向下取整
    * mod  取模  mod(3%4)=3
    * rand  介于0-1随机数
    * round  四舍五入（参数1，参数2） 参数1:  数字，参数2：保留的小数位数
3. 日期函数

    * curdate()  当前日期
    * curtime()  当前时间
    * now()    当前日期+时间
    * year(date)   获取日期年份
    * month(date) 获取日期月份
    * day(date)  获取日期天数
    * date_add(date,interval x year/month/day)  date+x year/month/day
    * datediff(date1,date2)  日期差值   date1-date2
4. 流程函数：

    * if(express,'Ok','Error')
    * ifnull(e,defult)  若e=null  返回default
    * case [字段] when [a] then [value-a] else [value-a2] end
    * case when [express] then [value1] else [value2] end

​![image](assets/image-20231017171443-u8dfms3.png)​

‍

## **约束：**

​![image](assets/image-20231018103854-uvj9c5h.png)​

外键约束：

​![image](assets/image-20231018111721-19zm68q.png)​

cascade使用:

​`alter table ​`​`**user ​**`​`add  constrain fk foreign key(t_id) refernce ​`​`**parent**`​`(id) on update cascade on delete cascade`​

user parent 表

## 多表查询

### 多表关系：

* 一对多      【员工和老板】
* 一对一      【用户和用户详情】**多用于单表拆分**
* 多对多      【学生和课程】

  ‍

​![image](assets/image-20231018140749-qflu4ms.png)​

### 多表查询：

​![image](assets/image-20231018141718-ksslgon.png)​

#### 内连接：

* 隐式内连接

  ​`select e.name,d.name from emp e , dept d where e.dept_id = d.id;`​
* 显示内连接

  ​`select e.name, d.name from emp e join dept d  on e.dept_id = d.id;`​

#### 外连接：

* 左外连接

  ​`select e.*, d.name from emp e left join dept d on e.dept_id = d.id;`​
* 右外连接

#### 自连接：

​`select a.name , b.name from emp a , emp b where a.managerid = b.id;`​

#### 联合查询：

```sql
select * from emp where salary < 5000
union all
select * from emp where age > 50;
```

uninon all

去除all ：可以去除重复值

​![image](assets/image-20231018144136-qkz48yu.png)​

#### 子查询：

​![image](assets/image-20231018144259-gv3ofed.png)​

​![image](assets/image-20231018144906-pg2hre7.png)​

## 事务

​![image](assets/image-20231018155414-ihr1hgg.png)​

开启事务

提交事务

回滚事务

‍

### **四大特性：**

* 原子性
* 一致性
* 隔离性
* 持久性

​![image](assets/image-20231018160842-xxo7vpf.png)​

### **并发事务：**

​![image](assets/image-20231018161233-ql7ew9p.png)​

### 事务的隔离级别：

​![image](assets/image-20231018161837-b4gpc9y.png)

 【x】是不会发生该现象

​![image](assets/image-20231018162050-iyw6d07.png)​

​![image](assets/image-20231018163528-5doqxq8.png)​

‍

# 进阶篇

## 存储引擎

### MySQL体系结构

​![image](assets/image-20231018164731-zykp4vd.png)​

### 存储引擎介绍

​![image](assets/image-20231018164924-zei8fay.png)`engine=xxx   指定存储引擎，mysql默认是Innodb引擎`​

​`show engines;  查看数据库所有存储引擎`​

‍

### 存储引擎特点

Innodb   高可靠、高性能

**事务、行锁、外键**

​![image](assets/image-20231018165611-qok7flh.png)​

​![image](assets/image-20231018170209-ptj2jdm.png)page:16K

Extent:1MB

​​![image](assets/image-20231018170639-y0v2z0z.png)​​

### 存储引擎选择

​![image](assets/image-20231018170742-vhp177t.png)​

## 索引

### 索引概述

​![image](assets/image-20231019142006-epng2gw.png)

优缺点：

**提高了检索速度，但是降低了更新表的速度**

‍

### 索引结构

* B-Tree 多路平衡查找树
* B+ 树    非叶子节点存储索引，不存储数据，叶子节点存储数据，叶子节点形成一个单向链表
* Hash
* ‍

​![image](assets/image-20231019144624-t6jkwjq.png)​

​![image](assets/image-20231019145013-noc4vw9.png)

​

​![image](assets/image-20231019145350-6iehgyg.png)​

### 索引分类

​![image](assets/image-20231019145447-kg4hr3w.png)​

​![image](assets/image-20231019145609-qx63mvm.png)​

​![image](assets/image-20231019145738-lnx2a12.png)​

​![image](assets/image-20231019150240-n90cda8.png)​

​![image](assets/image-20231019150921-nc367ir.png)​

‍

### 索引语法

* 创建
* 查看
* 删除

​![image](assets/image-20231019151112-hgpxb4h.png)​

### SQL性能分析

#### **SQL执行频率：**

​`show global/session status like 'Com_______';`​

​![image](assets/image-20231019153543-630oavu.png)​

#### **慢查询日志：**

show variables like 'slow_query_log'

‍

#### **profile详情：**

​`select @@have_profiling`​

​`select @@profiling`​

​`set profiling =1;`​

show profiles

​![image](assets/image-20231019154606-aqn7zfd.png)​

#### explain执行计划：

​![image](assets/image-20231019161119-177lpoj.png)​

**const  使用唯一索引会出现const**

**type类型很重要**

​![image](assets/image-20231019161004-vb8563z.png)​

### 索引使用

#### 最左前缀法则:

​![image](assets/image-20231019163421-ud698sy.png)

#### 范围查询:

![image](assets/image-20231019163800-rh1xhtk.png)**&gt;=30时不会导致右侧列表索引失效**

**&gt;30时会导致右侧列表索引失效**

‍

#### **索引失效：**

* *索引上不能进行运算*，否则会导致索引失效
* *字符串类型不加引号*也会导致索引失效
* 模糊查询：

  首部匹配后面模糊不会导致索引失效

  前面模糊会导致索引失效
* ​![image](assets/image-20231019165739-v8b5n53.png)
* mysql自己进行评估索引和全表扫描所花费时间,如果走索引更慢,此时索引也会失效

#### SQL提示

​![image](assets/image-20231024102228-le5rf44.png)​

#### 覆盖索引

​![image](assets/image-20231024102606-edv4omi.png)​

​![image](assets/image-20231024103017-cqq2bv7.png)​

​![image](assets/image-20231024103601-oclcyhi.png)`select  *`​很容易造成回表查询，降低性能

‍

#### 前缀索引：处理长文本

​![image](assets/image-20231024104114-isspm17.png)​

#### 单列索引和联合索引

* 单列索引：索引只包含一列
* 联合索引：索引包含多列

建议多使用联合索引，提高性能。单列索引查询多个字段时容易出现回表查询降低性能

‍

### 索引设计原则

​![image](assets/image-20231024105858-01j2995.png)​

数据量大：数据量超百万

## SQL优化

### 插入数据

* 批量插入（建议500~1000条）
* 手动提交事务（减少事务提交频次）
* 主键顺序插入

‍

​​![image](assets/image-20231024111628-jxqw3jb.png)​​

‍

### 主键优化

#### 数据组织方式：

​![image](assets/image-20231024140815-e95ksdn.png)​

#### 页分裂：

​![image](assets/image-20231024141530-hrdcyne.png)​

#### 页合并：

​![image](assets/image-20231024141639-zb2oh5u.png)​

‍

#### 主键的设计原则：

‍

​![image](assets/image-20231024142047-orwy4c7.png)​

‍

### order by优化

​![image](assets/image-20231024142427-cr1hw8h.png)​

​![image](assets/image-20231024143420-awlihdl.png)​​![image](assets/image-20231024143432-7qsv6jp.png)​

​![image](assets/image-20231024143748-zvddbep.png)​

‍

### group by优化

​![image](assets/image-20231024144317-aqwooxa.png)​

‍

### limit优化

​![image](assets/image-20231024144839-kow0mey.png)​

‍

### count优化

​![image](assets/image-20231024145138-njd5r9c.png)​

​![image](assets/image-20231024145742-xa9i386.png)​​![image](assets/image-20231024145828-crxphd4.png)​

### update优化

​![image](assets/image-20231024150456-69j7sk9.png)​

## 视图/存储过程/触发器

### 视图

​![image](assets/image-20231024152026-v52zcte.png)​

​![image](assets/image-20231024153345-3ou410z.png)​

​`cascaded[v2]`​：级联, 向前检查视图v1，会自动检查条件是否满足,即无论是否附加cascaded或local，都会检查

​`local[v2]`​：向前检查视图v1，不会自动检查条件是否满足，会根据依赖的视图是否附加cascaded或local来进行检查

​![image](assets/image-20231024155100-9fqim5y.png)​

作用：

### ​![image](assets/image-20231024155511-w22css8.png)存储过程

​![image](assets/image-20231025103428-8ry5wou.png)​

​![image](assets/image-20231025103542-tz81brq.png)​

## 锁

### 概述

​![image](assets/image-20231025104143-4k5i90m.png)​​![image](assets/image-20231025104236-iuw5yq0.png)​

### 全局锁：【一般用于数据库的备份】

​![image](assets/image-20231025104314-osux4jv.png)​​![image](assets/image-20231025104852-brfasvl.png)​

开启全局锁：`flush tables with read lock;`​

关闭全局锁：`unlock tables;`​

​![image](assets/image-20231025105438-rqz4hn0.png)​

### 表级锁

#### ​![image](assets/image-20231025105723-l73tslb.png)表锁：

* 表共享读锁：**所有**客户端都可以**读**，但**不可以写**
* 表独占写锁：只有**当前**加锁的客服端可以**读写**，其它客户端不能读写

​![image](assets/image-20231025110525-1jl6mje.png)​

#### 元数据锁（MDL）

​![image](assets/image-20231025110830-zdyalf2.png)

#### 意向锁

​![image](assets/image-20231025111639-44nijb6.png)​​![image](assets/image-20231025111811-l5ja55l.png)​​![image](assets/image-20231025111906-9kmlflp.png)​

‍

‍

### 行级锁

#### ​![image](assets/image-20231025140932-fp3qe20.png)行锁：

​![image](assets/image-20231025141212-lzzfble.png)​

​![image](assets/image-20231025141338-lvchfjb.png)​​![image](assets/image-20231025142454-vfur85u.png)​

​![image](assets/image-20231025143907-vjbn2ud.png)​

## InnoDB引擎

### 逻辑存储结构

​​![image](assets/image-20231025144959-lco3q07.png)​​

‍

‍

### 架构

内存结构：

​![image](assets/image-20231025145428-xyhas7h.png)![image](assets/image-20231025145644-8qzvfc6.png)![image](assets/image-20231025145840-2tbv0ie.png)​

​![image](assets/image-20231025150030-jyr29d8.png)​

磁盘结构：

​![image](assets/image-20231025150340-2cb87xq.png)​​![image](assets/image-20231025150702-3owgwwm.png)​​![image](assets/image-20231025150900-f201pk0.png)

‍

**内存数据刷入磁盘数据时所使用的后台线程：**

‍

​​![image](assets/image-20231025151557-6is03fi.png)

‍

​

### 事务原理

​![image](assets/image-20231025152004-btf42jf.png)​

​`redo log`​:

​![image](assets/image-20231025152707-fb1urvk.png)`undo log`​: 

​![image](assets/image-20231025153124-qp9kc9z.png)**一致性**：undo log+redo log

**隔离性**：锁+MVCC

### MVCC

#### **基本概念：**

​![image](assets/image-20231025153553-kvsey6g.png)​

#### 实现原理

##### 隐藏字段

​![image](assets/image-20231025154029-4g4jsmp.png)

##### undo log

​![image](assets/image-20231025154533-zfzira2.png)​

​![image](assets/image-20231025155117-mkzul17.png)

##### ReadView

​![image](assets/image-20231025155613-a9zp149.png)​

​​![image](assets/image-20231025155551-91nya28.png)​​

​![image](assets/image-20231025160203-dropbb7.png)​

​![image](assets/image-20231025160614-ha3pc4w.png)

# 运维篇

## 日志

### 错误日志

​![image](assets/image-20231026101645-sc5nk4w.png)​

### 二进制日志

​![image](assets/image-20231026102214-kxshm5j.png)​​![image](assets/image-20231026102545-8qevb0t.png)​​![image](assets/image-20231026103349-kdhq9ir.png)​​![image](assets/image-20231026103406-lakmk80.png)​​![image](assets/image-20231026103712-2pmg3zn.png)​

### 查询日志

​![image](assets/image-20231026104454-yy11w78.png)​

### 慢查询日志

​![image](assets/image-20231026104844-3hrrab7.png)​​![image](assets/image-20231026105303-ws65t3h.png)​

## 主从复制

​![image](assets/image-20231026111554-dfjh1cv.png)原理：

​![image](assets/image-20231026112145-tr03ecn.png)​

‍

‍

## <span style="font-weight: bold;" class="mark">分库分表</span>

### **介绍：**

​![image](assets/image-20231026142338-ua8yz02.png)​​![image](assets/image-20231026142456-7dyosa9.png)​

拆分策略：

​![image](assets/image-20231026142634-9hd4syg.png)​

#### 垂直分库：

​![image](assets/image-20231026142806-8e83a75.png)​

#### 垂直分表：

​​![image](assets/image-20231026142926-m3qyzkd.png)​​

‍

#### 水平拆分：

​![image](assets/image-20231026143054-k0fqjkm.png)​

#### 水平分表：

​![image](assets/image-20231026143154-cv2er1w.png)​

‍

### 实现技术：

*shardingJDBC、MyCat*

***MyCat:***

​![image](assets/image-20231026144519-jfsqzog.png)​

**MyCat配置：**

​![image](assets/image-20231026152918-pzwpdye.png)

‍

​![image](assets/image-20231026153100-5l2i719.png)注意：name属性区分大小写

​![image](assets/image-20231026153434-kzf65ob.png)​

​![image](assets/image-20231026153635-ebsnvw7.png)![image](assets/image-20231026153700-dzim9ef.png)​

​![image](assets/image-20231026154113-vnmzh5w.png)​

​![image](assets/image-20231026154249-cw4sm15.png)

​![image](assets/image-20231026154553-5nk44l6.png)​

‍

案例：

​![image](assets/image-20231026162751-98b8m6c.png)​**垂直拆分后，不同机器上的数据库中的表进行多表查询时会出错，不允许这种方式！**

**解决办法：设置全局表**

​![image](assets/image-20231026164234-od2uxel.png)​

**水平拆分：**

​![image](assets/image-20231026164955-l3l22jd.png)​

‍

‍

‍

‍

‍

‍

读写分离

‍
