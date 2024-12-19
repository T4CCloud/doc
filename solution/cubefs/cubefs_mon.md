# CubeFS分布式磁带存储——监控运维操作

## 1. 查看CubeFS数据盘切片磁盘占用空间

```
Every 1.0s: df -h /cachefs && df -i /cachefs && echo ----------------------------&& find /cachefs/cubefs/data2 -type f -size +1M | grep -v log | grep -v CRC | grep -v fuse_hidden | xargs du -sh                            Fri Dec 20 09:07:16 2024

文件系统           容量  已用  可用 已用% 挂载点
T4CADMIN:/dev/md0   50G   19G   32G   38% /cachefs
文件系统             Inode 已用(I)  可用(I) 已用(I)% 挂载点
T4CADMIN:/dev/md0 26197504    5795 26191709	  1% /cachefs
----------------------------
0	/cachefs/cubefs/data2/disk/datapartition_5_128849018880/1078
0	/cachefs/cubefs/data2/disk/datapartition_5_128849018880/1079
0	/cachefs/cubefs/data2/disk/datapartition_5_128849018880/1080
0	/cachefs/cubefs/data2/disk/datapartition_9_128849018880/1105
0	/cachefs/cubefs/data2/disk/datapartition_17_128849018880/1070
0	/cachefs/cubefs/data2/disk/datapartition_17_128849018880/1071
0	/cachefs/cubefs/data2/disk/datapartition_17_128849018880/1072
```



## 6. 查看T4C引擎作业执行，数据召回和迁移任务的执行情况

```


```
