---
title: MySQL的锁机制
date: 2020-09-14 16:38:23
tags: [java,mysql,锁]
categories: java
copyright: true
photos: "https://yaoyao-blog.oss-cn-beijing.aliyuncs.com/photo/mysql-lock/msyql_lock_head.png"
---



# MySQL的锁机制



关于一些数据库的锁，之前有写过：[锁和并发控制](http://www.fatyao.top/2020/09/14/lock-concurrence/#more)
这篇着重讲讲MySQL的锁机制，由之前的理论转实际

## 1.MySQL的锁

[MySQL 8.0 innodb-locking](https://links.jianshu.com/go?to=https%3A%2F%2Fdev.mysql.com%2Fdoc%2Frefman%2F8.0%2Fen%2Finnodb-locking.html)
数据库里的锁可以按不同角度划分，如下图。比较值得关注的是锁模式。锁粒度的话，InnoDB只支持行锁和表锁。**注意⚠️行锁是加在索引上的。**



![img](https://yaoyao-blog.oss-cn-beijing.aliyuncs.com/photo/mysql-lock/mysql-lock1.png)

*数据库里的锁* 来源：https://zhuanlan.zhihu.com/p/31875702

##### 加锁机制：

- 乐观锁：先修改，保存时判断是否被更新过，应用级别
- 悲观锁：先获取锁，再操作修改，数据库级别

##### 锁粒度：

- 表级锁：开销小，加锁快，粒度大，锁冲突概率大，并发度低，适用于读多写少的情况。MyISAM存储引擎。
- 页级锁：BDB存储引擎
- 行级锁：Innodb存储引擎，默认选项

##### 兼容性：

- S锁：也叫做读锁、共享锁，对应于 `select * from users where id =1 lock in share mode`
- X锁：也叫做写锁、排它锁、独占锁、互斥锁，对应于 `select * from users where id =1 for update`

##### 锁模式：

**（记录锁、间隙锁、临键锁都是排它锁）**

- **记录锁(Record Locks)：**封锁一个索引记录，也叫行锁，其他事务只能读被锁的记录，不能insert、update、delete。

- 间隙锁(Gap Locks)：

  封锁索引记录中的间隔，或者查询到的第一条索引记录之前的范围，又或者查询到的最后一条索引记录之后的范围。

  - 多个事务可以获得同一个gap的间隙锁，间隙锁的唯一目的就是防止在间隙中insert数据。
  - 共享间隙锁与排他间隙锁没有区别。

- 临键锁(Next-key Locks)：

  是记录锁与间隙锁的组合，它的封锁范围，既包含索引记录，又包含索引区间。

  - 临键锁主要是为了避免幻读。如果把事务的隔离级别降级为RC，临键锁则会失效。

- **意向锁(Intention Locks)：**简单来说是帮助更快地判断当前表加了什么样的行锁。如果另一个任务试图在该表级别上应用共享或排它锁，则受到由第一个任务控制的表级别意向锁的阻塞。第二个任务在锁定该表前不必检查各个页或行锁，而只需检查表上的意向锁。

- 插入意向锁(Insert Intention Locks)：

  该锁用以表示插入意向，当多个事务在同一区间（gap）插入位置不同的多条数据时，事务之间不需要互相等待（允许并发insert）。

  - 插入意向锁是一种间隙锁，但它不妨碍多个事务同时insert，多个插入意向锁可以兼容。但此时如果有事务要进行加间隙锁的查询，需要等待，不是因为间隙锁和插入意向锁冲突，而是因为insert操作和间隙锁是冲突的。
  - 例1：假设已有数据4和7，有A、B线程分别要插入5、6。A、B线程先分别获得4～7这个间隙的插入意向锁，然后各自插入数据，A线程获得数据5的记录锁，B线程获得数据6的记录锁。
  - 例2：A线程查询数据并获得4～7的间隙锁，此时B线程想插入数据6，B线程可以获得4～7的插入意向锁，但B线程需要等待A释放间隙锁，才可以insert加记录锁。

## 2.不同SQL语句加的锁

### 2.1.普通select

在4个事务隔离级别中，除了在串行化(Serializable)时会加**共享锁**，其他的都不加锁。因为MySQL(InnoDB)支持MVCC，对普通select语句能够进行快照读，很好地支持了读写并发。
[InnoDB学习笔记（1）MVCC](https://www.jianshu.com/p/e22b588e2ff4)

### 2.2.显式加锁select

显式加锁select主要是指：

- `select ... for update`（加互斥锁）
- `select ... in share mode`（加共享锁）

（要在事务中使用这些锁，否则不在事务中是没有意义的）

#### 聚集索引等值查询

聚集索引上：记录锁

```csharp
select * from user where id=10 for update
```



![img](https://yaoyao-blog.oss-cn-beijing.aliyuncs.com/photo/mysql-lock/mysql-lock2.png)

来源：https://zhuanlan.zhihu.com/p/31875702

#### 唯一索引(非聚集索引)等值查询

聚集索引上：记录锁
辅助索引(唯一索引)上：记录锁

- 为什么聚簇索引上的记录也要加锁？试想一下，如果有并发的另外一个SQL，是直接通过主键索引id=30来更新，会先在聚集索引中请求加锁。如果只在辅助索引中加锁的话，两个并发事务之间是互相感知不到的。

```csharp
select * from t where id = 10  for update; # id是唯一索引列
```



![img](https://yaoyao-blog.oss-cn-beijing.aliyuncs.com/photo/mysql-lock/mysql-lock3.png)

来源：https://zhuanlan.zhihu.com/p/31875702

#### 非唯一索引等值查询

聚集索引：记录锁
辅助索引：临键锁（记录锁 + 间隙锁）

```csharp
select * from user where name=‘b’ for update
```



![img](https://yaoyao-blog.oss-cn-beijing.aliyuncs.com/photo/mysql-lock/mysql-lock4.png)

来源：https://zhuanlan.zhihu.com/p/31875702

#### 无索引查询

(全表)聚集索引：临键锁（记录锁 + 间隙锁）
即，对全表每一条记录加记录锁，每一个间隙加间隙锁

#### 记录不存在

相应索引：间隙锁
要保证没有其他人可以插入，所以锁住间隙

### 2.3.隐式加锁update和delete

对于update和delete语句，会隐式加锁。加锁的情况和上述显式加锁一样。

```csharp
update user set address '北京' where id=1;
delete from user where id=1;
```

### 2.4.普通insert

(插入区间)聚集索引：插入意向锁
(插入记录)聚集索引：记录锁

```csharp
insert into child (id) values (90),(102);
```

### 2.5.先select后insert

(插入区间)聚集索引：插入意向锁
(插入记录)聚集索引：记录锁
(select的源表)聚集索引：共享的临键锁。这是为了防止主从同步出问题。

```csharp
insert into target_table select * from source_table ...
create target_table select * from source_table ...
# 将select查询的结果集，插入到另一张表中，或者使用结果集，创建一个新表。
```

> 参考资料：
> [https://zhuanlan.zhihu.com/p/31875702](https://links.jianshu.com/go?to=https%3A%2F%2Fzhuanlan.zhihu.com%2Fp%2F31875702)
> [https://blog.csdn.net/zcl_love_wx/article/details/82386143](https://links.jianshu.com/go?to=https%3A%2F%2Fblog.csdn.net%2Fzcl_love_wx%2Farticle%2Fdetails%2F82386143)
> [https://zhuanlan.zhihu.com/p/48269420](https://links.jianshu.com/go?to=https%3A%2F%2Fzhuanlan.zhihu.com%2Fp%2F48269420)
> [https://segmentfault.com/a/1190000014133576](https://links.jianshu.com/go?to=https%3A%2F%2Fsegmentfault.com%2Fa%2F1190000014133576)