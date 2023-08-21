## 单机版

```
docker run --restart=always -d --name namesrv -p 9876:9876 apache/rocketmq:4.9.7 sh mqnamesrv

docker run -d --name broker -p 10909:10909 -p 10911:10911  -e "NAMESRV_ADDR=192.168.0.252:9876" apache/rocketmq:4.9.7  sh mqbroker -n namesrv:9876
```
