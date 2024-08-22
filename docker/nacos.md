## 端口

> Nacos2.0版本相比1.X新增了gRPC的通信方式，因此需要增加2个端口。新增端口是在配置的主端口(server.port)基础上，进行一定偏移量自动生成。

| 端口        | 与主端口的偏移量 | 描述                                                  |
| ----------- | -----------    | ----------------------------------------------------  |
| 9848        | 1000           | 客户端gRPC请求服务端端口，用于客户端向服务端发起连接和请求 |
| 9849        | 1001           | 服务端gRPC请求服务端端口，用于服务间同步等                |
| 7848        | -1000          | Jraft请求服务端端口，用于处理服务端间的Raft相关请求       |


## 安装命令

```bash
docker run -d \
  --name nacos \
  -e SPRING_DATASOURCE_PLATFORM=mysql
  -e MYSQL_SERVICE_HOST=mysql
  -e MYSQL_SERVICE_DB_NAME=nacos
  -e MYSQL_SERVICE_USER=root
  -e MYSQL_SERVICE_PASSWORD=123456
  -p 8848:8848 \
  -p 9848:9848 \
  -p 9849:9849 \
  --restart=always \
  nacos/nacos-server:v2.4.1-slim
```