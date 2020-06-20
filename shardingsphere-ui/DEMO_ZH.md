# 使用示例


## 0. 安装前端依赖

安装nodejs

```shell script
npm i -g cnpm

cnpm i -g puppeteer

```


## 1. 下载与编译项目：


参考 ShardingSphere-UI 使用手册：

https://shardingsphere.apache.org/document/current/cn/user-manual/shardingsphere-ui/usage/



## 2. 启动 Docker

如果没有现成的 zookeeper 或者 etcd, 可以使用 shardingsphere-jdbc 的Docker配置：

https://github.com/apache/shardingsphere/blob/master/examples/docker/docker-compose-zh.md

执行脚本为:

```
cd examples/docker/sharding-jdbc/sharding/

docker-compose up -d

```

## 3. 启动项目

参考:

https://shardingsphere.apache.org/document/current/cn/user-manual/shardingsphere-ui/usage/build/


## 4. 使用示例

访问 http://localhost:8088/

#  执行登录

按照 Apache 项目的标准，配置文件在部署路径的 conf 目录下。

如果用户名和密码的有修改则去里面查找。


### 4.1 添加配置中心

示例数据：

```
配置中心名称： 请自己命名, 如 etcd-test
配置中心类型:  选择 Zookeeper/Etcd
配置中心地址（Zookeeper）： localhost:2181
配置中心地址（Etcd）：  localhost:2379,localhost:2380
数据治理实例：  根据服务治理进行命名，比如 ss-jdbc-etcd-test
命名空间：     根据服务治理进行命名，比如 test
登录凭证：     配置中心服务器的密码， 如果有的话
```


### 4.2 激活配置中心

添加管理后，可以点击列表项中对应的 "激活"按钮，进行激活。

后续的配置管理操作，都是基于已激活的配置中心进行的。

### 4.2 配置管理




# 添加Schema


名称

```
自己指定
```

分片配置规则:

```
rules:
- !!org.apache.shardingsphere.sharding.yaml.config.YamlShardingRuleConfiguration
  tables:
    t_order:
      logicTable: t_order
      actualDataNodes: ds${0..1}.t_order${0..1}
      databaseStrategy:
        standard:
          shardingColumn: user_id
          shardingAlgorithm:
            type: PreciseShardingAlgorithm
            props:
              algorithmExpression: ds${user_id % 2}
      tableStrategy:
        standard:
          shardingColumn: order_id
          shardingAlgorithm:
            type: PreciseShardingAlgorithm
            props:
              algorithmExpression: t_order${order_id % 2}

```

数据源配置规则:

```
dataSources:
  ds0:
    driverClassName: com.mysql.jdbc.Driver
    url: jdbc:mysql://localhost:3306/ds0
    username: root
    password:
  ds1:
    driverClassName: com.mysql.jdbc.Driver
    url: jdbc:mysql://localhost:3306/ds1
    username: root
    password:
```
