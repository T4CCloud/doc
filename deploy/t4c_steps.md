# T4C归档引擎安装配置

## 1.T4C归档引擎下载
T4C归档引擎1.0.0-1 beta版本下载如下： 

```
通过百度网盘分享的文件：Tape4Cloud-1.0.0-1.el8.x86_64.rpm
链接: https://pan.baidu.com/s/1dUilW3fnw65KDtJZN2qvmA 
提取码: febd 

```

## 2.T4C归档引擎安装
[root@t4ccloud t4c_image]# yum install Tape4Cloud-1.0.0-1.el8.x86_64.rpm

```
[root@t4ccloud t4c_image]# ls Tape4Cloud-1.0.0-1.el8.x86_64.rpm 
Tape4Cloud-1.0.0-1.el8.x86_64.rpm
[root@t4ccloud t4c_image]# yum install Tape4Cloud-1.0.0-1.el8.x86_64.rpm
Repository extras is listed more than once in the configuration
Repository epel is listed more than once in the configuration
Repository epel-debuginfo is listed more than once in the configuration
Repository epel-source is listed more than once in the configuration
Last metadata expiration check: 0:20:05 ago on Sun 15 Dec 2024 01:26:50 PM CST.
Dependencies resolved.
===============================================================================================================================================
 Package                      Architecture       Version                                                        Repository                Size
===============================================================================================================================================
Installing:
 Tape4Cloud                   x86_64             1.0.0-1.el8                                                    @commandline              10 M
Installing dependencies:
 boost-filesystem             x86_64             1.66.0-13.el8                                                  appstream                 49 k
 cryptopp                     x86_64             8.8.0-1.el8                                                    epel                     1.3 M
 nodejs                       x86_64             1:10.23.1-1.module_el8.4.0+645+9ce14ba2                        appstream                8.9 M
 npm                          x86_64             1:6.14.10-1.10.23.1.1.module_el8.4.0+645+9ce14ba2              appstream                3.7 M
 protobuf                     x86_64             3.5.0-15.el8                                                   appstream                892 k
Installing weak dependencies:
 nodejs-full-i18n             x86_64             1:10.23.1-1.module_el8.4.0+645+9ce14ba2                        appstream                7.3 M
Enabling module streams:
 nodejs                                          10                                                                                           

Transaction Summary
===============================================================================================================================================
Install  7 Packages

Total size: 32 M
Total download size: 22 M
Installed size: 170 M
Is this ok [y/N]: y
Downloading Packages:
(1/6): boost-filesystem-1.66.0-13.el8.x86_64.rpm                                                               2.0 MB/s |  49 kB     00:00    
(2/6): npm-6.14.10-1.10.23.1.1.module_el8.4.0+645+9ce14ba2.x86_64.rpm                                           16 MB/s | 3.7 MB     00:00    
(3/6): nodejs-full-i18n-10.23.1-1.module_el8.4.0+645+9ce14ba2.x86_64.rpm                                        26 MB/s | 7.3 MB     00:00    
(4/6): nodejs-10.23.1-1.module_el8.4.0+645+9ce14ba2.x86_64.rpm                                                  30 MB/s | 8.9 MB     00:00    
(5/6): protobuf-3.5.0-15.el8.x86_64.rpm                                                                         16 MB/s | 892 kB     00:00    
(6/6): cryptopp-8.8.0-1.el8.x86_64.rpm                                                                          26 MB/s | 1.3 MB     00:00    
-----------------------------------------------------------------------------------------------------------------------------------------------
Total                                                                                                           66 MB/s |  22 MB     00:00     
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Running scriptlet: npm-1:6.14.10-1.10.23.1.1.module_el8.4.0+645+9ce14ba2.x86_64                                                          1/1 
  Preparing        :                                                                                                                       1/1 
  Installing       : nodejs-full-i18n-1:10.23.1-1.module_el8.4.0+645+9ce14ba2.x86_64                                                       1/7 
  Installing       : npm-1:6.14.10-1.10.23.1.1.module_el8.4.0+645+9ce14ba2.x86_64                                                          2/7 
  Installing       : nodejs-1:10.23.1-1.module_el8.4.0+645+9ce14ba2.x86_64                                                                 3/7 
  Installing       : cryptopp-8.8.0-1.el8.x86_64                                                                                           4/7 
  Installing       : protobuf-3.5.0-15.el8.x86_64                                                                                          5/7 
  Installing       : boost-filesystem-1.66.0-13.el8.x86_64                                                                                 6/7 
  Running scriptlet: boost-filesystem-1.66.0-13.el8.x86_64                                                                                 6/7 
  Running scriptlet: Tape4Cloud-1.0.0-1.el8.x86_64                                                                                         7/7 
******pre-startpwd*Tape4Cloud
package Tape4Cloud is not installed
*************pre-endpwd********

  Installing       : Tape4Cloud-1.0.0-1.el8.x86_64                                                                                         7/7 
  Running scriptlet: Tape4Cloud-1.0.0-1.el8.x86_64                                                                                         7/7 
****************post-start***/***Tape4Cloud*****
****************post-end***/********

  Verifying        : boost-filesystem-1.66.0-13.el8.x86_64                                                                                 1/7 
  Verifying        : nodejs-1:10.23.1-1.module_el8.4.0+645+9ce14ba2.x86_64                                                                 2/7 
  Verifying        : nodejs-full-i18n-1:10.23.1-1.module_el8.4.0+645+9ce14ba2.x86_64                                                       3/7 
  Verifying        : npm-1:6.14.10-1.10.23.1.1.module_el8.4.0+645+9ce14ba2.x86_64                                                          4/7 
  Verifying        : protobuf-3.5.0-15.el8.x86_64                                                                                          5/7 
  Verifying        : cryptopp-8.8.0-1.el8.x86_64                                                                                           6/7 
  Verifying        : Tape4Cloud-1.0.0-1.el8.x86_64                                                                                         7/7 

Installed:
  Tape4Cloud-1.0.0-1.el8.x86_64                                           boost-filesystem-1.66.0-13.el8.x86_64                               
  cryptopp-8.8.0-1.el8.x86_64                                             nodejs-1:10.23.1-1.module_el8.4.0+645+9ce14ba2.x86_64               
  nodejs-full-i18n-1:10.23.1-1.module_el8.4.0+645+9ce14ba2.x86_64         npm-1:6.14.10-1.10.23.1.1.module_el8.4.0+645+9ce14ba2.x86_64        
  protobuf-3.5.0-15.el8.x86_64                                           

Complete!
[root@t4ccloud t4c_image]# 
```


