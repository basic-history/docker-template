# jenkins

[仓库官网](https://hub.docker.com/_/mysql?tab=tags)

### 启动

```shell
docker run --name mysql -e MYSQL_ROOT_PASSWORD=123 -d mysql:5.7.27
```
开放远程访问权限

```shell
docker exec -it mysql bash
ALTER USER 'root'@'localhost' IDENTIFIED BY '123';
#添加远程登录用户
CREATE USER 'pleuvoir'@'%' IDENTIFIED WITH mysql_native_password BY '123';
GRANT ALL PRIVILEGES ON *.* TO 'pleuvoir'@'%';
```

### 参考

https://www.runoob.com/docker/docker-install-mysql.html

https://my.oschina.net/damienchen/blog/1931305

https://hub.docker.com/r/cytopia/mysql-5.7/