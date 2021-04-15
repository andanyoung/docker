### 这里选择的是可以访问web管理界面的tag
```
docker pull rabbitmq：management
```
### 创建容器并运行（15672是管理界面的端口，5672是服务的端口。这里顺便将管理系统的用户名和密码设置为admin admin）
```
docker run -dit --name rabbitmq -e RABBITMQ_DEFAULT_USER=admin -e RABBITMQ_DEFAULT_PASS=admin -p 15672:15672 -p 5672:5672 rabbitmq:management
```