## 3.T4C归档引擎配置

### 3.1 T4C归档引擎启动

[root@t4ccloud t4c_image]# /usr/bin/t4cadmin startService

```
[root@t4ccloud t4c_image]# /usr/bin/t4cadmin startService 
Starting the Tape for Cloud Data Management backend service.
Tape for Cloud Data Management version: 0.0.1-rhel7-protobuf.2024-05-08T14:09:34

Connecting...
The Tape for Cloud Data Management server process has been started with pid 56001.
```

### 3.2 T4C归档引擎运行状态查看

[root@t4ccloud t4c_image]# /usr/bin/t4cadmin status 

```
[root@t4ccloud t4c_image]# /usr/bin/t4cadmin status 
The Tape for Cloud Data Management server process is operating with pid 56001.
[root@t4ccloud t4c_image]# 
```


### 3.3 T4C归档引擎关联缓存文件系统

[root@t4ccloud t4c_image]# /usr/bin/t4cadmin addFs /cachefs

```
[root@t4ccloud t4c_image]# df -h /cachefs
Filesystem      Size  Used Avail Use% Mounted on
/dev/vdb         20G  175M   20G   1% /cachefs
[root@t4ccloud t4c_image]# /usr/bin/t4cadmin addFs /cachefs
[root@t4ccloud t4c_image]# df -h /cachefs
Filesystem         Size  Used Avail Use% Mounted on
T4CADMIN:/dev/vdb   20G  175M   20G   1% /cachefs
[root@t4ccloud t4c_image]# 


[root@t4ccloud t4c_image]# /usr/bin/t4cadmin show fs
device              mount point         file system type    mount options
/dev/vdb            /cachefs            xfs                 rw,relatime,attr2,inode64,logbufs=8,logbsize=32k,noquota
[root@t4ccloud t4c_image]# 

```

