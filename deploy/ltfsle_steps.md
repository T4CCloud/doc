# 带库驱动LTFSLE配置步骤

LTFSLE是带库设备的驱动程序，详细过程参考 
[带库驱动在线文档](https://www.ibm.com/docs/en/storage-archive-le/2.4.6)


## 1.带库LTFSLE驱动下载

带库驱动2.4.3版本下载如下： 

```
通过百度网盘分享的文件：ltfsle-2.4.3.0-10450-free.x86_64.bin
链接：https://pan.baidu.com/s/1hgAgAhH4RZndZKhqIRtHWA 
提取码：xmls 
```

python3-pyxattr下载如下：

```
通过百度网盘分享的文件：python3-pyxattr-0.5.3-18.el8.x86_64...
链接：https://pan.baidu.com/s/1nm83c8F1ia_Bs0DzbZMMAA 
提取码：gjfp
```

### 1.2 带库驱动最新下载
如果网盘的版本不够新，可以官方网站下载最新带库驱动。  
  
下载步骤如下： 
* 在您的 Web 浏览器中打开以下 [http://www.ibm.com/support/fixcentral](http://www.ibm.com/support/fixcentral)
* 从“产品组”菜单中选择System Storage。 
* 从“System Storage”菜单中选择Tape System。
* 从“Tape System”菜单中选择Tape drives and software。
* 从“Tape drives and software”菜单中选择 IBM Spectrum Archive™ Library Edition (LTFSLE)。
* 从“LTFSLE安装版本”菜单中选择2.4最新版本。
* 从“平台”菜单中选择您的操作系统Linux。
* 单击“继续”以查看可用下载列表。
* 选择要下载的最新版本的 IBM Spectrum Archive™ Library Edition (LTFSLE)，然后单击“继续”。
* 按照 Fix Central 下载页面上的说明下载 LTFSLE 工具。

 [** 下载需要提前注册IBM用户ID **](https://www.ibm.com/account/reg/cn-zh/signup?formid=urx-19776&target=https%3A%2F%2Flogin.ibm.com%2Foidc%2Fendpoint%2Fdefault%2Fauthorize%3FqsId%3D5e82b4bb-aadc-44a0-a84c-f6c4b7e4887c%26client_id%3DMyIBMLondonProdCI)

## 2. 带库驱动依赖包安装
[带库驱动依赖包安装步骤，参考系统环境配置](./env_steps.md)

## 3. 准备带库驱动安装包

* 修改带库驱动安装包可执行文件

[root@t4ccloud t4c_image]# chmod +x ltfsle-2.4.3.0-10450-free.x86_64.bin

```
[root@t4ccloud t4c_image]# chmod +x ltfsle-2.4.3.0-10450-free.x86_64.bin 
[root@t4ccloud t4c_image]# ls -la
total 20980
drwxr-xr-x  2 root root     4096 Dec 15 11:46 .
dr-xr-x---. 9 root root     4096 Dec 15 11:46 ..
-rwxr-xr-x  1 root root 10477871 Jan 18  2021 ltfsle-2.4.3.0-10450-free.x86_64.bin
-rw-r--r--  1 root root 10990292 Dec 13 20:55 Tape4Cloud-1.0.0-1.el8.x86_64.rpm
[root@t4ccloud t4c_image]# 
```
* 安装包依赖Java，可以安装OpenJDK

[root@t4ccloud t4c_image]# yum install java-1.8.0-openjdk


```
[root@t4ccloud t4c_image]# yum install java-1.8.0-openjdk
```

* 解压带库驱动包

[root@t4ccloud t4c_image]# ./ltfsle-2.4.3.0-10450-free.x86_64.bin 

```
[root@t4ccloud t4c_image]# ./ltfsle-2.4.3.0-10450-free.x86_64.bin 
Preparing to install
Extracting the installation resources from the installer archive...
Configuring the installer for this system's environment...

Launching installer...


Graphical installers are not supported by the VM. The console mode will be used instead...

===============================================================================
IBM Spectrum Archive Library Edition             (created with InstallAnywhere)
-------------------------------------------------------------------------------

Preparing CONSOLE Mode Installation...

===============================================================================


    LICENSE INFORMATION
    
    ......
    
    timesharing, or to sublicense, rent, or lease the Program unless
 
Press Enter to continue viewing the license agreement, or enter "1" to 
   accept the agreement, "2" to decline it, "3" to print it, or "99" to go back
   to the previous screen.: 1
 

===============================================================================
Installing...
-------------

 [==================|==================|==================|==================]
 [------------------|------------------|------------------|------------------]
[root@t4ccloud t4c_image]# 


```

* 带库驱动安装软件

```
[root@t4ccloud t4c_image]# tree /root/LTFSLE
/root/LTFSLE
├── _IBM Spectrum Archive Library Edition_installation
│   ├── Change IBM Spectrum Archive Library Edition Installation
│   ├── Change IBM Spectrum Archive Library Edition Installation.lax
│   ├── InstallScript.iap_xml
│   ├── installvariables.properties
│   ├── Logs
│   │   └── IBM_Spectrum_Archive_Library_Edition_Install_12_15_2024_12_19_40.log
│   └── uninstaller.jar
├── license
│   ├── ......
│   └── Turkish.txt
└── rpm.10450
    ├── INSTALL_LE.10450
    ├── README_LE.10450
    ├── RHEL7
    │   ├── ltfs-admin-utils-2.4.3.0-10450-RHEL7.x86_64.rpm
    │   ├── ltfs-admin-utils-devel-2.4.3.0-10450-RHEL7.x86_64.rpm
    │   ├── ltfsle-2.4.3.0-10450-RHEL7.x86_64.rpm
    │   ├── ltfsle-library-2.4.3.0-10450-RHEL7.x86_64.rpm
    │   └── ltfs-license-2.1.0-RHEL7.x86_64.rpm
    └── RHEL8
        ├── ltfs-admin-utils-2.4.3.0-10450-RHEL8.x86_64.rpm
        ├── ltfs-admin-utils-devel-2.4.3.0-10450-RHEL8.x86_64.rpm
        ├── ltfsle-2.4.3.0-10450-RHEL8.x86_64.rpm
        ├── ltfsle-library-2.4.3.0-10450-RHEL8.x86_64.rpm
        └── ltfs-license-2.1.0-RHEL8.x86_64.rpm

6 directories, 37 files
[root@t4ccloud t4c_image]# 
```


## 4. 带库驱动安装


* 依赖python3-pyxattr安装

[root@t4ccloud t4c_image]# yum install python3-pyxattr-0.5.3-18.el8.x86_64.rpm


```
[root@t4ccloud t4c_image]# yum install python3-pyxattr-0.5.3-18.el8.x86_64.rpm
Repository extras is listed more than once in the configuration
Repository epel is listed more than once in the configuration
Repository epel-debuginfo is listed more than once in the configuration
Repository epel-source is listed more than once in the configuration
Last metadata expiration check: 0:48:43 ago on Sun 15 Dec 2024 11:40:41 AM CST.
Dependencies resolved.
===============================================================================================================================================
 Package                              Architecture                Version                              Repository                         Size
===============================================================================================================================================
Installing:
 python3-pyxattr                      x86_64                      0.5.3-18.el8                         @commandline                       36 k

Transaction Summary
===============================================================================================================================================
Install  1 Package

Total size: 36 k
Installed size: 64 k
Is this ok [y/N]: y
Downloading Packages:
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                                                                                       1/1 
  Installing       : python3-pyxattr-0.5.3-18.el8.x86_64                                                                                   1/1 
  Running scriptlet: python3-pyxattr-0.5.3-18.el8.x86_64                                                                                   1/1 
  Verifying        : python3-pyxattr-0.5.3-18.el8.x86_64                                                                                   1/1 

Installed:
  python3-pyxattr-0.5.3-18.el8.x86_64                                                                                                          

Complete!
[root@t4ccloud t4c_image]# 
```


* 带库驱动LTFSLE安装软件

[root@t4ccloud RHEL8]# yum install ltfs-admin-utils-2.4.3.0-10450-RHEL8.x86_64.rpm ltfsle-2.4.3.0-10450-RHEL8.x86_64.rpm ltfs-license-2.1.0-RHEL8.x86_64.rpm ltfs-admin-utils-devel-2.4.3.0-10450-RHEL8.x86_64.rpm ltfsle-library-2.4.3.0-10450-RHEL8.x86_64.rpm

```
[root@t4ccloud RHEL8]# ls /root/LTFSLE/rpm.10450/RHEL8
ltfs-admin-utils-2.4.3.0-10450-RHEL8.x86_64.rpm        ltfsle-2.4.3.0-10450-RHEL8.x86_64.rpm          ltfs-license-2.1.0-RHEL8.x86_64.rpm
ltfs-admin-utils-devel-2.4.3.0-10450-RHEL8.x86_64.rpm  ltfsle-library-2.4.3.0-10450-RHEL8.x86_64.rpm
[root@t4ccloud RHEL8]# yum install ltfs-admin-utils-2.4.3.0-10450-RHEL8.x86_64.rpm ltfsle-2.4.3.0-10450-RHEL8.x86_64.rpm ltfs-license-2.1.0-RHEL8.x86_64.rpm ltfs-admin-utils-devel-2.4.3.0-10450-RHEL8.x86_64.rpm ltfsle-library-2.4.3.0-10450-RHEL8.x86_64.rpm
Repository extras is listed more than once in the configuration
Repository epel is listed more than once in the configuration
Repository epel-debuginfo is listed more than once in the configuration
Repository epel-source is listed more than once in the configuration
Last metadata expiration check: 0:55:41 ago on Sun 15 Dec 2024 11:40:41 AM CST.
Dependencies resolved.
===============================================================================================================================================
 Package                                     Architecture            Version                               Repository                     Size
===============================================================================================================================================
Installing:
 ltfs-admin-utils                            x86_64                  2.4.3.0-10450                         @commandline                  372 k
 ltfs-admin-utils-devel                      x86_64                  2.4.3.0-10450                         @commandline                  1.0 M
 ltfs-license                                x86_64                  2.1.0-20130412_2702                   @commandline                   11 k
 ltfsle                                      x86_64                  2.4.3.0-10450                         @commandline                  451 k
 ltfsle-library                              x86_64                  2.4.3.0-10450                         @commandline                  444 k
Installing dependencies:
 fuse                                        x86_64                  2.9.7-19.el8                          baseos                         83 k
 fuse-common                                 x86_64                  3.3.0-19.el8                          baseos                         22 k
 libicu                                      x86_64                  60.3-2.el8_1                          baseos                        8.8 M
 mariadb-connector-c                         x86_64                  3.1.11-2.el8_3                        appstream                     200 k
 mariadb-connector-c-config                  noarch                  3.1.11-2.el8_3                        appstream                      15 k
 net-snmp                                    x86_64                  1:5.8-30.el8                          appstream                     368 k
 net-snmp-agent-libs                         x86_64                  1:5.8-30.el8                          appstream                     750 k
 net-snmp-libs                               x86_64                  1:5.8-30.el8                          baseos                        843 k

Transaction Summary
===============================================================================================================================================
Install  13 Packages

......

Installed:
  fuse-2.9.7-19.el8.x86_64                           fuse-common-3.3.0-19.el8.x86_64               libicu-60.3-2.el8_1.x86_64                 
  ltfs-admin-utils-2.4.3.0-10450.x86_64              ltfs-admin-utils-devel-2.4.3.0-10450.x86_64   ltfs-license-2.1.0-20130412_2702.x86_64    
  ltfsle-2.4.3.0-10450.x86_64                        ltfsle-library-2.4.3.0-10450.x86_64           mariadb-connector-c-3.1.11-2.el8_3.x86_64  
  mariadb-connector-c-config-3.1.11-2.el8_3.noarch   net-snmp-1:5.8-30.el8.x86_64                  net-snmp-agent-libs-1:5.8-30.el8.x86_64    
  net-snmp-libs-1:5.8-30.el8.x86_64                 

Complete!
[root@t4ccloud RHEL8]# 
```


## 5 带库驱动配置


### 5.1 带库设备连接查看

** 带库驱动配置前提是服务器和带库设备连接，带机可以FC光纤线直接连接服务器HBA卡，也可以通过交换机来连接。 **

* 查看服务器SCSI设备
[root@t4ccloud ~]# lsscsi -g

```
[root@t4ccloud ~]# lsscsi -g
[0:0:0:0]    disk    VMware   Virtual disk     1.0   /dev/sda   /dev/sg0
[0:0:1:0]    disk    VMware   Virtual disk     1.0   /dev/sdb   /dev/sg1
[0:0:2:0]    disk    VMware   Virtual disk     1.0   /dev/sdc   /dev/sg2
[2:0:0:0]    cd/dvd  NECVMWar VMware IDE CDR10 1.00  /dev/sr0   /dev/sg3
[3:0:0:0]    tape    IBM      ULT3580-TD9      PA60  /dev/st0   /dev/sg4
[3:0:0:1]    mediumx IBM      03584L32         1B00  /dev/sch0  /dev/sg5
[4:0:0:0]    tape    IBM      ULT3580-TD9      P370  /dev/st1   /dev/sg6
[4:0:0:1]    mediumx IBM      03584L32         1B00  /dev/sch1  /dev/sg7
[root@t4ccloud ~]# 
```

* 查看带库带机连接
[root@t4ccloud ~]# /opt/ibm/ltfsle/bin/ltfs -o device_list

```
[root@t4ccloud ~]# /opt/ibm/ltfsle/bin/ltfs -o device_list
b67 LTFS14000I LTFS starting, LTFS version 2.4.3.0 (10450), log level 2.
b67 LTFS14058I LTFS Format Specification version 2.4.0.
b67 LTFS14104I Launched by "/opt/ibm/ltfsle/bin/ltfs -o device_list".
b67 LTFS14105I This binary is built for Linux (x86_64).
b67 LTFS14106I GCC version is 4.8.3 20140911 (Red Hat 4.8.3-9).
b67 LTFS17087I Kernel version: Linux version 3.10.0-1160.el7.x86_64 (mockbuild@x86-vm-26.build.eng.bos.redhat.com) (gcc version 4.8.5 20150623 (Red Hat 4.8.5-39) (GCC) ) #1 SMP Tue Aug 18 14:50:17 EDT 2020 i386.
b67 LTFS17089I Distribution: NAME="Red Hat Enterprise Linux Server".
b67 LTFS17089I Distribution: Red Hat Enterprise Linux Server release 7.9 (Maipo).
b67 LTFS17089I Distribution: Red Hat Enterprise Linux Server release 7.9 (Maipo).
b67 LTFS17085I Plugin: Loading "sg" changer backend.
b67 LTFS17085I Plugin: Loading "sg" tape backend.
Changer Device list:.
Device Name = /dev/sg7 (4.0.0.1), Vendor ID = IBM    , Product ID = 03584L32       , Serial Number = 0000078xxxx50407, Product Name = TS3500/TS4500.
Device Name = /dev/sg5 (3.0.0.1), Vendor ID = IBM    , Product ID = 03584L32       , Serial Number = 0000078xxxx50407, Product Name = TS3500/TS4500.
Tape Device list:.
Device Name = /dev/sg6 (4.0.0.0), Vendor ID = IBM     , Product ID = ULT3580-TD9     , Serial Number = 00078xxxxB, Product Name =[ULT3580-TD9].
Device Name = /dev/sg4 (3.0.0.0), Vendor ID = IBM     , Product ID = ULT3580-TD9     , Serial Number = 00078xxxxB, Product Name =[ULT3580-TD9].

[root@t4ccloud ~]# 
```


### 5.2 带库驱动配置


* 带库驱动连接带库配置

[root@t4ccloud ~]# /opt/ibm/ltfsle/bin/ltfs -o changer_devname=/dev/sg5 /ltfs

```
[root@install-machine ~]# /opt/ibm/ltfsle/bin/ltfs -o changer_devname=/dev/sg5 /ltfs
cb4 LTFS14000I LTFS starting, LTFS version 2.4.3.0 (10450), log level 2.
cb4 LTFS14058I LTFS Format Specification version 2.4.0.
cb4 LTFS14104I Launched by "/opt/ibm/ltfsle/bin/ltfs -o changer_devname=/dev/sg5 /ltfs".
cb4 LTFS14105I This binary is built for Linux (x86_64).
cb4 LTFS14106I GCC version is 4.8.3 20140911 (Red Hat 4.8.3-9).
cb4 LTFS17087I Kernel version: Linux version 3.10.0-1160.el7.x86_64 (mockbuild@x86-vm-26.build.eng.bos.redhat.com) (gcc version 4.8.5 20150623 (Red Hat 4.8.5-39) (GCC) ) #1 SMP Tue Aug 18 14:50:17 EDT 2020 i386.
cb4 LTFS17089I Distribution: NAME="Red Hat Enterprise Linux Server".
cb4 LTFS17089I Distribution: Red Hat Enterprise Linux Server release 7.9 (Maipo).
cb4 LTFS17089I Distribution: Red Hat Enterprise Linux Server release 7.9 (Maipo).
cb4 LTFS14063I Sync type is "time", Sync time is 300 sec.
cb4 LTFS17085I Plugin: Loading "sg" tape backend.
cb4 LTFS17085I Plugin: Loading "unified" iosched backend.
cb4 LTFS17085I Plugin: Loading "sg" changer backend.
cb4 LTFS17085I Plugin: Loading "ondemand" dcache backend.
cb4 LTFS17085I Plugin: Loading "memory" crepos backend.

......

cb4 LTFS11777I Loaded required volume caches.
cb4 LTFS14708I LTFS admin server version 2 is starting on port 7600.
cb4 LTFS14111I Initial setup completed successfully.
cb4 LTFS14112I Invoke 'mount' command to check the result of final setup.
cb4 LTFS14113I Specified mount point is listed if succeeded. 

```

* 查看驱动程序运行

[root@t4ccloud ~]#  ps -ef | grep ltfs

```
[root@t4ccloud ~]#  ps -ef | grep ltfs
root      3255     1  0 21:57 ?        00:00:01 /opt/ibm/ltfsle/bin/ltfs -o changer_devname=/dev/sg5 /ltfs
root      3618  2622  0 22:02 pts/0    00:00:00 grep --color=auto ltfs

```


* 查看ltfs磁带文件系统挂载点

[root@t4ccloud ~]#  df /ltfs

```
[root@t4ccloud ~]#  df /ltfs
文件系统               1K-块  已用          可用 已用% 挂载点
ltfs:/dev/sg5  2199023255040     0 2199023255040    0% /ltfs

```


## 6. 带库驱动基本使用

带库驱动安装完成后，用户可以使用命令查看带库设备、带机设备、磁带设备等物理设备信息。 

[root@t4ccloud ~]#  /opt/ibm/ltfsle/bin/leadm drive list

```
[root@t4ccloud ~]#  /opt/ibm/ltfsle/bin/leadm drive list
Serial                 Device            Model       Status  Tape
00078xxxxB           /dev/sg6    [ULT3580-TD9]    Available  TST100L8
00078xxxxB           /dev/sg4    [ULT3580-TD9]    Available  -
[root@t4ccloud ~]# 

```






















