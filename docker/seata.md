> **首先启动一个临时容器，将resources目录文件拷出**

```bash
docker run -d -p 8091:8091 -p 7091:7091 --name seata-serve seataio/seata-server:2.0.0-slim

docker cp seata-serve:/seata-server/resources /User/seata/config

docker rm seata-serve
```

> **修改application.yml配置文件，详细配置可以参考application.example.yml**

```yaml
seata:
  config:
    # support: nacos, consul, apollo, zk, etcd3
    type: file
  registry:
    # support: nacos, eureka, redis, zk, consul, etcd3, sofa
    type: nacos
    nacos:
      application: seata-server
      server-addr: nacos:8848
      group: "DEFAULT_GROUP"
      namespace: ""
      username: nacos
      password: nacos
  store:
    # support: file 、 db 、 redis
    mode: db
    session:
      mode: db
    lock:
      mode: db
    db:
      datasource: druid
      db-type: mysql
      driver-class-name: com.mysql.jdbc.Driver
      url: jdbc:mysql://mysql:3306/seata?rewriteBatchedStatements=true&serverTimezone=UTC
      user: root
      password: 123456
      min-conn: 10
      max-conn: 100
      global-table: global_table
      branch-table: branch_table
      lock-table: lock_table
      distributed-lock-table: distributed_lock
      query-limit: 1000
      max-wait: 5000
```

> **安装**

```bash
docker run -d \
  --name seata \
  -p 8091:8091 \
  -p 7091:7091 \
  -e SEATA_IP=192.168.217.128 \
  -v /User/seata/config:/seata-server/resources \
  --privileged=true \
  --network hm-net \
  seataio/seata-server:2.0.0-slim
```