### 3.4 T4C归档引擎创建存储pool

[root@t4ccloud t4c_image]# /usr/bin/t4cadmin pool create -P pool01

```
[root@t4ccloud t4c_image]# /usr/bin/t4cadmin pool create -P pool01
Pool "pool01" successfully created.
[root@t4ccloud t4c_image]# /usr/bin/t4cadmin pool create -P pool02
Pool "pool02" successfully created.

[root@install-machine ~]# t4cadmin show pools
pool name    total cap.   rem. cap.    unref. cap.  #tapes
pool01       33473968     33457916     0            3
pool02       33473968     33457916     0            3
[root@install-machine ~]#
```



### 3.5 T4C格式化磁带

[root@t4ccloud t4c_image]# /usr/bin/t4cadmin pool add -F -P pool01 -t TST002L8

```
[root@install-machine ~]# t4cadmin show tapes
id              slot            total           remaining       reclaimable     in progress     status          pool            state 
                                capacity (GiB)  capacity (GiB)  estimated (GiB) (GiB) 
TST001L9        1031            16344.7109375   16329.064453125 4.2373046875    0               writable        pool01         1038  
TST002L9        0               0               0               0               not mounted yet n/a             not mounted 


[root@t4ccloud t4c_image]# /usr/bin/t4cadmin pool add -F -P pool02 -t TST002L8
Cartridge TST002L8 successfully added to tape storage pool "pool02".

[root@install-machine ~]# t4cadmin show tapes
id              slot            total           remaining       reclaimable     in progress     status          pool            state  
                                capacity (GiB)  capacity (GiB)  estimated (GiB) (GiB)  
TST001L9        1031            16344.7109375   16329.064453125 4.2373046875    0               writable        pool01        1038   
TST002L9        1031            16344.7109375   16329.064453125 4.2373046875    0               writable        pool02        1038    
          
```

### 3.6 T4C归档引擎环境检查

T4C环境配置后，可以通过命令行检查进程状态、带机状态、和磁带状态是否正常。 

```
[root@t4ccloud t4c_image]# /usr/bin/t4cadmin status 
The Tape for Cloud Data Management server process is operating with pid 56001.

[root@t4ccloud t4c_image]# t4cadmin show drives
id           device name   slot         status       usage
00078xxxxB   /dev/sg6     257          Available    free
00078xxxxB   /dev/sg4     258          Available    free

[root@t4ccloud t4c_image]# t4cadmin show tapes
id              slot            total           remaining       reclaimable     in progress     status          pool            state  
                                capacity (GiB)  capacity (GiB)  estimated (GiB) (GiB)  
TST001L9        1031            16344.7109375   16329.064453125 4.2373046875    0               writable        pool01        1038   
TST002L9        1031            16344.7109375   16329.064453125 4.2373046875    0               writable        pool02        1038    
   
```


## 4 T4C归档引擎RestAPI安装配置


### 4.1 配置nvm安装环境

创建nvm运行目录，下载nvm安装脚本

```
[root@t4ccloud ~]# mkdir /root/.nvm
[root@t4ccloud ~]# cd /root/.nvm
[root@t4ccloud .nvm]# vi nvm_install.sh
[root@t4ccloud .nvm]# pwd
/root/.nvm
[root@t4ccloud .nvm]# tree /root/.nvm
/root/.nvm
└── nvm_install.sh

0 directories, 1 file
[root@t4ccloud .nvm]# 
```

### 4.2 安装nvm环境

