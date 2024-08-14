## Docker开机自启
```bash
systemctl enable docker
```

## 容器开机启动
```bash
docker update --restart=always [容器名/容器id]
```

## 拉取镜像
```bash
docker pull [容器名(:tag)]
```

## 查看本地镜像列表
```
docker images
```

## 删除本地镜像
```bash
docker rmi [容器名:tag/镜像id]
```

## 创建并运行容器
```bash
docker run [容器名(:tag)]
```

## 停止指定容器
```bash
docker stop [容器名/容器id]
```

## 启动指定容器
```bash
docker start [容器名/容器id]
```

## 重新启动容器
```bash
docker restart [容器名/容器id]
```

## 删除指定容器
```bash
docker rm [容器名/容器id]
```

## 查看容器列表
```bash
docker ps
```

## 查看容器运行日志
```bash
docker logs -f [容器名/容器id]
```

## 进入容器
```bash
docker exec -it [容器名/容器id] bash
```

## 保存镜像到本地压缩文件
```bash
docker save -o xxx.tar
```

## 加载本地压缩文件到镜像
```bash
docker load -i xxx.tar
```

## 查看容器详细信息
```bash
docker inspect [容器名/容器id]
```