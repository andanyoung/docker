# mysql
> [https://hub.docker.com/_/mysql](https://hub.docker.com/_/mysql)

```
docker pull mysql
#docker run --name some-mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql:tag
docker run --name mysql5.7 -p 3306:3306  -v mysql57:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 -d mysql:5.7
```
```
docker run -p 3306:3306 --name mysql --restart=always --privileged=true \
-v /home/software/data/mysql/log:/var/log/mysql \
-v /home/software/data/mysql/data:/var/lib/mysql \
-v /home/software/data/mysql/conf:/etc/mysql/conf.d  \
-v /etc/localtime:/etc/localtime:ro \
-e MYSQL_ROOT_PASSWORD=TNjddIq123 -d mysql:5.7
```

## windows
> 错误
> `bind: An attempt was made to access a socket in a way forbidden by its access permissions.`

> 最近除了自动更新了一下windows 10 64补丁，没啥特别操作，猜测是最近的更新增强了端口上的安全策略。
>
>控制面板- 防火墙-高级设置-出站规则-新建-端口
>增加udp和tcp的所有端口，允许连接。
>重启电脑，解决
