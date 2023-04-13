## 常用

### 给文件加权限

```sh
chmod  777 ./minio
```

### 查看文件夹大小

```sh
# 查看文件夹大小
du -sh *
# 查看硬盘使用了多少
du -hs /
```

### 软连接

```sh
ln -s ./abc/def/ ./r
```

### 项目的启动与停止

```sh
启动：
nohup java -jar ./jenkins.war --httpPort=8081 &

查看端口号：
netstat -nlp

停止：
fuser -k -n tcp 8101
```

| Java                         |                                                 |                           |
| ---------------------------- | ----------------------------------------------- | ------------------------- |
| 启动服务                     | 正常启动                                        | nohup java -jar xxx.jar & |
| 指定端口号                   | java -jar jenkins.war –httpPort=9999            |                           |
| 静寂启动（关闭窗口会被打断） | nohup java -jar jenkins.war –httpPort=9999      |                           |
| 静寂启动（不会打断）         | nohup java -jar ./jenkins.war --httpPort=8081 & |                           |
| 停止服务                     | 1、查看端口号                                   | netstat -nlp              |
| 2、强制杀死某端口号          | sudo fuser -k -n tcp 8101                       |                           |

### 压缩解压

```sh
#解压缩 tar.gz
tar -zxvf httpd-2.4.4.tar.bz
#压缩	tar.gz
tar -zcf npas20200610.tar.gz npas20200610.sql

# 解压tar
tar -xvf httpd-2.4.4.tar

# 解压zip
nuzip asdfas.zip
```

### 查看端口号及pid

#### 查看端口号

```sh
#查询全部端口号
 
#根据软件名查询端口号及pid
netstat -tpnl | grep java

#时刻是否能访问某端口
telnet 125.34.49.211 8080
```

#### 查看pid

```sh
#根据端口号查询pid
netstat -nlp|grep :80

#查看pid 详情
ps -x | grep 8246
```

#### 查服务路径

```sh
#查询进程情况（执行文件的路径）
ps -ef | grep postgres 

#查询服务路径
whereis nginx
```

### 用户

#### 新增用户

```sh
# 创建新用户
adduser es
passwd es
# 123456

# 添加权限
# 修改文件
vim /etc/sudoers

# 去点注释这两行的
## Allows people in group wheel to run all commands
# %wheel ALL=(ALL) ALL

# 将用户放入root分组
usermod -g root es

# 赋予某文件夹的权限
chown -R es:es /opt/es/

# 切换用户
su - es
```

## 文档

### 查看文档命令

#### cat

1、从第3000行开始，显示1000行。即显示3000~3999行

```shell
cat filename | tail -n +3000 | head -n 1000
```

2、显示1000行到3000行

```shell
cat filename| head -n 3000 | tail -n +1000
```

3、查看包涵某字符串的行

```shell
cat vsftpd.conf | grep -v "某字符串"

# 查不包涵某字符串的行
cat vsftpd.conf | grep -v "某字符串"
```

4、查看带行号的文件

```sh

```

#### tail

1、示例

```shell
tail -n 1000：显示最后1000行
tail -n +1000：从1000行开始显示，显示1000行以后的
head -n 1000：显示前面1000行
```

2、根据关键字查询日志

```shell
方法一：
cat -n 文件名称 | grep '关键字' -C 1
方法二：
vi 文件名 #进入编辑状态
/关键字 #进行搜索
按n查找下一个
按N查找上一个
按G进入文件末尾
```

3、动态查看日志

```shell
tail -f [文件名]
```

## 防火墙

### 查看状态

```shell
systemctl status firewalld
service  iptables status
firewall-cmd --state
```

报错

```sh
service  iptables status
Redirecting to /bin/systemctl status iptables.service
Unit iptables.service could not be found.
```

解决办法

```shell
yum install iptables-services
```

### 暂时关闭

```shell
systemctl stop firewalld

service  iptables stop
```

### 永久关闭

```shell
systemctl disable firewalld

Service iptables off

#这个亲测可用
systemctl stop firewalld.service
#开机禁止启动
systemctl disable firewalld.service
```

### 重启防火墙

```shell
systemctl enable firewalld

service iptables restart
```

### 永久关闭后重启

```shell
chkconfig iptables on
```

### 防火墙常用配置

#### 1、firewalld

启动： systemctl start firewalld

关闭： systemctl stop firewalld

查看状态： systemctl status firewalld 

开机禁用 ： systemctl disable firewalld

开机启用 ： systemctl enable firewalld

#### 2.systemctl

是CentOS7的服务管理工具中主要的工具，它融合之前service和chkconfig的功能于一体。

启动一个服务：systemctl start firewalld.service
关闭一个服务：systemctl stop firewalld.service
重启一个服务：systemctl restart firewalld.service
显示一个服务的状态：systemctl status firewalld.service
在开机时启用一个服务：systemctl enable firewalld.service
在开机时禁用一个服务：systemctl disable firewalld.service
查看服务是否开机启动：systemctl is-enabled firewalld.service
查看已启动的服务列表：systemctl list-unit-files|grep enabled
查看启动失败的服务列表：systemctl --failed

