# T4C元信息数据库Mongodb的安装配置步骤

## 1. T4C元信息数据库Mongodb
T4C数据引擎的元数据信息采用MongoDB存储。

### 1.1 查看操作系统版本

[root@t4ccloud yum.repos.d]# cat /etc/redhat-release

```
[root@t4ccloud yum.repos.d]# cat /etc/redhat-release
CentOS release 8
[root@t4ccloud yum.repos.d]# 
```
## 2. MongoDB安装yum源配置

### 2.1 配置T4C 安装yum mongodb_base源

[root@t4ccloud yum.repos.d]# cat mongodb_base.repo

```
[root@t4ccloud yum.repos.d]# cat mongodb_base.repo
[mongodb-org-8.0]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/8/mongodb-org/8.0/x86_64/
gpgcheck=0
enabled=1
gpgkey=https://pgp.mongodb.com/server-8.0.asc
```

### 2.2 清除旧yum缓存
[root@t4ccloud yum.repos.d]# yum clean all

```
[root@t4ccloud yum.repos.d]# yum clean all
Repository extras is listed more than once in the configuration
282 files removed
[root@t4ccloud yum.repos.d]# 
```

### 2.3 生产新yum缓存
[root@t4ccloud yum.repos.d]# yum makecache

```
[root@t4ccloud yum.repos.d]# yum makecache
Repository extras is listed more than once in the configuration
Repository epel is listed more than once in the configuration
Repository epel-debuginfo is listed more than once in the configuration
Repository epel-source is listed more than once in the configuration
CentOS Stream 8 - AppStream                                                                                    115 MB/s |  29 MB     00:00    
CentOS Stream 8 - BaseOS                                                                                        66 MB/s |  10 MB     00:00    
CentOS Stream 8 - Extras                                                                                       1.3 MB/s |  18 kB     00:00    
CentOS Stream 8 - Extras common packages                                                                       590 kB/s | 8.0 kB     00:00    
Extra Packages for Enterprise Linux 8 - x86_64                                                                  80 MB/s |  14 MB     00:00    
Extra Packages for Enterprise Linux Modular 8 - x86_64                                                          14 MB/s | 733 kB     00:00    
MongoDB Repository                                                                                              21 kB/s |  17 kB     00:00    
CentOS-8 - Base - mirrors.aliyun.com                                                                           1.0 MB/s |  10 MB     00:09    
CentOS-8 - Updates - mirrors.aliyun.com                                                                        832 kB/s |  34 MB     00:42    
DevToolset-11                                                                                                  669 kB/s |  18 MB     00:27    
Metadata cache created.
[root@t4ccloud yum.repos.d]# 

```

## 3. MongoDB安装步骤
[root@t4ccloud yum.repos.d]# yum install mongodb-org libcurl openssl

