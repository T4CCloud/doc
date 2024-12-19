# T4C引擎基本运维操作

## 1. T4C引擎基本命令

```
[root@t4ccloud ~]# t4cadmin
commands:
           t4cadmin help              - show this help message
           t4cadmin startService      - start the Tape for Cloud Data Management service in background
           t4cadmin stopService       - stop the Tape for Cloud Data Management service
           t4cadmin addFs             - adds Tape for Cloud Data Management management to a file system
           t4cadmin status            - provides information if the back end has been started
           t4cadmin migrate           - migrate file system objects from the local file system to tape
           t4cadmin recall            - recall file system objects back from tape to local disk
           t4cadmin syncs             - synchronizes the inventory with the information provided by Spectrum Archive LE
           t4cadmin version           - provides the version number of Tape for Cloud Data Management
           t4cadmin cancel            - try to cancel the request
           t4cadmin clean             - clean request history

show sub commands:
           t4cadmin show requests     - retrieve information about all or a specific Tape for Cloud Data Management requests
           t4cadmin show jobs         - retrieve information about all or a specific Tape for Cloud Data Management jobs
           t4cadmin show files        - retrieve information about the migration state of file system objects
           t4cadmin show fs           - lists the file systems managed by Tape for Cloud Data Management
           t4cadmin show drives       - lists the drives known to Tape for Cloud Data Management
           t4cadmin show tapes        - lists the cartridges known to Tape for Cloud Data Management
           t4cadmin show pools        - lists all defined tape storage pools and their sizes

pool sub commands:
           t4cadmin pool create       - create a tape storage pool
           t4cadmin pool delete       - delete a tape storage pool
           t4cadmin pool add          - add a cartridge to a tape storage pool
           t4cadmin pool remove       - removes a cartridge from a tape storage pool

[root@t4ccloud ~]# 
```


## 2. 启停T4C引擎，查看引擎状态

```
[root@t4ccloud t4c]# t4cadmin status
Unable to connect to the Tape for Cloud Data Management server.

[root@t4ccloud t4c]# t4cadmin startService
Starting the Tape for Cloud Data Management backend service.
Tape for Cloud Data Management version: 0.0.1-rhel8-protobuf.2024-12-18T14:09:34

Connecting...
The Tape for Cloud Data Management server process has been started with pid 1869.

[root@t4ccloud t4c]# t4cadmin status
The Tape for Cloud Data Management server process is operating with pid 1869.


[root@t4ccloud t4c]# t4cadmin stopService
The Tape for Cloud Data Management backend is terminating.
Waiting for the termination of the Tape for Cloud Data Management server.
10 

[root@t4ccloud t4c]# 
[root@t4ccloud t4c]# t4cadmin status
Unable to connect to the Tape for Cloud Data Management server.


[root@t4ccloud t4c]# t4cadmin startService
Starting the Tape for Cloud Data Management backend service.
Tape for Cloud Data Management version: 0.0.1-rhel8-protobuf.2024-12-18T14:09:34

Connecting...
The Tape for Cloud Data Management server process has been started with pid 1940.
[root@t4ccloud t4c]# 

```



## 3. 缓存文件数据迁移到磁带

```
15 file name(s) sent to the Tape for Cloud Data Management server
--- sending completed within 0 seconds ---                 
               resident  transferred  premigrated     migrated       failed
[00:00:03]            0            0            0           15            0

```


## 4. 缓存文件数据从磁带召回

```


```



## 5. 查看缓存文件数据状态，磁盘存储、迁移、共存。

```
[root@t4ccloud cachefs]# t4cadmin show files /cachefs/t4c_test/f1
state             size               blocks              tape id            file name
r             10485760                32640              -                  /cachefs/t4c_test/f1

[root@t4ccloud cachefs]# t4cadmin show files /cachefs/cfs/data1/disk/datapartition_5_128849018880/18
state             size               blocks              tape id            file name
m              1048576                    0             VTAPE0L5            /cachefs/cfs/data1/disk/datapartition_5_128849018880/18
[root@t4ccloud cachefs]# 

```




## 6. 查看T4C引擎当前作业

