# Apache Dubbo Learning

### 一、安装手册

#### 1.1.Zookeeper 注册中心安装

建议使用 `dubbo-2.3.3` 以上版本的 zookeeper 注册中心客户端。

Dubbo 未对 Zookeeper 服务器端做任何侵入修改，只需安装原生的 Zookeeper 服务器即可，所有注册中心逻辑适配都在调用 Zookeeper 客户端时完成。

##### 1.1.3 Windows 10

安装：

```sh
zookeeper官网，下载zookeeper-3.3.3.tar.gz包
解压到指定文件夹中，进入zookeeper-3.3.3
复制 conf/zoo_sample.cfg 到 conf/zoo.cfg
```

配置：

```SH
文本编辑器，编辑zoo.cfg
```

如果不需要集群，`zoo.cfg` 的内容如下 [2](https://dubbo.apache.org/zh/docs/v2.7/admin/install/zookeeper/#fn:2)：

```fallback
tickTime=2000
initLimit=10
syncLimit=5
dataDir=../data
clientPort=2181
```

如果需要集群，`zoo.cfg` 的内容如下 [3](https://dubbo.apache.org/zh/docs/v2.7/admin/install/zookeeper/#fn:3)：

```fallback
tickTime=2000
initLimit=10
syncLimit=5
dataDir=../data
clientPort=2181
server.1=10.20.153.10:2555:3555
server.2=10.20.153.11:2555:3555
```

并在 data 目录 [4](https://dubbo.apache.org/zh/docs/v2.7/admin/install/zookeeper/#fn:4) 下放置 myid 文件：

```sh
在zookeeper根目录下，创建目录 data
创建并编辑。文件 myid
```

myid 指明自己的 id，对应上面 `zoo.cfg` 中 `server.` 后的数字，第一台的内容为 1，第二台的内容为 2，内容如下：

```fallback
1
```

启动:

```sh
./bin/zkServer.cmd start
```

停止:

```sh
./bin/zkServer.cmd stop
```

##### 1.1.2 Centos 7

安装:

```sh
wget http://archive.apache.org/dist/zookeeper/zookeeper-3.3.3/zookeeper-3.3.3.tar.gz
tar zxvf zookeeper-3.3.3.tar.gz
cd zookeeper-3.3.3
cp conf/zoo_sample.cfg conf/zoo.cfg
```

配置:

```sh
vi conf/zoo.cfg
```

如果不需要集群，`zoo.cfg` 的内容如下 [2](https://dubbo.apache.org/zh/docs/v2.7/admin/install/zookeeper/#fn:2)：

```fallback
tickTime=2000
initLimit=10
syncLimit=5
dataDir=/home/dubbo/zookeeper-3.3.3/data
clientPort=2181
```

如果需要集群，`zoo.cfg` 的内容如下 [3](https://dubbo.apache.org/zh/docs/v2.7/admin/install/zookeeper/#fn:3)：

```fallback
tickTime=2000
initLimit=10
syncLimit=5
dataDir=/home/dubbo/zookeeper-3.3.3/data
clientPort=2181
server.1=10.20.153.10:2555:3555
server.2=10.20.153.11:2555:3555
```

并在 data 目录 [4](https://dubbo.apache.org/zh/docs/v2.7/admin/install/zookeeper/#fn:4) 下放置 myid 文件：

```sh
mkdir data
vi myid
```

myid 指明自己的 id，对应上面 `zoo.cfg` 中 `server.` 后的数字，第一台的内容为 1，第二台的内容为 2，内容如下：

```fallback
1
```

启动:

```sh
./bin/zkServer.sh start
```

停止:

```sh
./bin/zkServer.sh stop
```

命令行 [5](https://dubbo.apache.org/zh/docs/v2.7/admin/install/zookeeper/#fn:5):

```sh
telnet 127.0.0.1 2181
dump
```

或者:

```shell
echo dump | nc 127.0.0.1 2181
```

用法:

```xml
dubbo.registry.address=zookeeper://10.20.153.10:2181?backup=10.20.153.11:2181
```

或者:

```xml
<dubbo:registry protocol="zookeeper" address="10.20.153.10:2181,10.20.153.11:2181" />
```

------

1. Zookeeper是 Apache Hadoop 的子项目，强度相对较好，建议生产环境使用该注册中心 [↩︎](https://dubbo.apache.org/zh/docs/v2.7/admin/install/zookeeper/#fnref:1)
2. 其中 data 目录需改成你真实输出目录 [↩︎](https://dubbo.apache.org/zh/docs/v2.7/admin/install/zookeeper/#fnref:2)
3. 其中 data 目录和 server 地址需改成你真实部署机器的信息 [↩︎](https://dubbo.apache.org/zh/docs/v2.7/admin/install/zookeeper/#fnref:3)
4. 上面 `zoo.cfg` 中的 `dataDir` [↩︎](https://dubbo.apache.org/zh/docs/v2.7/admin/install/zookeeper/#fnref:4)
5. http://zookeeper.apache.org/doc/r3.3.3/zookeeperAdmin.html [↩︎](https://dubbo.apache.org/zh/docs/v2.7/admin/install/zookeeper/#fnref:5)



#### 1.2.管理控制台安装

https://github.com/apache/dubbo-admin/blob/develop/README_ZH.md

目前版本的管理控制台正在开发中，已经完成了服务查询和服务治理的功能，采用前后端分离的模式，具体的安装和使用步骤如下：

安装:

```sh
git clone https://github.com/apache/dubbo-admin.git 
cd  ../dubbo-admin/dubbo-admin-server
mvn clean package
```

配置：

```sh
配置文件为：
/dubbo-admin/dubbo-admin-server/src/main/resources/application.properties

主要的配置有：
# centers in dubbo2.7
admin.registry.address=zookeeper://127.0.0.1:2181
admin.config-center=zookeeper://127.0.0.1:2181
admin.metadata-report.address=zookeeper://127.0.0.1:2181
# 登录账户密码
admin.root.user.name=root
admin.root.user.password=root
```

- 前端部分

  构建

  > - `npm install`
  > - `npm run dev`

  终端展示

  ```SH
   DONE  Compiled successfully in 5183ms                                           
    App running at:
    - Local:   http://localhost:8082/
  
    Note that the development build is not optimized.
    To create a production build, run yarn build.
  ```

- 后端部分

  直接运行jar包

  > - `java -jar .\dubbo-admin-server-0.3.0-SNAPSHOT.jar`

- 最后

  页面访问url：http://localhost:8082/

  登录：root/root