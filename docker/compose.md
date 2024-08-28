`Docker Compose`是一个用于定义和运行多个`Docker`容器的工具。它使用`YAML`文件来配置应用程序的服务，并允许您通过一个命令来启动、停止和重启应用中的所有服务。

`Docker Compose`的主要作用是简化容器的管理和部署。它使得多个容器能够以`正确的顺序`和`依赖关系`启动，并确保它们在运行时可以相互通信。这使得开发人员可以更容易地处理复杂的`Docker`环境，尤其是在需要多个容器协同工作的场景下。

## 常用指令

docker-compose文件中可以定义多个相互关联的应用容器，每一个应用容器被称为一个服务（service）。由于service就是在定义某个应用的运行时参数，因此与docker run参数非常相似。

**举例来说，用`docker run`部署MySQL的命令如下：**
```bash
docker run -d \
  --name mysql \
  -p 3306:3306 \
  -e TZ=Asia/Shanghai \
  -e MYSQL_ROOT_PASSWORD=123 \
  -v ./mysql/data:/var/lib/mysql \
  -v ./mysql/conf:/etc/mysql/conf.d \
  -v ./mysql/init:/docker-entrypoint-initdb.d \
  --restart=always
  --network hmall
  mysql
```

**如果用`docker-compose.yml`文件来定义，就是这样：**
```yaml
version: "3"
services:
  mysql:
    image: mysql
    container_name: mysql
    ports:
      - "3306:3306"
    environment:
      TZ: Asia/Shanghai
      MYSQL_ROOT_PASSWORD: 123
    volumes:
      - "./mysql/data:/var/lib/mysql"
      - "./mysql/conf:/etc/mysql/conf.d"
      - "./mysql/init:/docker-entrypoint-initdb.d"
    restart: always
    networks:
      - hmall
networks:
  hmall:
```

**对比如下：**

| docker run 参数  | docker compose 指令 | 说明         |
| ---------------- | ------------------ | ------------ |
| --name           | container_name     | 容器名称      |
| -p               | ports              | 端口映射      |
| -e               | environment        | 环境变量      |
| -v               | volumes            | 数据卷配置    |
| --network        | networks           | 网络         |

> 黑马商城部署文件：

```yaml
version: "3"
services:
  mysql:
    image: mysql
    container_name: mysql
    ports:
      - "3306:3306"
    environment:
      TZ: Asia/Shanghai
      MYSQL_ROOT_PASSWORD: 123
    volumes:
      - "./mysql/conf:/etc/mysql/conf.d"
      - "./mysql/data:/var/lib/mysql"
      - "./mysql/init:/docker-entrypoint-initdb.d"
    networks:
      - hm-net
  hmall:
    build: 
      context: .
      dockerfile: Dockerfile
    container_name: hmall
    ports:
      - "8080:8080"
    networks:
      - hm-net
    depends_on:
      - mysql
  nginx:
    image: nginx
    container_name: nginx
    ports:
      - "18080:18080"
      - "18081:18081"
    volumes:
      - "./nginx/nginx.conf:/etc/nginx/nginx.conf"
      - "./nginx/html:/usr/share/nginx/html"
    depends_on:
      - hmall
    networks:
      - hm-net
networks:
  hm-net:
    driver: bridge
```

## 执行命令

> **（重新）构建服务**
```bash
docker compose build [service]
```

> **验证compose文件格式**
```bash
doker compose config
```

> **构建、（重新）创建、启动并附加到服务的容器**
```bash
docker compose up
```
- `-d`：在后台运行服务容器。
- `--force-recreate`：强制重新创建容器，不能与`--no-recreate`同时使用
- `--no-recreate`：如果容器已经存在了，则不重新创建，不能与`--force-recreate`同时使用。

> **列出所有服务容器**
```bash
docker compose ps
```

> **停止并删除服务容器，移除网络**
```bash
docker compose down
```

> **启动已存在的服务容器**
```bash
docker compose start [service]
```

> **停止服务容器**
```bash
docker compose stop [service]
```

> **重启服务容器**
```bash
docker compose restart [service]
```

> **删除已停止的服务容器**
```bash
docker compose rm [service]
```