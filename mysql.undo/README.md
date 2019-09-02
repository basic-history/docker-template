# mysql


直接参考下面的即可，测试通过


https://www.cnblogs.com/116970u/p/10869430.html

上面的这个没有设置数据挂载卷以及忽略大小写的配置，可参考下一列的配置更详细

https://blog.51cto.com/binuu/1943176

```shell
docker run -d -e MYSQL_ROOT_PASSWORD=123 --name mysql -v /data/mysql/my.cnf:/etc/mysql/my.cnf -v /data/mysql/data:/var/lib/mysql -p 3306:3306 mysql:5.7 --lower_case_table_names=1
```

这个直接运行还是有问题（似乎和配置文件不存在有关系），下面这个可以直接运行，记得先创建好库和用户

```shell
docker run -d -e MYSQL_ROOT_PASSWORD=123 --name mysql  -v /data/mysql/data:/var/lib/mysql -p 3306:3306 mysql:5.7 --lower_case_table_names=1
```