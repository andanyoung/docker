# mysql
> [https://hub.docker.com/_/mysql](https://hub.docker.com/_/mysql)

```
docker pull mysql
#docker run --name some-mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql:tag
docker run --name mysql5.7 -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 -d mysql:5.7
```