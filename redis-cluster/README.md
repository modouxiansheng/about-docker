# Mac上最简单明了的利用Docker搭建Redis集群

本文只是记录一下我在Mac上利用Docker搭建Redis集群成功后的步骤，期间走了许多的坑。有许多教程对于Mac用户不友好，搭建成功以后无法进行集群间的通信。

首先说明一下有多简单，如果你机器上已经有了Docker，那么就一个步骤就行。如果没有Docker那么在[Docker下载](https://docs.docker.com/docker-for-mac/install/)下载一个就行。

接下来我们就开始进行Redis集群的搭建。

首先先从[github](https://github.com/modouxiansheng/mac-docker-redis-cluster)。例如我的存放目录结构是如下

```
/Users/hupengfei/myDocker/newRedis-cluster

-rwxr-xr-x@ 1 hupengfei  staff   1.6K  4 19 14:01 Dockerfile
-rwxr-xr-x@ 1 hupengfei  staff   1.0K  3 31 14:28 LICENSE
-rwxr-xr-x@ 1 hupengfei  staff   8.8K  3 31 14:28 Makefile
-rwxr-xr-x@ 1 hupengfei  staff   4.8K  3 31 14:28 README.md
-rwxr-xr-x@ 1 hupengfei  staff   341B  4 22 11:43 docker-compose.yml
-rwxr-xr-x@ 1 hupengfei  staff   2.2K  4 19 13:55 docker-entrypoint.sh
-rwxr-xr-x@ 1 hupengfei  staff   506B  3 31 14:28 generate-supervisor-conf.sh
-rwxr-xr-x@ 1 hupengfei  staff   167B  4 19 16:47 redis-cluster.tmpl
-rwxr-xr-x@ 1 hupengfei  staff    65B  3 31 14:28 redis.tmpl
-rwxr-xr-x@ 1 hupengfei  staff   219B  3 31 14:28 sentinel.tmpl
```

那么就在`newRedis-cluster`此目录下执行命令`docker-compose build`，然后执行`docker-compose up`命令，你就会发现如下输出

```
 2019 03:50:25.179 * Background AOF rewrite finished successfully
```

就代表已经启动成功了，此时输入`redis-cli -c -p 7001`，进入其中一个节点，你就会发现已经启动成功了，在节点中输入`cluster nodes`可以查看到集群的信息。

此时可以进行测试一下，在里面存放不同的值，他会跳转到端口上面进行存放

```
127.0.0.1:7001> set hu1 1
OK
127.0.0.1:7001> set hu2 2
-> Redirected to slot [4983] located at 127.0.0.1:7000
OK
127.0.0.1:7000> set hu3 3
OK
127.0.0.1:7000> set hu4 4
-> Redirected to slot [13233] located at 127.0.0.1:7002
OK

```

此时集群就搭建成功了。此Git项目是参考[https://github.com/Grokzen/docker-redis-cluster](https://github.com/Grokzen/docker-redis-cluster)这个项目，只是根据Issues进行修改了一些东西使Mac用户能够更方便使用。

如果你想修改集群中redis的配置信息，可以修改里面的`redis-cluster.tmpl`文件。修改完以后用`docker-compose build`完以后再启动就好了。
