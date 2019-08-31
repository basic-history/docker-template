### 容器


其中有几个比较重要的参数

`-t` 代表开启伪终端
`-i` 代表保持标准输入打开，默认不打开
`-d` 代表以后台形式运行，也就是说加了这个参数直接就返回了


#### 前台

`docker run -it --name centos1 centos /bin/bash` 代表以前台运行启动容器并进入内部交互环境

`exit`会退出并关闭容器，使用`ctrl+p+q`可以退出不关闭容器。

使用`docker container prune -f`删除已关闭被占用的容器，方便下面的测试`

####  后台

`docker run -itd --name centos1 centos /bin/bash`

`docker exec -it centos1 /bin/bash`

`docker exec -it cfb2b8699b74385c264c6dd04600093740f5d2a13055329c41e5850a1c3c17c5  /bin/bash`

都可以进入容器，注意：这里`exit`会退出但不会关闭容器，因为它是后台运行的。

推荐使用`-it`打开伪终端，再通过`exec`进入容器。

### 端口映射/文件挂载/容器互联

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

其中`--rm`代表容器停止后，容器会自动删除。

其中 ` -p 49000:8081` 代表您将通过端口`49000`访问`jekins`。

常规的经验是容器是不保存数据的，解耦数据和容器。

其中 `-v jenkins-data:/var/jenkins_home` 代表我们会创建一个数据卷，这个数据卷是一个特殊的目录，可以和容器内的目录映射。


结果是`/var/lib/docker/volumes/jenkins-data`目录和容器内`/var/jenkins_home`绑定在一起。

访问`jenkins`页面，提示我们`/var/jenkins_home/secrets/initialAdminPassword`寻找密码，我们需要进入到`/var/lib/docker/volumes/jenkins-data/_data/secrets`查看` cat initialAdminPassword `的内容得到密码。


下载完成后理论上我们可以通过`curl localhost:49000`访问容器内的`jenkins`，可以试一下，如果能正确返回响应代表`jenkins`已经正常运作了，一般返回的内容是没有权限。

通过公网ip即可访问。


容器互联：

当我们一个容器需要访问到另外一个容器时，典型的场景一个WEB应用需要访问容器数据库，可以使用容器名，而不用使用具体的IP。

需要先给容器启动时指定自定义的名称


web：

`docker run -d -P --name web training/webapp python app.py`

其中`training/webapp`是镜像名称。

db：

`docker run -d --name db traing/postgres`

开始连接：

`docker run -d -P --name web --link db:db training/webapp python app.py`


其中`--link name:alias`，`name`是要链接的容器的名称。

这样的好处是即使重启生成新的容器id，也无需重新配置，因为是和名字关联的。另外，可以让容器可以访问数据库容器，避免了数据库容器可以通过外部端口访问的安全问题。

### 使用Dockerfile创建容器

TODO