```
[root@t4ccloud cachefs]# t4cadmin show requests 
Request_number            Result               Start_at                 End_at                 Operation            Status              Tape_pool            Tape_id              Target_state
103                  Success              2024-12-20 00:00:02.276   2024-12-20 00:00:02.616   migration            completed            pool01                                    

```


## 7. 查看T4C引擎历史作业

```
[root@t4ccloud cachefs]# t4cadmin show requests -n 88
Request Number:   88                   
Operation:        migration             
Command:          t4cadmin migrate -P pool01 -f /tmp/t4c_jobs/mig-20241219232556    
Status:           completed             
Result:           Success               
Start at:         2024-12-19 23:25:56.361    
End at:           2024-12-19 23:25:56.785    
Tape Pool:        pool01                
Tape Id:                                
Target state:     migrated    
Jobs:             The current request has completed 4 jobs successfully, with 0 currently ongoing and 0 failed.      
[root@t4ccloud cachefs]# 

[root@t4ccloud cachefs]# t4cadmin show requests -c 10
Request_number            Result               Start_at                 End_at                 Operation            Status              Tape_pool            Tape_id              Target_state
103                  Success              2024-12-20 00:00:02.276   2024-12-20 00:00:02.616   migration            completed            pool01                                    migrated            
88                   Success              2024-12-19 23:25:56.361   2024-12-19 23:25:56.785   migration            completed            pool01                                    migrated            
84                   Success              2024-12-19 23:24:59.801   2024-12-19 23:25:00.137   migration            completed            pool01                                    migrated            
80                   Success              2024-12-19 23:13:30.849   2024-12-19 23:13:31.184   migration            completed            pool01                                    migrated            
79                   -                    2024-12-19 23:05:02.187   2024-12-19 23:05:05.437   migration            completed            pool01                                    migrated            
78                   Success              2024-12-19 22:53:58.933   2024-12-19 22:53:59.89    migration            completed            pool01                                    migrated            
76                   -                    2024-12-19 22:51:22.636   2024-12-19 23:26:03.948   transparent recall   completed                                 VTAPE0L5                                 
76                   -                    2024-12-19 22:51:22.740   2024-12-19 23:02:45.820   transparent recall   completed                                 VTAPE0L5                                 
76                   -                    2024-12-19 22:51:22.742   2024-12-19 23:13:11.627   transparent recall   completed                                 VTAPE0L5                                 
76                   -                    2024-12-19 23:02:45.304   2024-12-19 23:13:13.64    transparent recall   completed                                 VTAPE0L5                                 


```


## 7. 查看T4C引擎磁带池使用

```   
[root@t4ccloud cachefs]# t4cadmin show pools 
pool name    total cap.   rem. cap.    unref. cap.  #tapes
pool01       0            0            0            2           
[root@t4ccloud cachefs]# 

```


## 8. 查看T4C引擎磁带列表

```

[root@t4ccloud cachefs]# t4cadmin show tapes
id              slot            total           remaining       reclaimable     in progress     status          pool            state
                                capacity (GiB)  capacity (GiB)  estimated (GiB) (GiB)
VTAPE0L5        256             0               0               0               0               not mounted yet pool01          mounted        
VTAPE1L5        259             0               0               0               0               not mounted yet pool01          mounted        
VTAPE2L5        258             0               0               0               0               not mounted yet n/a             mounted        
VTAPE3L5        4099            0               0               0               0               not mounted yet n/a             not mounted    
VTAPE4L5        4100            0               0               0               0               not mounted yet n/a             not mounted    
VTAPE5L5        4101            0               0               0               0               not mounted yet n/a             not mounted    
VTAPE6L5        4102            0               0               0               0               not mounted yet n/a             not mounted    
VTAPE7L5        4103            0               0               0               0               not mounted yet n/a             not mounted    
VTAPE8L5        4104            0               0               0               0               not mounted yet n/a             not mounted    
VTAPE9L5        4105            0               0               0               0               not mounted yet n/a             not mounted    


```

## 8. 查看T4C引擎带机列表

```
[root@t4ccloud cachefs]# t4cadmin show drives
id           device name   slot         status       usage
VDRIVE0000   VDRIVE0000   256          Available    free        
VDRIVE0001   VDRIVE0001   257          Available    free        
VDRIVE0002   VDRIVE0002   258          Available    free        
VDRIVE0003   VDRIVE0003   259          Available    free     

```

