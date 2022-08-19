---
title: MySQL笔记
date: 2022-03-01 15:17
---
### SQL的四种语句类型
1. DDL（简单来说就是改表结构的操作）
2. DML（一般的数据查询，比如SELECT）
3. DCL（授予权限，创建新用户等等）
4. TCL（涉及事务的关键操作）
### Innodb 引擎和 MyISAM 引擎的区别：
1. Innodb 是 MySQL 5.1 之后的默认存储引擎，MyISAM 是 5.1 之前的默认存储引擎
2. Innodb 支持事务，MyISAM 不支持事务
3. Innodb 支持外键，MyISAM 不支持外键
4. Innodb 使用聚簇索引，MyISAM 使用非聚簇索引
5. Innodb 对 select count(*) from table 这样的查询会执行全表扫描，但是 MyISAM 为此专门保存了一个用于统计表行数的变量可以直接调用
6. Innodb 最小粒度锁是行锁，MyISAM 的最小粒度是表锁
### Innodb 实现事务的大致过程 
1. 先通过 where 条件找到所有的数据页，然后将数据页的数据放入 BufferPool （内存）中
2. 对 BufferPool 中的数据执行对应语句
3. 生成一个 RedoLog 存入 BufferPool 中
4. 生成一个 UndoLog 存入 BufferPool 中以防事务回滚
5. 如果事务提交，将 RedoLog 对象进行持久化，后续有异步任务会将 BufferPool 中的修改持久化到磁盘中
6. 如果事务回滚，使用 UndoLog
### MySQL 慢查询的常规优化
1. 先调查查询是否使用了索引，没使用的话则使用索引
2. 如果用了索引是否为最优索引（三星索引的概念），不是最优索引的话考虑使用最优索引
3. 索引无法优化考虑查询是否查了冗余字段
4. 是否单表数据过多，需要分表
5. 节点服务器性能优化
### 聚簇索引的两个特点
1. 数据存储的位置在磁盘中是物理相邻的，保证了I/O效率
2. 叶子节点存储了完整的数据
### 哈希索引
### 实用命令
1. EXPLAIN EXTENDED: explain命令加长版
2. SHOW FULL PROCESSLIST: 显示当前实例中所有正在运行的用户线程
3. SHOW STATUS LIKE 'Last_Query_Cost': 计算一个SQL查询的消费值