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
