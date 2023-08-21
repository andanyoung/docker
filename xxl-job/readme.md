```
docker run -d --name=xxl-job-admin  --restart=always -v /home/software/data/xxl/config:/config  -p 9995:8080 -v /home/software/data/xxl/log:/data/applogs/xxl-job/ -e PARAMS='--spring.config.location=/config/application.properties' xuxueli/xxl-job-admin:2.4.0
```
