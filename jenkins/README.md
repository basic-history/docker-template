# jenkins

[官网](https://jenkins.io/zh/doc/book/installing/#accessing-the-jenkins-blue-ocean-docker-container)

### 启动

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

*这个版本包含了blueocean，是一种新的pipeline模式，官网推荐下载*，也可以选择基础版本

```shell
docker run \
  --name jenkins49000 \
  -u root  \
  --rm \
  -d \
  -p 49000:8080\
  -p 50000:50000 \
  -v jenkins-data:/var/jenkins_home \
  -v /var/run/docker.sock:/var/run/docker.sock \
  jenkins
```

其中 ` -p 49000:8081` 代表您将通过端口`49000`访问`jekins`。

常规的经验是容器是不保存数据的，解耦数据和容器。

其中 `-v jenkins-data:/var/jenkins_home` 代表我们会创建一个数据卷，这个数据卷是一个特殊的目录，可以和容器内的目录映射。


结果是`/var/lib/docker/volumes/jenkins-data`目录和容器内`/var/jenkins_home`绑定在一起。

下载完成后理论上我们可以通过`curl localhost:49000`访问容器内的`jenkins`，可以试一下，如果能正确返回响应代表`jenkins`已经正常运作了，一般返回的内容是没有权限。

通过公网ip即可访问。

访问`jenkins`页面，提示我们`/var/jenkins_home/secrets/initialAdminPassword`寻找密码，我们需要进入到`/var/lib/docker/volumes/jenkins-data/_data/secrets`查看` cat initialAdminPassword `的内容得到密码。

按照向导安装即可，比较傻瓜，一般安装完`jenkins`后占用机器内存`300M`。

### 已知问题


####  设置完成后页面会提示重启，此时会发现容器消失，

原因是我们设置了 ` --rm `，官网的解释是：

> 关闭时自动删除Docker容器（下图为实例）。如果您需要退出Jenkins，这可以保持整洁。

这不会删除我们刚才配置的数据，即`/var/lib/docker/volumes/jenkins-data/_data`的文件不会被删除，所以不用担心刚才的配置无用，设置完成后手动重启容器即可。


##### 可否设置`jenkins`启动端口不为8080？


### 参考

https://cloud.tencent.com/developer/article/1351330

<https://jenkins.io/zh/doc/book/installing/#accessing-the-jenkins-blue-ocean-docker-container>

