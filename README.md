# docker-template

本仓库主要保存使用`docker`时自定义的一些模版以及使用说明，方便在创建环境时参考。




### 常用命令


```shell
#列出已有镜像的信息
docker images
docker images -a 【列出所有，包含临时文件】
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
docker rm a21c0839edeae 【根据id删除容器】
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


```shell
#开启服务

```

```shell
#查看容器日志
docker logs <docker-container-name>
docker logs jenkins
```

```shell
#查看端口映射
docker port 9f93aee2f592
```

```shell
#创建数据卷
docker volume create -d local test 【查看/var/lib/docker/volumn路径下已经有test文件夹了，注意：还会多生成一层_data目录】
```

### 端口映射/文件挂载/容器互联

#### 1. 从外部访问容器应用

在启动容器时，如果不指定对应参数，在容器外部是无法访问容器内的网络应用的。所以一般使用`-p`或者`-P`来进行端口映射。当使用`-P`会随机映射一个49000-49900的端口供外部访问容器内部。

以下以`jenkins`举例：


```shell
docker run \
  -u root \
  --rm \
  -d \
  -p 49000:8080\
  -p 50000:50000 \
  -v jenkins-data:/var/jenkins_home \
  -v /var/run/docker.sock:/var/run/docker.sock \
  jenkinsci/blueocean
```


其中 ` -p 49000:8081` 代表您将通过端口`49000`访问`jekins`。

常规的经验是容器是不保存数据的，解耦数据和容器。

其中 `-v jenkins-data:/var/jenkins_home` 代表我们会创建一个数据卷，这个数据卷是一个特殊的目录，可以和容器内的目录映射。


结果是`/var/lib/docker/volumes/jenkins-data`目录和容器内`/var/jenkins_home`绑定在一起。

访问`jenkins`页面，提示我们`/var/jenkins_home/secrets/initialAdminPassword`寻找密码，我们需要进入到`/var/lib/docker/volumes/jenkins-data/_data/secrets`查看` cat initialAdminPassword `的内容得到密码。


下载完成后理论上我们可以通过`curl localhost:49000`访问容器内的`jenkins`，可以试一下，如果能正确返回响应代表`jenkins`已经正常运作了，一般返回的内容是没有权限。

通过公网ip即可访问。


### 加速器

```shell
 vi /etc/docker/daemon.json
```

配置为以下内容即可，当然也可以增加其他的源。

```shell
{
  "registry-mirrors": [
    "https://dockerhub.azk8s.cn",
    "https://reg-mirror.qiniu.com"
  ]
}
```
### 参考

https://yeasy.gitbooks.io/docker_practice/content/install
https://jenkins.io/zh/doc/book/installing/

