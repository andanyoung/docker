# redis

## 下载镜像：

`docker pull redis:6.2`

## 运行：

```
docker run --name some-redis -d redis
```

## 使用持久化模式运行

```
docker run --name some-redis -d redis redis-server --appendonly yes
```

## 使用`redis-cli`链接

```
docker run -it --network some-network --rm redis redis-cli -h some-redis
```

## 自定义配置文件

`D:/workspace/docker/github-docker/redis/volumes` 为配置文件和数据目录，配置文件需要提前准备好

```
docker run -v D:/workspace/docker/github-docker/redis/volumes:/data -p 6379:6379  --name redis -d redis:6.2 redis-server /data/config/redis.conf
```

## 主从架构集群模式

> 提前准备好配置文件放于`D:/workspace/docker/github-docker/redis/sentinel/redis-6379/config`中

### 主

```
docker run -v D:/workspace/docker/github-docker/redis/sentinel/redis-6379/config:/usr/local/etc/redis -p 6379:6379 --name redis-6379 -d redis:6.2 redis-server /usr/local/etc/redis/redis.conf
```

### 从 6380

```
slaveof 172.17.0.2 6379
masterauth 123456
slave-read-only yes
slave-serve-stale-data yes
min-slaves-to-write 0
min-slaves-max-lag 10
```

> 注意如果是单个宿主机部署的话 这里的主机 IP，是 docker 的 IP 即使在统一碳主机里面不能写 127.0.0.1，因为网络存在隔离。`127.0.0.1`是自己容器本身的 IP
> `172.17.0.2` redis 主的容器 IP 也可以填写宿主机的 IP

```
docker run -v D:/workspace/docker/github-docker/redis/sentinel/redis-6380/config:/usr/local/etc/redis -p 6380:6379 --name redis-6380 -d redis:6.2 redis-server /usr/local/etc/redis/redis.conf
```

### 从 6381

```
slaveof 172.17.0.2 6379
masterauth 123456
slave-read-only yes
slave-serve-stale-data yes
min-slaves-to-write 0
min-slaves-max-lag 10
```

```
docker run -v D:/workspace/docker/github-docker/redis/sentinel/redis-6381/config:/usr/local/etc/redis -p 6381:6379 --name redis-6381 -d redis:6.2 redis-server /usr/local/etc/redis/redis.conf
```

### 问题

- `Failed trying to load the MASTER synchronization DB from disk`
  在 windows 中不能将`/data` 目录挂载出来，不然会有权限问题
