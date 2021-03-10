# Docker-compose 搭建 ElasticSearch、Kibana

## [强力推荐 史上最全的 elasticsearch 教程](https://blog.csdn.net/agonie201218/category_9604962.html)

## 单机模式

### 使用`docker pull` 拉去镜像

```sh
docker pull docker.elastic.co/elasticsearch/elasticsearch:7.5.2
```

### 开始单 node 模式

- 1. 使用 docker

```sh
docker run --name=elasticsearch -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" docker.elastic.co/elasticsearch/elasticsearch:7.5.2
```

```sh
docker run --link elasticsearch:elasticsearch -p 5601:5601 kibana:7.5.2
```

- 2. 使用 docker-compose

```
version: '3.7'
networks:
  ES:

services:
  elasticsearch:
    container_name: elasticsearch
    image: docker.elastic.co/elasticsearch/elasticsearch:7.5.2
    ports:
      - 9200:9200
      - 9300:9300
    networks:
      - ES
    environment:
      - discovery.type=single-node
      - node.name=single-node-elastic
    volumes:
      - ./volumes/data:/usr/share/elasticsearch/data
      - ./volumes/config:/usr/share/elasticsearch/config

  kibana:
    container_name: kibana
    image: kibana:7.5.2
    ports:
      - 5601:5601
    networks:
      - ES
```

## 集群模式

> 当然 `volumes`等可以自行配置

```sh
version: '3.7'

volumes:
  data01:
    driver: local
  data02:
    driver: local
  data03:
    driver: local

networks:
  elastic:
    driver: bridge

services:
  es01:
    image: docker.elastic.co/elasticsearch/elasticsearch:7.5.2
    container_name: es01
    environment:
      - node.name=es01
      - cluster.name=es-docker-cluster
      - discovery.seed_hosts=es02,es03
      - cluster.initial_master_nodes=es01,es02,es03
      - bootstrap.memory_lock=true
      - 'ES_JAVA_OPTS=-Xms512m -Xmx512m'
    ulimits:
      memlock:
        soft: -1
        hard: -1
    volumes:
      - data01:/usr/share/elasticsearch/data
    ports:
      - 9200:9200
    networks:
      - elastic
  es02:
    image: docker.elastic.co/elasticsearch/elasticsearch:7.5.2
    container_name: es02
    environment:
      - node.name=es02
      - cluster.name=es-docker-cluster
      - discovery.seed_hosts=es01,es03
      - cluster.initial_master_nodes=es01,es02,es03
      - bootstrap.memory_lock=true
      - 'ES_JAVA_OPTS=-Xms512m -Xmx512m'
    ulimits:
      memlock:
        soft: -1
        hard: -1
    volumes:
      - data02:/usr/share/elasticsearch/data
    networks:
      - elastic
  es03:
    image: docker.elastic.co/elasticsearch/elasticsearch:7.5.2
    container_name: es03
    environment:
      - node.name=es03
      - cluster.name=es-docker-cluster
      - discovery.seed_hosts=es01,es02
      - cluster.initial_master_nodes=es01,es02,es03
      - bootstrap.memory_lock=true
      - 'ES_JAVA_OPTS=-Xms512m -Xmx512m'
    ulimits:
      memlock:
        soft: -1
        hard: -1
    volumes:
      - data03:/usr/share/elasticsearch/data
    networks:
      - elastic
  kibana:
    container_name: multi-kibana
    image: kibana:7.5.2
    volumes:
      - ./kibana.yml:/usr/share/kibana/config/kibana.yml
    environment:
      # SERVER_NAME: kibana.example.org
      ELASTICSEARCH_HOSTS: http://es01:9200
    ports:
      - 5601:5601
    networks:
      - elastic

```

## 错误集

### max virtual memory areas vm.max_count(65530) is too low

> set vm.max_map_count to at least 262144

根据平台的不同设置也不同

- Linux

> 修改配置文件

```sh
grep vm.max_map_count/etc/sysctl.conf
vm.max_map_count=262144
```

> 启动配置

```sh
sysctl -w vm.max_map_count=262144
```

- Mac

> 启动命令行执行

```sh
  screen ~/Library/Containers/com.docker.docker/Data/vms/0/tty
```

回车然后确认输入配置

```
sysctl -w vm.max_map_count=262144
```

- Windows and macOs with Docker Desktop
  通过 docker-machine 进行设置

```sh
docker-machine ssh
sudo sysctl -w vm.max_map_count=262144
```

- Windows with Docker Desktop WSL2 Backend

```sh
wsl -d docker-desktop
sysctl -w vm.max_map_count=262144
```
