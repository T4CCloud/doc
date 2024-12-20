# CubeFS分布式磁带存储——监控运维操作

## 1. 查看CubeFS数据盘切片磁盘占用空间

```
Every 1.0s: df -h /cachefs && df -i /cachefs && echo ----------------------------&& find /cachefs/cubefs/data2 -type f -size +1M | grep -v log | grep -v CRC | grep -v fuse_hidden | xargs du -sh                            

Fri Dec 20 09:07:16 2024

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



## 2. 查看T4C引擎作业执行，数据召回和迁移任务的执行情况

```
[root@t4ccloud ~]# t4cadmin show requests -c 10
Request_number            Result               Start_at                 End_at                 Operation            Status              Tape_pool            Tape_id              Target_state
126                  -                    2024-12-20 13:05:24.182   2024-12-20 13:05:24.520   migration            completed            pool01                                    migrated            
121                  -                    2024-12-20 13:06:04.739   2024-12-20 13:08:23.768   transparent recall   completed                                 VTAPE0L5                                 
121                  -                    2024-12-20 12:37:48.781   2024-12-20 13:06:05.86    transparent recall   completed                                 VTAPE0L5                                 
120                  -                    2024-12-20 12:36:52.415   2024-12-20 12:36:52.564   migration            completed            pool01                                    migrated            
103                  Success              2024-12-20 00:00:02.276   2024-12-20 00:00:02.616   migration            completed            pool01                                    migrated            
88                   Success              2024-12-19 23:25:56.361   2024-12-19 23:25:56.785   migration            completed            pool01                                    migrated            
84                   Success              2024-12-19 23:24:59.801   2024-12-19 23:25:00.137   migration            completed            pool01                                    migrated            
80                   Success              2024-12-19 23:13:30.849   2024-12-19 23:13:31.184   migration            completed            pool01                                    migrated            
79                   -                    2024-12-19 23:05:02.187   2024-12-19 23:05:05.437   migration            completed            pool01                                    migrated            
78                   Success              2024-12-19 22:53:58.933   2024-12-19 22:53:59.89    migration            completed            pool01                                    migrated            
[root@t4ccloud ~]# 

```
