---
title: 001_Coding
date: 2017-08-02 14:59:05
tags: jdbc-sharding
---

# 学习资料 
 http://www.cnblogs.com/codestory/p/5591651.html

# 项目的演化

1. 对数据表索引进行重新整理。同时也对数据库本身的参数设置进行了优化
2. 进行WEB多点部署，前端使用nginx做负载分发，如何解决用户会话保持的问题
  2.1 nginx缺省没有sticky机制，可以使用ip_hash方式来代替
  2.2 配置Tomcat实现Session复制
  2.3 代码使用SpringSession，利用redis实现session复制
3. SQL执行时间越来越长，而且无法优化。原因也很简单，数据量太大。单表数据已经超过几千万行， 需要分库分表

# 数据库分表

https://github.com/dangdangdotcom/sharding-jdbc

operate_history ---> operate_history_0operate_history_1 operate_history_2 operate_history_3 , 用外键作为拆分

```xml 
<bean id="db-node-0"class="org.apache.tomcat.jdbc.pool.DataSource"destroy-method="close">
 <property name="driverClassName"value="com.mysql.jdbc.Driver"></property>
 <property name="url"value="jdbc:mysql://localhost:3306/sharding"></property>
 <property name="username"value="root"></property>
 <property name="password"value="sharding"></property>
</bean>

<rdb:strategyid="historyTableStrategy" sharding-columns="entity_key" algorithm-class="cn.codestory.sharding.SingleKeyTableShardingAlgorithm"/>

<rdb:data-sourceid="dataSource">
 <rdb:sharding-ruledata-sources="db-node-0" default-data-source="db-node-0">
  <rdb:table-rules>
   <rdb:table-rulelogic-table="operate_history" actual-tables="operate_history_0,operate_history_1,operate_history_2,operate_history_3" table-strategy="historyTableStrategy" />
  </rdb:table-rules>
 </rdb:sharding-rule>
</rdb:data-source>
```

对entity_key进行求模，用余数确定数据表名。


## 如何解决自增长主键
不能使用identify方式生成数据表主键。如果主键是String类型，可以考虑使用uuid生成方法，但它查询效率会相对比较低。如果使用long型主键，可以使用其他方式，一定要确保各个子表中的主键不重复

## 历史数据
对原有数据包的数据进行迁移，分别移动到四个数据表中。如果不做这一步，或者数据迁移到了错误的数据表，后续将会查询不到这些数据。

# 分表规则

* 根据主键进行分配
* 根据用户ID进行分配
* 根据某一个外键的值进行分配
* 根据时间进行分配


# 单点登录方案

使用一个用户信息管理的主系统，其他系统不再保留或者同步这套系统的用户数据

## 如何同步
1. 当用户信息变更后，主系统主动向所有子系统推送用户变更信息
这种方案的缺点：1、更新时某个子系统正在存在故障，更新失败，以后可能找不到更新的时机了；2、某个子系统部署位置特殊，不能被主系统访问

2. 子系统定期主动发起同步请求。主系统将某个时间段之后的所有变更，按照特定格式封装后返回给子系统，由子系统负责解析并更新到本地用户表

3. 用户登录后，跳转到子系统时，从主系统中查询当前用户的信息，并同子系统进行比较，如有变更则更新本地用户信息。

这种方案的缺点：1、如果用户不访问子系统，永远没有更新的机会；2、用户被删除或禁用，没有机会更新子系统。

## 单点登录的演化

1. 用户在主系统中登录后，创建一个cookie，域名设置为xyz.com。这时候，*.xyz.com的所有服务器，都能读取到这个域名，子系统读取到userId这个cookie后，知道这是登录用户的id，从数据库中读取对应用户的信息并保存到Session中即可
2. 用户在主系统中登录后，在主系统中提供子系统链接，并将用户说的唯一标识通过参数传递给子系统  web1.xyz.com?userId=admin
3. 如果觉得传输用户的userId明文不够安全，可以考虑将参数值加密进行传输。
4. 如果用户在主系统以用户 admin 登录，在主系统中生成一个token，保存在session中，并在所有的子系统链接中增加token参数 (子系统拿到token=t12，不认识，它去问主系统：t12是谁？)
5. 每次构造子系统链接时，生成一个新的token，当子系统验证用户成功后，立刻抛弃这个token，后续用户就不能再使用这个token进行验证




成熟的方案，大致有：CAS、OAuth1.0/2.0、OpenID，以及QQ等第三方提供的单点登录接入方案