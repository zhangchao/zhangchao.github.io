---
title: 005_Coding
date: 2017-08-16 15:44:55
tags: ConnectionPool
---


# Connection Pool 

MySQL、Oracle 的 Connection 实现是线程安全的。

数据库连接池实现的 Connection 不一定是线程安全，例如 Druid 的线程池 Connection 非线程安全

java.sql.Connection是线程安全的，应用程序可以并发的使用同一个Connection来访问数据库

问题是，数据库的驱动可能会是并发的处理请求，也可能是串行的处理，这取决于数据库厂商。同时也说了，实际开发中最好还是单线程方式使用。

Oracle的JDBC API都是synchronized的，因此，对数据库的操作肯定是串行的。
MySQL的Connection也是串行的执行数据库操作的。

这个就是说，用1个连接可能会出现长时间不用试导致连接被回收的情况。一般连接的最大不活动时间在30分钟到8个小时之间，取决于数据库服务器的配置。

在Mysql的默认设置中，如果一个数据库连接超过8小时没有使用（闲置8小时，即28800s），mysql server将主动断开这条连接，后续在该连接上进行的查询操作都将失败，
将出现：error 2006 (MySQL server has gone away)!。