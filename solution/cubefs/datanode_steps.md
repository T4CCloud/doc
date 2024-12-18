# CubeFS DataNode配置步骤

**CubeFS磁带归档架构规划**
设计CubeFS磁带归档时，需要确定整个集群。 
[CubeFS 系统架构参考](https://cubefs.io/zh/docs/master/overview/architecture.html#%E6%8A%80%E6%9C%AF%E6%9E%B6%E6%9E%84)
_ ** Master节点**
集群需要三个Master节点

_ ** Metadata节点**
集群需要三个Metadata节点

_ ** 数据节点**
集群的数据节点取决于数据的副本要求，如果是三副本要求，一般采用三个节点。

** CubeFS的数据节点配置过程中，存储数据的路径必须在T4C的缓存目录下，比如/cachefs/cfs/**


## 1. CubeFS 安装依赖包

gcc-c++	4.8.5及以上	是
CMake	3.1及以上	是
bzip2-devel	1.0.6及以上	是
zlib-devel	1.2.7及以上	是
mvn	3.8.4及以上	是
Go	1.17及以上	是
kafka	1.x及以上	是（部署纠删码必需）
consul	1.1x及以上	是（部署纠删码必需）


## 2. CubeFS软件包下载

### 2.1 下载软件源代码

```
# 如该步骤已完成可跳过
[root@t4ccloud cubefs]# git clone https://github.com/cubefs/cubefs.git

[root@t4ccloud cubefs]# cd cubefs

[root@t4ccloud cubefs]# make build

...

[INFO] Building jar: /opt/cubefs-3.3.0/src_code/cubefs-master/java/target/libcubefs-1.0-SNAPSHOT-jar-with-dependencies.jar
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 14.673 s
[INFO] Finished at: 2024-12-18T15:08:45+08:00
[INFO] ------------------------------------------------------------------------
build java libcubefs success
build cfs-fsck      success
build fdstore success
build cfs-preload   success
build cfs-blockcache      success
build blobstore    success
build cfs-deploy      success
```

### 2.2 使用编译好的软件包

#### 2.2.1 下载软件包
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

#### 2.2.2 解压软件包

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
#### 2.2.3 安装软件包

```
[root@t4ccloud cubefs]# ls
cubefs-3.3.0  cubefs-3.3.0-linux-amd64.tar.gz
[root@t4ccloud cubefs]# mv cubefs-3.3.0 /opt/
[root@t4ccloud cubefs]# ls /opt/cubefs-3.3.0/
blobstore  cfs-authtool  cfs-bcache  cfs-cli  cfs-client  cfs-fsck  cfs-preload  cfs-server  fdstore  libcfs.h  libcfs.so  libcubefs-1.0-SNAPSHOT.jar  libcubefs-1.0-SNAPSHOT-jar-with-dependencies.jar
[root@t4ccloud cubefs]#
```



## 3. 配置CubeFS分布式存储配置

单机版本启动关闭方式

```
[root@t4ccloud cubefs-master]# sh ./shell/deploy.sh /cachefs/cfs eth0
peers 1:172.16.1.101:17010,2:172.16.1.102:17010,3:172.16.1.103:17010

......
begin start metanode service
start metanode service success
begin start datanode service
start datanode service success
Config has been set successfully!
wait a minute for cluster to prepare state!!!
[root@t4ccloud cubefs-master]#

[root@t4ccloud cubefs-master]# ps -ef | grep cfs
root      264089       1  0 15:28 ?        00:00:00 /opt/cubefs-3.3.0/src_code/cubefs-master/build/bin/cfs-server -f -c /cachefs/cfs/conf/master1.conf
root      264122       1  0 15:28 ?        00:00:00 /opt/cubefs-3.3.0/src_code/cubefs-master/build/bin/cfs-server -f -c /cachefs/cfs/conf/master2.conf
root      264157       1  0 15:28 ?        00:00:00 /opt/cubefs-3.3.0/src_code/cubefs-master/build/bin/cfs-server -f -c /cachefs/cfs/conf/master3.conf
root      264191       1  0 15:28 ?        00:00:00 /opt/cubefs-3.3.0/src_code/cubefs-master/build/bin/cfs-server -f -c /cachefs/cfs/conf/meta1.conf
root      264210       1  0 15:28 ?        00:00:00 /opt/cubefs-3.3.0/src_code/cubefs-master/build/bin/cfs-server -f -c /cachefs/cfs/conf/meta2.conf
root      264224       1  0 15:28 ?        00:00:00 /opt/cubefs-3.3.0/src_code/cubefs-master/build/bin/cfs-server -f -c /cachefs/cfs/conf/meta3.conf
root      264237       1  0 15:28 ?        00:00:00 /opt/cubefs-3.3.0/src_code/cubefs-master/build/bin/cfs-server -f -c /cachefs/cfs/conf/meta4.conf
root      264250       1  0 15:28 ?        00:00:00 /opt/cubefs-3.3.0/src_code/cubefs-master/build/bin/cfs-server -f -c /cachefs/cfs/conf/data1.conf
root      264263       1  0 15:28 ?        00:00:00 /opt/cubefs-3.3.0/src_code/cubefs-master/build/bin/cfs-server -f -c /cachefs/cfs/conf/data2.conf
root      264276       1  0 15:28 ?        00:00:00 /opt/cubefs-3.3.0/src_code/cubefs-master/build/bin/cfs-server -f -c /cachefs/cfs/conf/data3.conf
root      264290       1  0 15:28 ?        00:00:00 /opt/cubefs-3.3.0/src_code/cubefs-master/build/bin/cfs-server -f -c /cachefs/cfs/conf/data4.conf
root      264367  247308  0 15:29 pts/2    00:00:00 grep --color=auto cfs
[root@t4ccloud cubefs-master]# pwd
/opt/cubefs-3.3.0/src_code/cubefs-master
[root@t4ccloud cubefs-master]# sh ./shell/stop.sh
stop all service
stop client
[root@t4ccloud cubefs-master]#
```

### 3.1 Master节点配置

_ Master节点的配置文件

```
[root@t4ccloud cubefs-master]# cat /cachefs/cfs/conf/master1.conf
{
  "clusterName": "cfs_dev",
  "id": "1",
  "role": "master",
  "ip": "172.16.1.101",
  "bindIp": "true",
  "listen": "17010",
  "peers": "1:172.16.1.101:17010,2:172.16.1.102:17010,3:172.16.1.103:17010",
  "retainLogs": "2000",
  "logLevel": "debug",
  "logDir": "/cachefs/cfs/master1/logs",
  "warnLogDir": "/cachefs/cfs/master1/logs",
  "walDir": "/cachefs/cfs/master1/wal",
  "storeDir": "/cachefs/cfs/master1/store",
  "metaNodeReservedMem": "536870912"
}

[root@t4ccloud cubefs-master]# cat /cachefs/cfs/conf/master2.conf
{
  "clusterName": "cfs_dev",
  "id": "2",
  "role": "master",
  "ip": "172.16.1.102",
  "bindIp": "true",
  "listen": "17010",
  "peers": "1:172.16.1.101:17010,2:172.16.1.102:17010,3:172.16.1.103:17010",
  "retainLogs": "2000",
  "logLevel": "debug",
  "logDir": "/cachefs/cfs/master2/logs",
  "warnLogDir": "/cachefs/cfs/master2/logs",
  "walDir": "/cachefs/cfs/master2/wal",
  "storeDir": "/cachefs/cfs/master2/store",
  "metaNodeReservedMem": "536870912"
}
[root@t4ccloud cubefs-master]# cat /cachefs/cfs/conf/master3.conf
{
  "clusterName": "cfs_dev",
  "id": "3",
  "role": "master",
  "ip": "172.16.1.103",
  "bindIp": "true",
  "listen": "17010",
  "peers": "1:172.16.1.101:17010,2:172.16.1.102:17010,3:172.16.1.103:17010",
  "retainLogs": "2000",
  "logLevel": "debug",
  "logDir": "/cachefs/cfs/master3/logs",
  "warnLogDir": "/cachefs/cfs/master3/logs",
  "walDir": "/cachefs/cfs/master3/wal",
  "storeDir": "/cachefs/cfs/master3/store",
  "metaNodeReservedMem": "536870912"
}
```

_ 后台启动Master节点

```
[root@t4ccloud cubefs-master]# /opt/cubefs-3.3.0/src_code/cubefs-master/build/bin/cfs-server -f -c /cachefs/cfs/conf/master1.conf
[root@t4ccloud cubefs-master]# /opt/cubefs-3.3.0/src_code/cubefs-master/build/bin/cfs-server -f -c /cachefs/cfs/conf/master2.conf
[root@t4ccloud cubefs-master]# /opt/cubefs-3.3.0/src_code/cubefs-master/build/bin/cfs-server -f -c /cachefs/cfs/conf/master3.conf
```


### 3.2 Metadata节点配置

_ Metadata节点的配置文件

```
[root@t4ccloud cubefs-master]# cat  /cachefs/cfs/conf/meta1.conf
{
  "role": "metanode",
  "listen": "17210",
  "localIP": "172.16.1.101",
  "bindIp": "true",
  "raftHeartbeatPort": "17230",
  "raftReplicaPort": "17240",
  "logLevel": "debug",
  "logDir": "/cachefs/cfs/meta1/logs",
  "warnLogDir": "/cachefs/cfs/meta1/logs",
  "memRatio": "70",
  "metadataDir": "/cachefs/cfs/meta1/meta",
  "raftDir": "/cachefs/cfs/meta1/raft",
  "masterAddr": [
 	"172.16.1.101:17010","172.16.1.102:17010","172.16.1.103:17010"
  ]
}
[root@t4ccloud cubefs-master]# cat  /cachefs/cfs/conf/meta2.conf
{
  "role": "metanode",
  "listen": "17210",
  "localIP": "172.16.1.102",
  "bindIp": "true",
  "raftHeartbeatPort": "17230",
  "raftReplicaPort": "17240",
  "logLevel": "debug",
  "logDir": "/cachefs/cfs/meta2/logs",
  "warnLogDir": "/cachefs/cfs/meta2/logs",
  "memRatio": "70",
  "metadataDir": "/cachefs/cfs/meta2/meta",
  "raftDir": "/cachefs/cfs/meta2/raft",
  "masterAddr": [
 	"172.16.1.101:17010","172.16.1.102:17010","172.16.1.103:17010"
  ]
}
[root@t4ccloud cubefs-master]# cat  /cachefs/cfs/conf/meta3.conf
{
  "role": "metanode",
  "listen": "17210",
  "localIP": "172.16.1.103",
  "bindIp": "true",
  "raftHeartbeatPort": "17230",
  "raftReplicaPort": "17240",
  "logLevel": "debug",
  "logDir": "/cachefs/cfs/meta3/logs",
  "warnLogDir": "/cachefs/cfs/meta3/logs",
  "memRatio": "70",
  "metadataDir": "/cachefs/cfs/meta3/meta",
  "raftDir": "/cachefs/cfs/meta3/raft",
  "masterAddr": [
 	"172.16.1.101:17010","172.16.1.102:17010","172.16.1.103:17010"
  ]
}
[root@t4ccloud cubefs-master]# cat  /cachefs/cfs/conf/meta4.conf
{
  "role": "metanode",
  "listen": "17210",
  "localIP": "172.16.1.104",
  "bindIp": "true",
  "raftHeartbeatPort": "17230",
  "raftReplicaPort": "17240",
  "logLevel": "debug",
  "logDir": "/cachefs/cfs/meta4/logs",
  "warnLogDir": "/cachefs/cfs/meta4/logs",
  "memRatio": "70",
  "metadataDir": "/cachefs/cfs/meta4/meta",
  "raftDir": "/cachefs/cfs/meta4/raft",
  "masterAddr": [
 	"172.16.1.101:17010","172.16.1.102:17010","172.16.1.103:17010"
  ]
}
```

_ 后台启动Metadata节点

```
[root@t4ccloud cubefs-master]# /opt/cubefs-3.3.0/src_code/cubefs-master/build/bin/cfs-server -f -c /cachefs/cfs/conf/meta1.conf
[root@t4ccloud cubefs-master]# /opt/cubefs-3.3.0/src_code/cubefs-master/build/bin/cfs-server -f -c /cachefs/cfs/conf/meta2.conf
[root@t4ccloud cubefs-master]# /opt/cubefs-3.3.0/src_code/cubefs-master/build/bin/cfs-server -f -c /cachefs/cfs/conf/meta3.conf
[root@t4ccloud cubefs-master]# /opt/cubefs-3.3.0/src_code/cubefs-master/build/bin/cfs-server -f -c /cachefs/cfs/conf/meta4.conf
```



### 3.3 Data节点配置

_ Data节点的配置文件

```
[root@t4ccloud cubefs-master]# cat  /cachefs/cfs/conf/data1.conf
{
  "role": "datanode",
  "listen": "17310",
  "localIP": "172.16.1.101",
  "bindIp": "true",
  "raftHeartbeat": "17330",
  "raftReplica": "17340",
  "raftDir": "/cachefs/cfs/data1/raftlog/datanode",
  "logDir": "/cachefs/cfs/data1/logs",
  "warnLogDir": "/cachefs/cfs/data1/logs",
  "logLevel": "debug",
  "disks": [
  	"/cachefs/cfs/data1/disk:3930691768"
  ],
  "enableSmuxConnPool": "true",
  "masterAddr": [
      "172.16.1.101:17010","172.16.1.102:17010","172.16.1.103:17010"
  ]
}
[root@t4ccloud cubefs-master]# cat  /cachefs/cfs/conf/data2.conf
{
  "role": "datanode",
  "listen": "17310",
  "localIP": "172.16.1.102",
  "bindIp": "true",
  "raftHeartbeat": "17330",
  "raftReplica": "17340",
  "raftDir": "/cachefs/cfs/data2/raftlog/datanode",
  "logDir": "/cachefs/cfs/data2/logs",
  "warnLogDir": "/cachefs/cfs/data2/logs",
  "logLevel": "debug",
  "disks": [
  	"/cachefs/cfs/data2/disk:3930691768"
  ],
  "enableSmuxConnPool": "true",
  "masterAddr": [
      "172.16.1.101:17010","172.16.1.102:17010","172.16.1.103:17010"
  ]
}
[root@t4ccloud cubefs-master]# cat  /cachefs/cfs/conf/data3.conf
{
  "role": "datanode",
  "listen": "17310",
  "localIP": "172.16.1.103",
  "bindIp": "true",
  "raftHeartbeat": "17330",
  "raftReplica": "17340",
  "raftDir": "/cachefs/cfs/data3/raftlog/datanode",
  "logDir": "/cachefs/cfs/data3/logs",
  "warnLogDir": "/cachefs/cfs/data3/logs",
  "logLevel": "debug",
  "disks": [
  	"/cachefs/cfs/data3/disk:3930691768"
  ],
  "enableSmuxConnPool": "true",
  "masterAddr": [
      "172.16.1.101:17010","172.16.1.102:17010","172.16.1.103:17010"
  ]
}
[root@t4ccloud cubefs-master]# cat  /cachefs/cfs/conf/data4.conf
{
  "role": "datanode",
  "listen": "17310",
  "localIP": "172.16.1.104",
  "bindIp": "true",
  "raftHeartbeat": "17330",
  "raftReplica": "17340",
  "raftDir": "/cachefs/cfs/data4/raftlog/datanode",
  "logDir": "/cachefs/cfs/data4/logs",
  "warnLogDir": "/cachefs/cfs/data4/logs",
  "logLevel": "debug",
  "disks": [
  	"/cachefs/cfs/data4/disk:3930691768"
  ],
  "enableSmuxConnPool": "true",
  "masterAddr": [
      "172.16.1.101:17010","172.16.1.102:17010","172.16.1.103:17010"
  ]
}
[root@t4ccloud cubefs-master]#
```

_ 后台启动Data节点

```
[root@t4ccloud cubefs-master]# /opt/cubefs-3.3.0/src_code/cubefs-master/build/bin/cfs-server -f -c /cachefs/cfs/conf/data1.conf
[root@t4ccloud cubefs-master]# /opt/cubefs-3.3.0/src_code/cubefs-master/build/bin/cfs-server -f -c /cachefs/cfs/conf/data2.conf
[root@t4ccloud cubefs-master]# /opt/cubefs-3.3.0/src_code/cubefs-master/build/bin/cfs-server -f -c /cachefs/cfs/conf/data3.conf
[root@t4ccloud cubefs-master]# /opt/cubefs-3.3.0/src_code/cubefs-master/build/bin/cfs-server -f -c /cachefs/cfs/conf/data4.conf
```

## 4. 配置CubeFS客户端

_ CubeFS客户端节点的配置文件

```
[root@t4ccloud cubefs-master]# cat /cachefs/cfs/conf/client.conf
{
  "masterAddr": "172.16.1.101:17010,172.16.1.102:17010,172.16.1.103:17010",
  "mountPoint": "/cfs_client",
  "volName": "ltptest",
  "owner": "ltp",
  "logDir": "/cachefs/cfs/client/log",
  "logLevel": "debug"
}
```
_ 后台启动Client节点

```
[root@t4ccloud cubefs-master]# /opt/cubefs-3.3.0/src_code/cubefs-master/build/bin/cfs-client -f -c /cachefs/cfs/conf/client.conf

[root@t4ccloud cubefs-master]# df -h /cfs_client
文件系统        容量  已用  可用 已用% 挂载点
cubefs-ltptest   10G     0   10G    0% /cfs_client
[root@t4ccloud cubefs-master]#
[root@t4ccloud cubefs]# df -h
文件系统                容量  已用  可用 已用% 挂载点
T4CADMIN:/dev/vdb        20G  295M   20G    2% /cachefs
cubefs-ltptest           10G     0   10G    0% /cfs_client
[root@t4ccloud cubefs]# cp cubefs-3.3.0-linux-amd64.tar.gz /cfs_client/f1
[root@t4ccloud cubefs]# df -h
df: /cachefs/cfs/client/mnt: 传输端点尚未连接
文件系统                容量  已用  可用 已用% 挂载点
T4CADMIN:/dev/vdb        20G  707M   20G    4% /cachefs
cubefs-ltptest           10G     0   10G    0% /cfs_client
```

## 5. 配置CubeFS对象网关

_ CubeFS客户端节点的配置文件

```
[root@t4ccloud cubefs-master]# cat /cachefs/cfs/conf/object.conf
{
  "role": "objectnode",
  "listen": "17410",
  "logDir": "/cachefs/cfs/object/logs",
  "logLevel": "debug",
  "masterAddr": [
    "172.16.1.101:17010","172.16.1.102:17010","172.16.1.103:17010"
  ]
}
```
_ 后台启动对象网关节点

```
[root@t4ccloud cubefs-master]#  /opt/cubefs-3.3.0/src_code/cubefs-master/build/bin/cfs-server -f -c /cachefs/cfs/conf/object.conf
```

## 6. 查看CubeFS存储集群状态

```
[root@t4ccloud cubefs-master]# ./build/bin/cfs-cli cluster info
[Cluster]
  Cluster name       : cfs_dev
  Master leader      : 172.16.1.101:17010
  Master-1           : 172.16.1.101:17010
  Master-2           : 172.16.1.102:17010
  Master-3           : 172.16.1.103:17010
  Auto allocate      : Enabled
  MetaNode count (active/total)    : 4/4
  MetaNode used                    : 0 GB
  MetaNode available               : 21 GB
  MetaNode total                   : 21 GB
  DataNode count (active/total)    : 4/4
  DataNode used                    : 0 GB
  DataNode available               : 59 GB
  DataNode total                   : 59 GB
  Volume count       : 2
  Allow Mp Decomm    : Enabled
  EbsAddr            :
  LoadFactor         : 0
  DpRepairTimeout    : 2h0m0s
  DataPartitionTimeout : 20m0s
  volDeletionDelayTime : 48 h
  EnableAutoDecommission: false
  EnableAutoDpMetaRepair: false
  MarkDiskBrokenThreshold : 0%
  DecommissionDiskLimit: 1
  BatchCount         : 0
  MarkDeleteRate     : 0
  DeleteWorkerSleepMs: 0
  AutoRepairRate     : 0
  MaxDpCntLimit      : 3000
  MaxMpCntLimit      : 300

[root@t4ccloud cubefs-master]#
```


## 7. 修改T4C归档引擎归档数据路径

修改CubeFS的数据节点的存储路径为/cachefs/cfs/data*

```
#分布式存储切片数据存储路径
data_path=/cachefs/cfs/data*
#超过多长时间的数据被迁移到磁带
archive_atime=2
#多大的数据切片迁移到磁带，单位MB
archive_size=1
#tape pool
t4c_pool=pool01


mkdir -p /tmp/t4c_jobs/

mig_files=mig-`date +"%Y%m%d%H%M%S"`
#查找满足条件的文件列表

echo find $data_path -type f -amin +$archive_atime -size +$archive_size \
 -exec du -h {} + | awk '$1 ~ /[0-9]+[KMG]/ && (($1 ~ /K/ && $1+0 > 1024) || $1 ~ /[MG]/) {print $0}' \
 | grep -v "minio.sys" | awk '{$1=""; print $0}' | awk '{print substr($0, 2)}' > /tmp/t4c_jobs/$mig_files

find $data_path -type f -amin +$archive_atime -size +$archive_size \
 -exec du -h {} + | awk '$1 ~ /[0-9]+[KMG]/ && (($1 ~ /K/ && $1+0 > 1024) || $1 ~ /[MG]/) {print $0}' \
 | grep -v "minio.sys" | awk '{$1=""; print $0}' | awk '{print substr($0, 2)}' > /tmp/t4c_jobs/$mig_files

#迁移数据到磁带，如果是召回数据，chunk本地磁盘空间
/opt/Tape4Cloud/bin/t4cadmin migrate -P $t4c_pool -f /tmp/t4c_jobs/$mig_files

```
