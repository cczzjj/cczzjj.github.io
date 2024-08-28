## 常用语法

| 指令       | <div style="width:100px;">说明</div>      | 示例                        |
| ---------- | ---------------------------------------- | --------------------------- |
| FROM       | 指定基础镜像                              | FROM centos:6               |
| ENV        | 设置环境变量，可在后面指令使用              | ENV TZ=Asia/Shanghai        |
| COPY       | 拷贝本地文件到镜像的指定目录                | COPY ./xx.jar /tmp/app.jar |
| RUN        | 执行Linux的shell命令                      | RUN yum install gcc         |
| EXPOSE     | 指定容器运行时监听的端口，是给镜像使用者看的 | EXPOSE 8080                 |
| ENTRYPOINT | 镜像中应用的启动命令，容器运行时调用         | ENTRYPOINT java -jar xx.jar |

**例如，要基于Ubuntu镜像来构建一个Java应用，其Dockerfile内容如下：**
```bash
# 指定基础镜像
FROM ubuntu:16.04
# 配置环境变量，JDK的安装目录、容器内时区
ENV JAVA_DIR=/usr/local
ENV TZ=Asia/Shanghai
# 拷贝jdk和java项目的包
COPY ./jdk8.tar.gz $JAVA_DIR/
COPY ./docker-demo.jar /tmp/app.jar
# 设定时区
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime 
 && echo $TZ > /etc/timezone
# 安装JDK
RUN cd $JAVA_DIR \
 && tar -xf ./jdk8.tar.gz \
 && mv ./jdk1.8.0_144 ./java8
# 配置环境变量
ENV JAVA_HOME=$JAVA_DIR/java8
ENV PATH=$PATH:$JAVA_HOME/bin
# 指定项目监听的端口
EXPOSE 8080
# 入口，java项目的启动命令
ENTRYPOINT ["java", "-jar", "/app.jar"]
```

**使用基础的系统加JDK环境镜像，省去JDK的配置：**
```bash
# 基础镜像
FROM openjdk:11.0-jre-buster
# 设定时区
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime 
  && echo $TZ > /etc/timezone
# 拷贝jar包
COPY docker-demo.jar /app.jar
# 入口
ENTRYPOINT ["java", "-jar", "/app.jar"]
```

## 执行命令

```bash
# 进入镜像目录
cd /root/demo
# 开始构建
docker build -t docker-demo:1.0 .
```

**命令说明：**

- `docker build`：构建一个docker镜像
- `-t docker-demo:1.0`：`-t`参数是指定镜像的名称（`image`和`tag`）
- `.`：构建时Dockerfile所在路径，也可以指定其他目录
```bash
# 直接指定Dockerfile目录
docker build -t docker-demo:1.0 /root/demo
```
