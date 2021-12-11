# 数据库和缓存

## 关系型数据库

- 什么是索引？

- 索引的类型有哪些？（PostgreSQL）

  - 按实现算法分类：

     1. B-Tree。

        一类多叉树实现，B Tree 是在各个节点都放置了数据，而 B+ Tree 只在最终节点放置数据。

     2. Hash。

        Hash 结构，只能处理简单的等值比较。即 `=` 比对。

     3. GiST。

        其特色为有能力优化“最近邻”搜索。
        如：`SELECT * FROM places"ORDER BY location <-> point '(101,456)' LIMIT 10;`

     4. SP-GiST。

        功能类似 GiST，实现有所不同。

     5. GIN。

        倒排索引，适合于包含多个组成值的数据值，例如数组。倒排索引中为每一个组成值都包含一个单独的项，可以高效地处理测试指定组成值是否存在的查询。

     6. BRIN。

        块范围索引。存储有关放在一个表的连续物理块范围上的值摘要信息。

  - 按作用范围分类：

     1. 普通索引。

        `CREATE INDEX "index_name" ON "table_name" ("column_name");`

     2. 多列索引。

        `CREATE INDEX "index_name" ON "table_name" ("column_1_name", "column_2_name", ...);`

        多列索引应该较少地使用。在绝大多数情况下，单列索引就足够了且能节约时间和空间。具有超过三个列的索引不太有用，除非该表的使用是极端程式化的。

     3. 唯一索引。

        `CREATE UNIQUE INDEX "index_name" ON "table_name" ("column_name");`

        不需要手工在唯一列上创建索引，如果那样做也只是重复了自动创建的索引而已。

     4. 表达式索引。

        `CREATE INDEX "index_name" ON "table_name" (Expression);`

        如：
        1. `CREATE INDEX "test1_lower_col1_idx" ON "test1" (lower("col1"));`，该索引会在 `SELECT * FROM "test1" WHERE lower("col1") = 'value';` 查询语句中起效。

        2. `CREATE INDEX "people_names" ON "people" ((first_name || ' ' || last_name));`，该索引会在 `SELECT * FROM "people" WHERE (first_name || ' ' || last_name) = 'John Smith';`

        索引表达式的维护代价较为昂贵，因为在每一个行被插入或更新时都得为它重新计算相应的表达式。然而，索引表达式在进行索引搜索时却**不**需要重新计算，因为它们的结果已经被存储在索引中了。

     5. 部分索引。

        如：`CREATE INDEX "access_log_client_ip_idx" ON "access_log" ("client_ip") WHERE NOT ("client_ip" > inet '192.168.100.0' AND "client_ip" < inet '192.168.100.255');`

     6. 操作符类和操作符族索引。

        如：`CREATE INDEX "test_index" ON "test_table" ("column_name" varchar_pattern_ops);`

- 什么是悲观锁？什么是乐观锁？能举出一些例子吗？

- 事务的隔离级别有哪些？都是为了解决什么问题？

- 数据库的事务是如何实现的？

- 介绍一下*聚簇索引*和*非聚簇索引*。

- 什么是*回表*？有什么解决方案？

- 数据库优化方案有哪些？

## 缓存

- Redis 有哪些数据结构？

- Redis 是如何做持久化的？

- Redis 有没有事务？

- Redis 高可用的方案有哪些？

- 如何使用 Redis 来设计一个分布式锁？

- 什么是缓存雪崩、缓存穿透和缓存击穿？解决方案是怎样的？

## 文档型数据库（主要是 MongoDB）

- MongoDB 的特点有哪些？与关系型数据库有什么区别？

- 介绍一下 MongoDB 的原子操作。

- 介绍一下 MongoDB 的索引。创建一个集合的时候，可以不建立索引吗？

- 已知一个需要存储的数据的大小，其存储到 Mongo 时，Mongo 的磁盘占用大小会发生怎样的改变？与存储的数据的大小有什么关联？
