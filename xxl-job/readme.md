```
docker run -d --name=xxl-job-admin  --restart=always -v /home/software/data/xxl/config:/config  -p 9995:8080 -v /home/software/data/xxl/log:/data/applogs/xxl-job/ -e PARAMS='--spring.config.location=/config/application.properties' xuxueli/xxl-job-admin:2.4.0
```

--net=host 使用主机模式避免 xxl-job-admin访问不了执行器ip的问题
```
触发调度：
address：http://192.168.1.13:10000/
code：500
msg：xxl-job remoting error(connect timed out), for url : http://192.168.1.13:10000/run
```
