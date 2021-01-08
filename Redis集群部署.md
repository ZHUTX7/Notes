# Redis集群部署

环境：CentOS7  ,  gcc版本 9.0+

注意：gcc版本低于5.0会导致无法编译 ， 因为Redis底层是C语言编写的

## 1. 安装环境

环境准备，安装gcc

```shell
yum -y install centos-release-scl
yum -y install devtoolset-9-gcc devtoolset-9-gcc-c++ devtoolset-9-binutils
scl enable devtoolset-9 bash
```

查看gcc版本 ， 确认版本大于5.0

```shell
gcc -v
```

## 2.安装Redis

### 2.1 下载Redis

进入 https://redis.io/download 页面 ， 选择 Stable(6.0)版本 ， 直接怼到最高版本

（Redis 6.0开始，引入了多线程， 底层还是单线程，并且添加了SSL,TLS协议 ，增强了安全性 。）

###  2.2 解压tar包

### 2.3进入Redis安装目录下/src

```
     cd redis6.0...../src
```

   在src目录下执行以下命令进行编译

```
make install
```

等待...结束后安装成功 ，在src目录下执行./redis-server 启动并测试

## 3. Redis集群配置（主从模式）

### 3.1 Redis - Master 配置

进入安装目录下 ，vi redis.conf文件 ，将属性替换如下

```
#绑定该地址用于开放外网访问
bind 0.0.0.0    
port 6379
logfile "6379.log" 
dbfilename "dump-6379.rdb" 
#以守护线程启动，也就是后台启动server
daemonize yes 
#开启数据持久化，防止master节点宕机重启后无法同步slave数据
rdbcompression yes
```

### 3.2 Redis - slave 配置

```
#绑定该地址用于开放外网访问
bind 0.0.0.0    
port 6379
logfile "6379.log" 
dbfilename "dump-6379.rdb" 
#以守护线程启动，也就是后台启动server
daemonize yes 
#开启数据持久化，防止master节点宕机重启后无法同步slave数据
rdbcompression yes
# 配置master ip和port
slaveof ip port
```



## 4.启动Redis

进入安装目录下src目录 ，基于配置文件启动Redis

```
./redis-server ../redis.conf
```

## 

## 5. 测试

进入Redis客户端

```
./redis-cli
```

在Master节点上输入

```
set test 123
```

在Slave节点上输入

```
get test
```

若Slave能get到准确的值，那么主从集群部署成功.



> Author: zhutianxiang       Date: 2021 / 1 / 8