#### 3.firewalld-cmd

查看版本： firewall-cmd --version

查看帮助： firewall-cmd --help

显示状态： firewall-cmd --state

查看所有打开的端口： firewall-cmd --zone=public --list-ports

更新防火墙规则： firewall-cmd --reload

查看区域信息:  firewall-cmd --get-active-zones

查看指定接口所属区域： firewall-cmd --get-zone-of-interface=eth0

拒绝所有包：firewall-cmd --panic-on

取消拒绝状态： firewall-cmd --panic-off

查看是否拒绝： firewall-cmd --query-panic

#### 开启一个端口

添加

firewall-cmd --zone=public --add-port=80/tcp --permanent   （--permanent永久生效，没有此参数重启后失效）

重新载入

firewall-cmd --reload

查看

firewall-cmd --zone= public --query-port=80/tcp

删除

firewall-cmd --zone= public --remove-port=80/tcp --permanent

 

调整默认策略（默认拒绝所有访问，改成允许所有访问）：

firewall-cmd --permanent --zone=public --set-target=ACCEPT

firewall-cmd --reload

对某个IP开放多个端口：

firewall-cmd --permanent --add-rich-rule="rule family="ipv4" source address="10.159.60.29" port protocol="tcp" port="1:65535" accept"

firewall-cmd --reload

### 规则文件

```sh
vim /etc/firewalld/zones/public.xml
```

## 调优

### 查询某软件的pid

```sh
ps -ef | grep tomcat
```

### 查看jvm的线程状态

1、查询jvm的pid

```sh
jps -v
```

2、查询线程状态

```sh
stack pid
```

3、展示信息详解

```
- RUNNABLE 线程运行中或 I/O 等待

- BLOCKED 线程在等待 monitor 锁( synchronized 关键字)

- TIMED_WAITING 线程在等待唤醒，但设置了时限

- WAITING 线程在无限等待唤醒
```

### 查看gc情况

```sh
-- jstat -gcutil 688800 1s 
jstat -gc 399792 1000
```

```txt
S0C、S1C、S0U、S1U：Survivor 0``/1``区容量（Capacity）和使用量（Used）
EC、EU：Eden区容量和使用量
OC、OU：年老代容量和使用量
PC、PU：永久代容量和使用量
YGC、YGT：年轻代GC次数和GC耗时
FGC、FGCT：Full GC次数和Full GC耗时
GCT：GC总耗时<br><br>
-----------------------------------------------------------------------------------------------------------
0C：年轻代中第一个survivor（幸存区）的容量 (字节)         
S1C：年轻代中第二个survivor（幸存区）的容量 (字节)         
S0U：年轻代中第一个survivor（幸存区）目前已使用空间 (字节)         
 
S1U：年轻代中第二个survivor（幸存区）目前已使用空间 (字节)         
EC：年轻代中Eden（伊甸园）的容量 (字节)         
EU：年轻代中Eden（伊甸园）目前已使用空间 (字节)         
OC：Old代的容量 (字节)         
OU：Old代目前已使用空间 (字节)         
PC：Perm(持久代)的容量 (字节)         
PU：Perm(持久代)目前已使用空间 (字节)         
YGC：从应用程序启动到采样时年轻代中gc次数         
YGCT：从应用程序启动到采样时年轻代中gc所用时间(s)         
FGC：从应用程序启动到采样时old代(全gc)gc次数         
FGCT：从应用程序启动到采样时old代(全gc)gc所用时间(s)         
GCT：从应用程序启动到采样时gc用的总时间(s)         
NGCMN：年轻代(young)中初始化(最小)的大小 (字节)         
NGCMX：年轻代(young)的最大容量 (字节)         
NGC：年轻代(young)中当前的容量 (字节)         
OGCMN：old代中初始化(最小)的大小 (字节)         
OGCMX：old代的最大容量 (字节)         
OGC：old代当前新生成的容量 (字节)         
PGCMN：perm代中初始化(最小)的大小 (字节)         
PGCMX：perm代的最大容量 (字节)           
PGC：perm代当前新生成的容量 (字节)         
S0：年轻代中第一个survivor（幸存区）已使用的占当前容量百分比         
S1：年轻代中第二个survivor（幸存区）已使用的占当前容量百分比         
E：年轻代中Eden（伊甸园）已使用的占当前容量百分比         
O：old代已使用的占当前容量百分比         
P：perm代已使用的占当前容量百分比         
S0CMX：年轻代中第一个survivor（幸存区）的最大容量 (字节)         
S1CMX ：年轻代中第二个survivor（幸存区）的最大容量 (字节)         
ECMX：年轻代中Eden（伊甸园）的最大容量 (字节)         
DSS：当前需要survivor（幸存区）的容量 (字节)（Eden区已满）         
TT： 持有次数限制         
MTT ： 最大持有次数限制
```

