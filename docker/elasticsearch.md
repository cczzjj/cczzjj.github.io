## Elasticsearch安装命令
```bash
docker run -d \
  --name es \
  -e "ES_JAVA_OPTS=-Xms512m -Xmx512m" \
  -e "discovery.type=single-node" \
  -v es-data:/usr/share/elasticsearch/data \
  -v es-plugins:/usr/share/elasticsearch/plugins \
  --privileged \
  --network hm-net \
  -p 9200:9200 \
  -p 9300:9300 \
  elasticsearch:7.17.23
```


## Kibana安装命令
```bash
docker run -d \
  --name kibana \
  -e ELASTICSEARCH_HOSTS=http://es:9200 \
  --network=hm-net \
  -p 5601:5601  \
  kibana:7.17.23
```


## IK分词器

> **包含两种analyzer**
- `ik_smart`：智能语义切分
- `ik_max_word`：最细粒度切分

> **在线安装**
```bash
docker exec -it es \
  ./bin/elasticsearch-plugin install \
  https://release.infinilabs.com/analysis-ik/stable/elasticsearch-analysis-ik-7.17.23.zip
```

然后重启es容器

> **离线安装**

查看Elasticsearch容器的plugins数据卷目录：
```bash
docker volume inspect es-plugins
```

结果如下：
```json
[
    {
        "CreatedAt": "2024-11-06T10:06:34+08:00",
        "Driver": "local",
        "Labels": null,
        "Mountpoint": "/var/lib/docker/volumes/es-plugins/_data",
        "Name": "es-plugins",
        "Options": null,
        "Scope": "local"
    }
]
```

把IK分词器解压后的目录上传到`/var/lib/docker/volumes/es-plugins/_data`，然后重启es容器

> **拓展词典**

修改IK分词器config目录中的`IKAnalyzer.cfg.xml`配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE properties SYSTEM "http://java.sun.com/dtd/properties.dtd">
<properties>
        <comment>IK Analyzer 扩展配置</comment>
        <!--用户可以在这里配置自己的扩展字典 *** 添加扩展词典-->
        <entry key="ext_dict">ext.dic</entry>
</properties>
```

在config目录新建一个`ext.dic`文件

```
传智播客
泰裤辣
...
```

最后重启容器