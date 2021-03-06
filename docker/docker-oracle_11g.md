# Docker安装Oracle 11g

[TOC]

## 1. 拉取镜像（镜像大，拉取需要一些时间）

```shell
docker pull registry.cn-hangzhou.aliyuncs.com/helowin/oracle_11g
docker tag  registry.cn-hangzhou.aliyuncs.com/helowin/oracle_11g oracle_11g
docker rmi  registry.cn-hangzhou.aliyuncs.com/helowin/oracle_11g
```
```shell
# 数据持久化： 
# docker volume create oracle-11g

# 查看 volume 信息
# docker volume inspect oracle-11g
# [
#     {
#         "CreatedAt": "2021-03-23T19:38:29+08:00",
#         "Driver": "local",
#         "Labels": {},
#         "Mountpoint": "/var/lib/docker/volumes/oracle-11g/_data",
#         "Name": "oracle-11g",
#         "Options": {},
#         "Scope": "local"
#     }
# ]

# 挂载 volume 启动
# docker run -d -p 1521:1521 --name oracle_11g   -v oracle-11g:/home/oracle/app/oracle/data/  oracle_11g 
```
容器内部 root密码 helowin



## 2. 运行容器

```shell
docker run -d -p 1521:1521 --name oracle_11g oracle_11g
```

运行oracle镜像并映射本地1521端口



## 3. 进入容器

```shell
docker exec -it oracle_11g bash
```



## 4. 配置环境变量，使用root 配置/etc/profile ,增加以下内容

```shell
# 切换到root用户
su root# 密码helowin

# 编辑环境变量配置文件
vi /etc/profile

# 在文件最下方增加以下内容
# ORACLE 11G
export ORACLE_HOME=/home/oracle/app/oracle/product/11.2.0/dbhome_2
export ORACLE_SID=helowin
export PATH=$ORACLE_HOME/bin:$PATH
```



## 5. 使配置生效

```shell
source /etc/profile
```



## 6. 创建软连接

```shell
ln -s $ORACLE_HOME/bin/sqlplus /usr/bin
```



## 7. 登录oracle数据库 ，修改密码

```shell
su oracle
sqlplus
# username: system
# password: helowin
alter user system identified by oracle;
alter user sys identified by oracle;
ALTER PROFILE DEFAULT LIMIT PASSWORD_LIFE_TIME UNLIMITED;
exit  #退出oracle 命令行 
exit;  # 退出容器
```



## 8. 使用可视化工具远程连接

```shell
hostname: localhost
port: 1521
sid: helowin
username: system
password: oracle
```