## 文件推送

### scp

##### 推送

```sh
# scp推送，把本机上/opt/scp下的文件，全部推送到目标文件的opt目录下，推送事包涵scp文件夹
scp -r /opt/scp/ root@192.168.75.206:/opt/

# 对拷文件夹下所有文件 (不包括文件夹本身)
scp /opt/scp/* root@192.168.75.206:/opt/scp/

#对文件重命名
scp /opt/scp/abc.txt root@192.168.75.206:/opt/scp/aaa.txt
```

##### 拉取

```sh
#将目标机器的文件拉取到本机
scp root@192.168.75.206:/opt/scp/aaa.txt /opt/scp/

#带端口号
scp -p 4588 remote@www.abc.com:/usr/local/sin.sh /home/administrator
```

##### 免密

```sh
#服务器A需要传输文件至B：即A生成密钥，B保存密钥
#1.A服务器生成密钥，一般已有生成密钥，若无，
ssh-keygen -t rsa

#2. (输入B服务器密码)（服务器一般是root目录下.ssh）
scp .ssh/id_rsa.pub root@192.168.0.2:/root/.ssh/authorized_keys  
```



### rsync

#### 推送及拉取

```shell
# 推送
rsync -av ./source/ root@192.168.145.50:/opt/rsync/destination
# 拉取
rsync -av root@192.168.145.50:/opt/rsync/source ./destination
# 如果带端口
rsync -av -e 'ssh -p 2234' source/ user@remote_host:/destination
```

#### 免密

```shell
rsync -av /opt/module/ root@192.168.31.110:/opt/module/
```

### ftp

#### 连接

1-链接

```sh
ftp 192.168.1.1
sftp root@192.168.1.103
```

2-断开

```sh
ftp> bye 
```

### sftp

### ssh

#### 常用命令

查看ssh版本

```shell
ssh -V
```

查看运行状态

```shell
service sshd status 
```

启动

```shell
systemctl start sshd.service 
```

#### 远程登录

登录

```shell
ssh root@192.168.31.129
```

如果本地用户名与远程用户名一致，登录时可以省略用户名。

```shell
ssh host
```

带端口号的

```shell
ssh -p 2222 user@host
```

退出

```shell
exit
```

#### 设置ssh免密登录

1-生成秘钥

```shell
ssh-keygen -t rsa
```

2-推送秘钥

```shell
ssh-copy-id root@192.168.31.129
```

ssh-copy-id root@192.168.31.252



## shell脚本

### 简单的shell

```sh
#新增文件
vim hello.sh
```

```sh
#！/bin/bash
echo “Hello word!”
echo hello word
```

```shell
#增加权限
chmod +x ./hello.sh 
#执行
./hello.sh
```

### 定时任务

```sh
#!/bin/sh
#功能：每天[8-18]点每5分钟执行一次
#

# 开始时间
begin_time="800"
# 结束时间
end_time="1800"

while true
do
    # 获取当前时间
    now_time=`date "+%H%M"`
    now_minu=`date "+%M"`
    if [[ $now_time -ge $begin_time ]] && [[ $now_time -le $end_time ]] && [[ `expr $now_minu % 5` -eq 0 ]];then
        echo "hello"
    fi
    # 每分钟检测一次
    sleep 60
done              
```

### 定时2

```sh
#!/bin/sh
while true
do
	echo hello
    sleep 60
done
```

## 软件启动与停止

### zookeeper

#### 启动

```sh
cd /opt/zooKeeper/apache-zooKeeper-3.5.6-bin/bin/
#启动
./zkServer.sh  start
```

#### 查看状态

```sh
./zkServer.sh status
```

#### 停止

### kafka

先启动zookeeper

#### 启动

```sh
# 必须在根目录启动！
cd /usr/local/src/kafka/kafka_2.12-2.4.1
bin/kafka-server-start.sh -daemon config/server.properties
#查看是否启动成功
jps
#显示 34026 Kafka 则表示启动成功
```

#### 查看日志

```sh
tail -f ./logs/server.log 
```

#### 停止

```sh
#在根目录
bin/kafka-server-stop.sh config/server.properties
```

### hadoop

- 启动

```sh
cd /opt/hadoop/hadoop-3.3.1/sbin
./start-all.sh
```

- 停止

```sh
./stop-all.sh
```

### hive

- 启动

```sh
cd /opt/module/hive/bin
# 启动元数据
./hive --service metastore &
# 启动命令行
./hive
```

- 日志

```sh
tail -f /tmp/root/hive.log
```

### oracle

- 启动

  ```sh
  # 启动docker
  systemctl start docker
  
  # 启动容器
  docker start oracle_11g
  ```

- 链接

  账号：zhangyixiong

  密码：zhangyixiong

  ![image-20220519111805141](C:\Users\java0\AppData\Roaming\Typora\typora-user-images\image-20220519111805141.png)



## 用户

### 查询用户列表

```sh
cut -d : -f 1 /etc/passwd
```

















