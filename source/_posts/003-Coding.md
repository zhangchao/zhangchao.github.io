---
title: 003_Coding
date: 2017-08-07 09:53:23
tags: jdbc-sharding
---



SQL解析, SQL路由, SQL 改写, SQL 执行


# jdbc-sharding

StatementRoutingEngine  -- > SQLRouter[parse(SQLStatement),route(SQLRouteResult)] & ShardingStrategy --> SimpleRoutingEngine/ComplexRoutingEngine & SQLRewriteEngine[rewrite]


## SQL Route

逻辑表 --> 多个实际表 , DynamicDataNode 不同于DataNode(逻辑表和真实表有对应关系) 静态配置的， 是存在某种关系，比如：按照时间划分的表 
> 订单数据根据主键尾数拆分为10张表,分别是t_order_0到t_order_9，他们的逻辑表名为t_order


1. Strategy 
databaseShardingStrategy ：分库策略
tableShardingStrategy ：分表策略

2. Key:
generateKeyColumn ：主键字段
keyGenerator ：主键生成器

3. ShardingRule 

dataSourceRule :
TableRules :
bindingTableRules:    t_order o join t_order_item i ,当 t_order 和 t_order_item 为 BindingTable关系 时，计算的是 t_order 路由分片

4. RoutingEngine，路由引擎接口，共有四种实现：

DatabaseHintRoutingEngine：基于数据库提示的路由引擎
SimpleRoutingEngine：简单路由引擎
CartesianRoutingEngine：笛卡尔积的库表路由
ComplexRoutingEngine：混合多库表路由引擎


SQLRoute --> RoutingEngine  返回RoutingResult 
1. Parse 
2. Route 
 

## SQLRewriteEngine

SQLRewriteEngine 基于 SQLToken 实现 SQL改写。SQL解析器在SQL解析过程中，很重要的一个目的是标记需要SQL改写的部分，也就是 SQLToken

1. SQL 改写：改写 SQL，解决分库分表后，查询结果需要聚合，需要对 SQL 进行调整，例如分页
2. SQL 生成：生成分表分库的执行 SQL

各 SQLToken 生成条件如下

1. GeneratedKeyToken 自增主键标记对象
2. TableToken 表标记对象    
3. ItemsToken 选择项标记对象  :  查询column ,order by XXX, group by XXX 
4. OffsetToken 分页偏移量标记对象
5. RowCountToken 分页长度标记对象
6. OrderByToken 排序标记对象 : 当无 ORDER BY条件 而有 GROUP BY 条件时候 SELECT order_id FROM t_order GROUP BY order_id 等价于 SELECT order_id FROM t_order GROUP BY order_id ORDER BY order_id ASC

TableUnit <--> SQLExecutionUnit



# KeyGenerator 

DefaultKeyGenerator，默认的主键生成器。该生成器采用 Twitter Snowflake 算法实现，生成 64 Bits 的 Long 型编号

0 41bit时间 10bit 工作进程 12bit序列号

校验当前时间小于等于最后生成编号时间戳，避免服务器时钟同步，可能产生时间回退，导致产生重复编号

1. 我们使用一组机器作为服务器(<1024) 
2. 可以使用ip 后10位作为编号



# ExecutorEngine

并行执行 SQL
一个分片数据源( ShardingDataSource ) 独占 一个 SQL执行引擎( ExecutorEngine )。




>> PreparedStatementExecutor，多线程执行预编译语句对象请求的执行器。比 StatementExecutor 多了 parameters 参数
>> 



# MergeEngine

在各分片排序完后，Sharding-JDBC 获取到结果后，仍然需要再进一步排序。目前有 分页、分组、排序、聚合列、迭代 五种场景需要做进一步处理。

1. 返回合适的归并结果集接口( ResultSetMerger ) 
Stream 流式：将数据游标与结果集的游标保持一致，顺序的从结果集中一条条的获取正确的数据。看完下文第三节 OrderByStreamResultSetMerger 可以形象的理解。
Memory 内存：需要将结果集的所有数据都遍历并存储在内存中，再通过内存归并后，将内存中的数据伪装成结果集返回。看完下文第五节 GroupByMemoryResultSetMerger 可以形象的理解。
Decorator 装饰者：可以和前二者任意组合


## Example 

select * from order o inner join order_item i on o.order_id = i.order_id
select * from order_0 o inner join order_item_0 i on o.order_id = i.order_id

SqlStatement:

items:
1. CommonSelectItem * 

type:
1. select 

tables:
1. table :order 
2. table :order_item 

sqlToken:
1. TobelToken order 
2. TobelToken order_item 


orderbyitems 


groupbyitems


limit 

distinct : false  


ShardingRule:

dataSourceRule:  ds1 ds0 
tableRules: 

order : ds_0.order0 ,ds_0.order1, ds_1.order0 ,ds_1.order1, 
order_item : ds_0.order_item0 ,ds_0.order_item1, ds_1.order_item0 ,ds_1.order_item0, 
order_attr : ds_0.order_attra, ds_1.order_attrb, 

bindingTableRules:
order
order_attr

databaseShardingStrategy:
shardingCoulmn :  order_id
shadingAlgorthim :

tableShardingStrategy:
shardingCoulmn :  order_id
shadingAlgorthim :

keyGenerator  
DefaultGenerateor 