## Docker服务

> **配置镜像加速**
```bash
# 创建目录
mkdir -p /etc/docker

# 复制内容，注意把其中的镜像加速地址改成你自己的
tee /etc/docker/daemon.json <<-'EOF'
{
    "registry-mirrors": [
        "http://hub-mirror.c.163.com",
        "https://mirrors.tuna.tsinghua.edu.cn",
        "http://mirrors.sohu.com",
        "https://ustc-edu-cn.mirror.aliyuncs.com",
        "https://ccr.ccs.tencentyun.com",
        "https://docker.m.daocloud.io",
        "https://docker.awsl9527.cn"
    ]
}
EOF

# 重新加载配置
systemctl daemon-reload

# 重启Docker
systemctl restart docker
```

> **开机自启**
```bash
systemctl enable docker
```

> **查看版本**
```bash
docker version
```

## 镜像操作

> **拉取镜像**
```bash
docker pull [容器名(:tag)]
```

> **查看本地镜像列表**
```bash
docker images
```

> **删除本地镜像**
```bash
docker rmi [镜像名:tag/镜像id]
```

> **保存镜像到本地压缩文件**
```bash
docker save -o xxx.tar [镜像名/镜像id]
```

> **加载本地压缩文件到镜像**
```bash
docker load -i xxx.tar
```

## 容器操作

> **容器开机自启**
```bash
docker update --restart=always [容器名/容器id]
```

> **创建并运行容器**
```bash
docker run --name [容器名] [镜像名(:tag)]
```

> **查看容器列表**
```bash
docker ps
```

> **停止指定容器**
```bash
docker stop [容器名/容器id]
```

> **启动指定容器**
```bash
docker start [容器名/容器id]
```

> **重新启动容器**
```bash
docker restart [容器名/容器id]
```

> **删除指定容器**
```bash
docker rm [容器名/容器id]
```

> **查看容器运行日志**
```bash
docker logs -f [容器名/容器id]
```

> **查看容器详细信息**
```bash
docker inspect [容器名/容器id]
```

> **进入容器**
```bash
docker exec -it [容器名/容器id] bash
```

## 数据卷操作

> **列出所有卷**
```bash
docker volume ls
```

> **查看卷的详细信息**
```bash
docker volume inspect [卷名]
```

> **创建新卷**
```bash
docker volume create [卷名]
```

> **删除一个或多个卷**
```bash
docker volume rm [卷名] ([卷名2])
```

> **删除未使用的卷**
```bash
docker volume prune
```

## 网络操作

> **列出所有网络**
```bash
docker network ls
```

> **查看网络详细信息**
```bash
docker network inspect [网络名/网络id]
```

> **创建新网络**
```bash
docker network create [网络名]
```

> **删除一个或多个网络**
```bash
docker network rm [网络名/网路id] ([网络名2/网络id2])
```

> **将容器连接到网络**
```bash
docker network connect [网络名/网络id] [容器名/容器id]
```

> **将容器从网络断开**
```bash
docker network disconnect [网络名/网络id] [容器名/容器id]
```