[root@t4ccloud .nvm]# bash -c '. ~/.nvm/nvm.sh; nvm install 16'

```
[root@t4ccloud .nvm]# bash -c '. ~/.nvm/nvm.sh; nvm install 16'
Downloading and installing node v16.20.2...
Downloading https://nodejs.org/dist/v16.20.2/node-v16.20.2-linux-x64.tar.xz...
######################################################################################################################################## 100.0%
Computing checksum with sha256sum
Checksums matched!
Now using node v16.20.2 (npm v8.19.4)
Creating default alias: default -> 16 (-> v16.20.2)
```

### 4.3 检查nvm环境和版本

[root@t4ccloud .nvm]# node -v

```
[root@t4ccloud .nvm]# node -v
v10.23.1
[root@t4ccloud .nvm]# 
```


### 4.4 创建RestAPI服务

[root@t4ccloud .nvm]# cat /etc/systemd/system/restapi.service

```
[root@t4ccloud .nvm]# cat /etc/systemd/system/restapi.service

[Unit]
Description=restapi
Wants=network-online.target
After=network-online.target

[Service]
User=root
Group=root
ExecStart=/root/.nvm/versions/node/v16.20.2/bin/node /opt/Tape4Cloud/api/index.js
Restart=always
LimitNOFILE=65536
StandardOutput=append:/var/log/t4cadmin/restapi_service.log
StandardError=append:/var/log/t4cadmin/restapi_service.err.log

[Install]
WantedBy=multi-user.target
[root@t4ccloud .nvm]# 

[root@t4ccloud .nvm]# systemctl daemon-reload
[root@t4ccloud .nvm]# systemctl status restapi
● restapi.service - restapi
   Loaded: loaded (/etc/systemd/system/restapi.service; disabled; vendor preset: disabled)
   Active: inactive (dead)
[root@t4ccloud .nvm]# 
```

修改配置文件，用localhost替换192.168.1.19

```
[root@t4ccloud .nvm]# grep -r 192.168.1.19 /opt
/opt/Tape4Cloud/api/api/openapi.yaml:- url: http://192.168.1.19:38080/api/v1
/opt/Tape4Cloud/api/index.js:var serverIp = "192.168.1.19";
```


### 4.4 配置和启动RestAPI服务

```
[root@t4ccloud .nvm]# systemctl enable restapi
Created symlink /etc/systemd/system/multi-user.target.wants/restapi.service → /etc/systemd/system/restapi.service.
[root@t4ccloud .nvm]# systemctl start restapi
[root@t4ccloud .nvm]# systemctl status restapi
● restapi.service - restapi
   Loaded: loaded (/etc/systemd/system/restapi.service; enabled; vendor preset: disabled)
   Active: active (running) since Sun 2024-12-15 17:03:24 CST; 7s ago
 Main PID: 63704 (node)
    Tasks: 11 (limit: 50628)
   Memory: 48.0M
   CGroup: /system.slice/restapi.service
           └─63704 /root/.nvm/versions/node/v16.20.2/bin/node /opt/Tape4Cloud/api/index.js

Dec 15 17:03:24 t4ccloud systemd[1]: Started restapi.
[root@t4ccloud .nvm]# 
```

### 4.5 检查RestAPI服务
```
[root@t4ccloud ]# curl -X GET "http://localhost:38080/api/v1/drives" -H  "accept: application/json" -H "api_key:key_t4c"
[
  {
    "id": "607B800118",
    "device_name": "/dev/sg4",
    "slot": 257,
    "status": "Available",
    "usage": "free"
  }
   ......
  {
    "id": "607B800114",
    "device_name": "/dev/sg10",
    "slot": 260,
    "status": "Available",
    "usage": "free"
  }
][root@t4ccloud ]# 
```


## 5 T4C归档引擎使用说明

T4C归档引擎还处在beta版本，引擎的容量和规模受到限制. 大规模使用T4C归档引擎，请联系项目团队。  

微信：Tape4Cloud


  




