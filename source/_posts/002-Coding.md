---
title: 002_Coding
date: 2017-08-03 12:00:46
tags: jdbc-sharding
---

# jdbc-sharding

## Lexer 

词法分析，主要目的就是分词(每个数据库词法分析器稍有不同) ，对于SQL 来说有以下几种类型

1. Keyword (词法关键词-各种数据库稍有不同) //SELECT INSERT
2. Symbol (词法符号标记)  // + (  = *
3. Literals (词法字面量标记)  //INT VARIABLE
4. Assist (词法辅助标记) //END, ERROR


ignore 字符 ：
1. 空格
2. 注释

> 比较难处理的 ：
>  变量 ，".",　*
>  Order Group 

```SQL

例子：
 Select  \t \n * from \r\n TABLE_XXX \t

/*--xyz WHERE XX=1 //xyz*/　SELECT * FROM XXX_TABLE # comment 1 #comment 2 WHERE XX=%s AND YY=%s


```


## Parser 

最主要的是得到Statement , Statement 包含两部分信息：

* 分片上下文：用于 SQL 路由。
* SQL 标记对象：用于 SQL 改写。



DeleteParserFactory -->MySQLDeleteParser(implements SQLStatementParser)--> SQLParser
        sqlParser.getLexer().nextToken(); // 跳过 DELETE
        skipBetweenDeleteAndTable();  // 跳过关键字，例如：MYSQL 里的 LOW_PRIORITY、IGNORE 和 FROM
        sqlParser.parseSingleTable(deleteStatement); // 解析表
        sqlParser.skipUntil(DefaultKeyword.WHERE); // 跳到 WHERE
        sqlParser.parseWhere(deleteStatement); // 解析 WHERE

SqlParser --> MySQLParser -->  MySQLLexer &　ShardingRule --> Lexer.nextToken() ==> SELECT/INSERT/UPDATE/DELETE


SQLParser 
1. parseExpression()  解析表达式
2. parseAlias()   解析别名
3. parseSingleTable() 解析单表
4. skipJoin() 跳过表关联词法
5. parseWhere()   解析查询条件   --> condition 

SQLExpression :
SQLIdentifierExpression 标识表达式   Literals.IDENTIFIER
SQLPropertyExpression   属性表达式   无
SQLNumberExpression 数字表达式   Literals.INT, Literals.HEX
SQLPlaceholderExpression    占位符表达式  Symbol.QUESTION
SQLTextExpression   字符表达式   Literals.CHARS
SQLIgnoreExpression 分片中无需关注的SQL表达式  无


SQLStatement
1. Type :获取SQL语句类型
2. Tables： 获取表解析对象集合
3. Conditions 获取条件对象集合.
4. SqlTokens 获取SQL标记集合

SelectStatement
5.  orderby
6.  groupbu
7.  aggregationselectitems
8.  limit 


> DELETE FROM `order` WHERE order_id IN (?)
1. Type :DELETE
2. Tables： order
3. Conditions  order.order_id <--> in 
4. SqlTokens  `order`


> parseSingleTable  select * from (select )
 需要处理 ( , 使用alais 作为table 名字 


## sharding 

Sharding Rule  主要包含这些类型

dataSourceRule  : ds0 ds1  
tableRule : ds0-table1 ds1-table1  ds0-table1 ds0-table2
bindingTableRules: ??

databaseShardingStrategy : DatabaseShardingAlgorithm --> Nokey, SingleKey ,MutipleKeys
tableShardingStratgey : TableShardingAlgorithm --> Single ,Mutiple

keyGenerator 
 
