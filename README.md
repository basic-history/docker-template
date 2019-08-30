# docker-template

本仓库主要保存使用`docker`时自定义的一些模版以及使用说明，方便在创建环境时参考使用。


一些基础的知识可以参考手册  https://yeasy.gitbooks.io/docker_practice/content/install

### 常用命令


```shell
#列出已有镜像的信息
docker images
docker images -a --列出所有，包含临时文件
```

```shell
#拉取镜像
docker pull NAME[:TAG]
docker pull centos        【如果不指定TAG那么默认是拉取最新版本】
docker pull ubuntu:18.04
```

```shell
#添加TAG
docker tag ubuntu:latest myubuntu:latest
【之后使用docker images 会发现多了一个刚加了标签的镜像，但Id是一样的，实际上标签就是起了个链接的作用】
```

```shell
#查看镜像详细信息
docker inspect ubuntu:18.04
```

```shell
#查看镜像历史
docker history ubuntu:18.04 
【如果显示内容过长被截断了，可以使用 --no-trunc输出完整命令】
```

```shell
#查找镜像
docker search [option] keyword
docker search --filter=is-official=true nginx 【查找官方带nginx关键字的镜像】
docker search -f=stars=4 tensorflow 【查找收藏超过4的tensorflow镜像】
```

```shell
#删除镜像
docker rmi myubuntu:latest 【使用标签删除，注意：当这个标签对应的镜像只有一个时真的会被删除】
docker rmi myubuntu:latest -f 【强制删除，即使有容器依赖它，一般都是先删除容器再删除镜像】
docker rmi a21c0839edeae 【根据id删除镜像】
```

```shell
#列出本机存在的所有容器
docker ps -a
```

```shell
#删除容器
docker rmi a21c0839edeae 【根据id删除容器】
docker rm b098c66ef455 9e995983f718 b88bafdf8715 【批量删除】
```

```shell
#清理镜像
docker image prune -f 【清理临时镜像并且不进行确认提示】
```

```shell
#创建镜像
//TODO
```

