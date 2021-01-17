# 存储引擎
数据库存储引擎是数据库底层软件组织，数据库管理系统（DBMS）使用数据引擎进行创建、查询、更新和删除数据。

不同的存储引擎提供不同的存储机制、索引技巧、锁定水平等功能，使用不同的存储引擎，还可以获得特定的功能。

现在许多不同的数据库管理系统都支持多种不同的数据引擎。

存储引擎主要有：
- InnoDB
- MyIsam
- Memory 
- Archive
- Federated 

## InnoDB（B+ 树）
**MySQL 默认事务型引擎**，用来处理大量短期事务，默认隔离级别是 可重复读（REPEATABLE READ） 

InnoDB 底层存储结构为 B+ 树，B 树的每个节点对应 InnoDB 的一个 page，page 大小是固定的，一般设为 16k。

其中非叶子结点只有键值，叶子节点包含完成数据。

InnoDB 是基于聚簇索引建立的，InnoDB 的索引结构和其他存储引擎有很大不同，聚簇索引对主键查询有很高的性能，不过它的二级索引中必须包含主键列，所以如果主键很大的话其他所有索引都会很大，因此如果表上索引较多的话主键应当尽可能小。

适用场景：
1. 经常更新的表，适合处理多重并发的更新请求。
2. 支持事务和行级锁。
3. 可以从灾难中恢复（通过 bin-log 日志等）。
4. 外键约束。**只有它支持外键**。
5. 支持自动增加列属性 auto_increment。

## MyIASM
MyIASM 是 MySQL 默认的引擎，但是它**没有提供对数据库事务的支持**，也**不支持行级锁和外键**，因此当 INSERT 或 UPDATE 数据时即写操作需要锁定整个表，效率便会低一些。

MyIASM 执行**读取操作的速度很快**，而且不占用大量的内存和存储资源。

在设计之初就预想数据组织成有固定长度的长度，按顺序存储——MyIASM 是一种静态索引结构。

**缺点是它不支持事务处理，最大缺陷是崩溃后无法完全恢复**。

MyISAM 将表存储在数据文件和索引文件中，分别以 .MYD 和 .MYI 作为扩展名。

## Memory
Memory（也叫 HEAP）堆内存：使用存在内存中的内容来创建表。

每个 MEMORY 表只实际对应一个磁盘文件。

MEMORY 类型的表访问非常得快，因为它的数据时放在内存中的，并且默认使用 HASH 索引。

但是一旦服务关闭，表中的数据就会丢失掉。

Memory **同时支持散列索引和 B 数索引，B 数所以可以使用部分查询和通配查询**，也可以使用大于，小于和大于等于等操作符方便数据挖掘，散列索引对相等的比较快但是对于范围的比较要慢很多。


































