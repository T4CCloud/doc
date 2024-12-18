# CubeFS DataNode配置步骤

## 1. CubeFS磁带归档架构规划
设计CubeFS磁带归档时，需要确定整个集群。 
[CubeFS 系统架构参考](https://cubefs.io/zh/docs/master/overview/architecture.html#%E6%8A%80%E6%9C%AF%E6%9E%B6%E6%9E%84)
_ ** Master节点**
集群需要三个Master节点

_ ** Metadata节点**
集群需要三个Metadata节点

_ ** 数据节点**
集群的数据节点取决于数据的副本要求，如果是三副本要求，一般采用三个节点。


## 2. CubeFS软件包下载

### 2.1 下载软件包
通过CubeFS官方网站，可以找到下载链接，比如 [CubeFS 3.4.0版本](https://github.com/cubefs/cubefs/releases/download/v3.4.0/cubefs-3.4.0-linux-amd64.tar.gz)

[root@t4ccloud cubefs]# wget https://github.com/cubefs/cubefs/releases/download/v3.3.0/cubefs-3.3.0-linux-amd64.tar.gz

```
[root@t4ccloud cubefs]# wget https://github.com/cubefs/cubefs/releases/download/v3.3.0/cubefs-3.3.0-linux-amd64.tar.gz
--2024-12-18 14:25:09--  https://github.com/cubefs/cubefs/releases/download/v3.3.0/cubefs-3.3.0-linux-amd64.tar.gz
正在解析主机 github.com (github.com)... 20.205.243.166
正在连接 github.com (github.com)|20.205.243.166|:443... 已连接。

......

已发出 HTTP 请求，正在等待回应... 200 OK
长度：86390455 (82M) [application/octet-stream]
正在保存至: “cubefs-3.3.0-linux-amd64.tar.gz”

cubefs-3.3.0-linux-amd64.tar.gz                         100%[===============================================================================================================================>]  82.39M  1.10MB/s  用时 72s

2024-12-18 14:26:22 (1.15 MB/s) - 已保存 “cubefs-3.3.0-linux-amd64.tar.gz” [86390455/86390455])

[root@t4ccloud cubefs]#
```

### 2.2 解压软件包

```
[root@t4ccloud cubefs]# tar -xvf cubefs-3.3.0-linux-amd64.tar.gz
cubefs-3.3.0/
cubefs-3.3.0/cfs-cli
cubefs-3.3.0/cfs-bcache
cubefs-3.3.0/fdstore
cubefs-3.3.0/libcfs.h
cubefs-3.3.0/cfs-authtool
cubefs-3.3.0/cfs-client
cubefs-3.3.0/libcubefs-1.0-SNAPSHOT.jar
cubefs-3.3.0/blobstore/
cubefs-3.3.0/blobstore/scheduler
cubefs-3.3.0/blobstore/clustermgr
cubefs-3.3.0/blobstore/proxy
cubefs-3.3.0/blobstore/access
cubefs-3.3.0/blobstore/blobstore-cli
cubefs-3.3.0/blobstore/blobnode
cubefs-3.3.0/libcfs.so
cubefs-3.3.0/cfs-preload
cubefs-3.3.0/cfs-fsck
cubefs-3.3.0/.version
cubefs-3.3.0/libcubefs-1.0-SNAPSHOT-jar-with-dependencies.jar
cubefs-3.3.0/cfs-server
[root@t4ccloud cubefs]#
```
### 2.3 安装软件包

```
[root@t4ccloud cubefs]# ls
cubefs-3.3.0  cubefs-3.3.0-linux-amd64.tar.gz
[root@t4ccloud cubefs]# mv cubefs-3.3.0 /opt/
[root@t4ccloud cubefs]# ls /opt/cubefs-3.3.0/
blobstore  cfs-authtool  cfs-bcache  cfs-cli  cfs-client  cfs-fsck  cfs-preload  cfs-server  fdstore  libcfs.h  libcfs.so  libcubefs-1.0-SNAPSHOT.jar  libcubefs-1.0-SNAPSHOT-jar-with-dependencies.jar
[root@t4ccloud cubefs]#
```



## CFS数据节点配置


