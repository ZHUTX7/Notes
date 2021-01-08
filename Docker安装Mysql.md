# 1.进入Docker容器

docker run --privileged -it --name xxxx(自定义实例名称) centos:7(镜像名称) /usr/sbin/init #运行容器

# 2.配置YUM源

```
curl -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
curl -o /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-7.repo
yum clean all
yum makecache
```

# 3.安装wget

```
yum install wget
```

# 4. 下载yum repo配置文件

由于CentOS 的yum源中没有mysql，需要到mysql的官网下载yum repo配置文件。

下载命令：

wget https://dev.mysql.com/get/mysql57-community-release-el7-9.noarch.rpm

# 5.进行repo安装

rpm -ivh mysql57-community-release-el7-9.noarch.rpm

执行完成后会在/etc/yum.repos.d/目录下生成两个repo文件mysql-community.repo mysql-community-source.repo

# 6.使用Yum命令安装

**必须进入到 cd  /etc/yum.repos.d/目录后再执行以下脚本**

### 6.1安装命令：

yum install mysql-server

### 6.2启动msyql：

systemctl start mysqld #启动MySQL

### 6.3获取安装时的临时密码（在第一次登录时就是用这个密码）：

grep 'temporary password' /var/log/mysqld.log

### 6.4倘若没有获取临时密码，则

4.1、删除原来安装过的mysql残留的数据

rm -rf /var/lib/mysql

4.2.再启动mysql

systemctl start mysqld #启动MySQL

# 7. MYSQL登录

1.  修改/etc/my.cnf文件夹 添加跳过登录验证
2.  修改密码复杂要求度
3.  set global validate_password_policy=LOW;
4.   set global validate_password_length=6; 

ALTER USER 'root'@'localhost' IDENTIFIED BY '123456'



# 8.开启远程控制

MySQL默认是没有开启远程控制的，必须添加远程访问的用户，即默认是只能自己访问，别的机器是访问不了的。

### 1、方式一（已验证）：

　  1.1、连接服务器: mysql -u root -p

　　1.2、看当前所有数据库：show databases;

　　1.3、进入mysql数据库：use mysql;

　　1.4、查看mysql数据库中所有的表：show tables;

　　1.5、查看user表中的数据：select Host, User,Password from user;

　　1.6、修改user表中的Host:  update user set Host='%' where User='root'; 

​        说明： % 代表任意的客户端,可替换成具体IP地址。

　　1.7、最后刷新一下：flush privileges