```
[root@t4ccloud yum.repos.d]# yum install mongodb-org libcurl openssl
Repository extras is listed more than once in the configuration
Repository epel is listed more than once in the configuration
Repository epel-debuginfo is listed more than once in the configuration
Repository epel-source is listed more than once in the configuration
Last metadata expiration check: 0:03:17 ago on Sun 15 Dec 2024 01:26:50 PM CST.
Package libcurl-7.61.1-34.el8.x86_64 is already installed.
Package openssl-1:1.1.1k-12.el8.x86_64 is already installed.
Dependencies resolved.
===============================================================================================================================================
 Package                                           Architecture            Version                      Repository                        Size
===============================================================================================================================================
Installing:
 mongodb-org                                       x86_64                  8.0.4-1.el8                  mongodb-org-8.0                  9.5 k
Installing dependencies:
 mongodb-database-tools                            x86_64                  100.10.0-1                   mongodb-org-8.0                   25 M
 mongodb-mongosh                                   x86_64                  2.3.6-1.el8                  mongodb-org-8.0                   56 M
 mongodb-org-database                              x86_64                  8.0.4-1.el8                  mongodb-org-8.0                  9.6 k
 mongodb-org-database-tools-extra                  x86_64                  8.0.4-1.el8                  mongodb-org-8.0                   15 k
 mongodb-org-mongos                                x86_64                  8.0.4-1.el8                  mongodb-org-8.0                   30 M
 mongodb-org-server                                x86_64                  8.0.4-1.el8                  mongodb-org-8.0                   41 M
 mongodb-org-tools                                 x86_64                  8.0.4-1.el8                  mongodb-org-8.0                  9.5 k

Transaction Summary
===============================================================================================================================================
Install  8 Packages

Total download size: 153 M
Installed size: 701 M
Is this ok [y/N]: y
Downloading Packages:
(1/8): mongodb-org-8.0.4-1.el8.x86_64.rpm                                                                       19 kB/s | 9.5 kB     00:00    
(2/8): mongodb-org-database-8.0.4-1.el8.x86_64.rpm                                                             4.7 kB/s | 9.6 kB     00:02    
(3/8): mongodb-org-database-tools-extra-8.0.4-1.el8.x86_64.rpm                                                  21 kB/s |  15 kB     00:00    
(4/8): mongodb-org-mongos-8.0.4-1.el8.x86_64.rpm                                                               443 kB/s |  30 MB     01:10    
(5/8): mongodb-mongosh-2.3.6.x86_64.rpm                                                                        477 kB/s |  56 MB     02:01    
(6/8): mongodb-org-tools-8.0.4-1.el8.x86_64.rpm                                                                 36 kB/s | 9.5 kB     00:00    
(7/8): mongodb-org-server-8.0.4-1.el8.x86_64.rpm                                                               704 kB/s |  41 MB     00:59    
(8/8): mongodb-database-tools-100.10.0-1.x86_64.rpm                                                            190 kB/s |  25 MB     02:14    
-----------------------------------------------------------------------------------------------------------------------------------------------
Total                                                                                                          1.1 MB/s | 153 MB     02:14     
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                                                                                       1/1 
  Installing       : mongodb-org-database-tools-extra-8.0.4-1.el8.x86_64                                                                   1/8 
  Running scriptlet: mongodb-org-server-8.0.4-1.el8.x86_64                                                                                 2/8 
  Installing       : mongodb-org-server-8.0.4-1.el8.x86_64                                                                                 2/8 
  Running scriptlet: mongodb-org-server-8.0.4-1.el8.x86_64                                                                                 2/8 
Created symlink /etc/systemd/system/multi-user.target.wants/mongod.service → /usr/lib/systemd/system/mongod.service.

  Installing       : mongodb-org-mongos-8.0.4-1.el8.x86_64                                                                                 3/8 
  Installing       : mongodb-org-database-8.0.4-1.el8.x86_64                                                                               4/8 
  Installing       : mongodb-mongosh-2.3.6-1.el8.x86_64                                                                                    5/8 
  Running scriptlet: mongodb-database-tools-100.10.0-1.x86_64                                                                              6/8 
  Installing       : mongodb-database-tools-100.10.0-1.x86_64                                                                              6/8 
  Running scriptlet: mongodb-database-tools-100.10.0-1.x86_64                                                                              6/8 
  Installing       : mongodb-org-tools-8.0.4-1.el8.x86_64                                                                                  7/8 
  Installing       : mongodb-org-8.0.4-1.el8.x86_64                                                                                        8/8 
  Running scriptlet: mongodb-org-8.0.4-1.el8.x86_64                                                                                        8/8 
  Verifying        : mongodb-database-tools-100.10.0-1.x86_64                                                                              1/8 
  Verifying        : mongodb-mongosh-2.3.6-1.el8.x86_64                                                                                    2/8 
  Verifying        : mongodb-org-8.0.4-1.el8.x86_64                                                                                        3/8 
  Verifying        : mongodb-org-database-8.0.4-1.el8.x86_64                                                                               4/8 
  Verifying        : mongodb-org-database-tools-extra-8.0.4-1.el8.x86_64                                                                   5/8 
  Verifying        : mongodb-org-mongos-8.0.4-1.el8.x86_64                                                                                 6/8 
  Verifying        : mongodb-org-server-8.0.4-1.el8.x86_64                                                                                 7/8 
  Verifying        : mongodb-org-tools-8.0.4-1.el8.x86_64                                                                                  8/8 

Installed:
  mongodb-database-tools-100.10.0-1.x86_64    mongodb-mongosh-2.3.6-1.el8.x86_64                     mongodb-org-8.0.4-1.el8.x86_64          
  mongodb-org-database-8.0.4-1.el8.x86_64     mongodb-org-database-tools-extra-8.0.4-1.el8.x86_64    mongodb-org-mongos-8.0.4-1.el8.x86_64   
  mongodb-org-server-8.0.4-1.el8.x86_64       mongodb-org-tools-8.0.4-1.el8.x86_64                  

Complete!
[root@t4ccloud yum.repos.d]# 
```


## 4. 启动MongoDB服务
[root@t4ccloud yum.repos.d]# systemctl start mongod

```
[root@t4ccloud yum.repos.d]# systemctl start mongod
[root@t4ccloud yum.repos.d]# systemctl enable  mongod
[root@t4ccloud yum.repos.d]# systemctl status mongod
● mongod.service - MongoDB Database Server
   Loaded: loaded (/usr/lib/systemd/system/mongod.service; enabled; vendor preset: disabled)
   Active: active (running) since Sun 2024-12-15 13:33:48 CST; 13s ago
     Docs: https://docs.mongodb.org/manual
 Main PID: 54433 (mongod)
   Memory: 101.3M
   CGroup: /system.slice/mongod.service
           └─54433 /usr/bin/mongod -f /etc/mongod.conf

Dec 15 13:33:48 t4ccloud systemd[1]: Started MongoDB Database Server.
Dec 15 13:33:48 t4ccloud mongod[54433]: {"t":{"$date":"2024-12-15T05:33:48.651Z"},"s":"I",  "c":"CONTROL",  "id":7484500, "ctx":"main","msg":">
lines 1-11/11 (END)
```


