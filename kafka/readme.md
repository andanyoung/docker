# 单机版

```
docker run -d --name kafka -p 9092:9092 --link zookeeper -e KAFKA_ZOOKEEPER_CONNECT=192.168.1.152:2181 -e KAFKA_ADVERTISED_HOST_NAME=192.168.1.152 -e KAFKA_ADVERTISED_LISTENERS=PLAINTEXT://192.168.1.152:9092 -e KAFKA_LISTENERS=PLAINTEXT://0 wurstmeister/kafka:3.5
```

注意：请将 IP 地址更改为实际使用的 IP 地址。
