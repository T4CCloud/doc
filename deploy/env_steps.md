# 磁带存储节点环境配置步骤


## 1. 服务器基本配置

### 1.1 配置hosts文件
```
[root@t4ccloud ~]# cat  /etc/hosts   
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4   
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6   
10x.4x.62.1xx t4ccloud   
```

## 2. 依赖包安装
### 2.1 查看操作系统版本
```
[root@t4ccloud yum.repos.d]# cat /etc/redhat-release
CentOS release 7
[root@t4ccloud yum.repos.d]# 
```
### 2.1 配置T4C 安装yum base源
```
[root@t4ccloud yum.repos.d]# cat t4c_base.repo  

[base]
name=CentOS-$releasever - Base - mirrors.aliyun.com 
failovermethod=priority 
baseurl=https://mirrors.aliyun.com/centos-vault/7.9.2009/os/$basearch/ 
gpgcheck=1
gpgkey=https://mirrors.aliyun.com/centos-vault/RPM-GPG-KEY-CentOS-7
 
#released updates 
[updates]
name=CentOS-$releasever - Updates - mirrors.aliyun.com
failovermethod=priority
baseurl=https://mirrors.aliyun.com/centos-vault/7.9.2009/updates/$basearch/
gpgcheck=1
gpgkey=https://mirrors.aliyun.com/centos-vault/RPM-GPG-KEY-CentOS-7
 
#additional packages that may be useful
[extras]
name=CentOS-$releasever - Extras - mirrors.aliyun.com
failovermethod=priority
baseurl=https://mirrors.aliyun.com/centos-vault/7.9.2009/extras/$basearch/
gpgcheck=1
gpgkey=https://mirrors.aliyun.com/centos-vault/RPM-GPG-KEY-CentOS-7
 
#additional packages that extend functionality of existing packages
[centosplus]
name=CentOS-$releasever - Plus - mirrors.aliyun.com
failovermethod=priority
baseurl=https://mirrors.aliyun.com/centos-vault/7.9.2009/centosplus/$basearch/
gpgcheck=1
enabled=0
gpgkey=https://mirrors.aliyun.com/centos-vault/RPM-GPG-KEY-CentOS-7
 
#contrib - packages by Centos Users
[contrib]
name=CentOS-$releasever - Contrib - mirrors.aliyun.com
failovermethod=priority
baseurl=https://mirrors.aliyun.com/centos-vault/7.9.2009/contrib/$basearch/
gpgcheck=1
enabled=0
gpgkey=https://mirrors.aliyun.com/centos-vault/RPM-GPG-KEY-CentOS-7

[devtoolset-11]
name=DevToolset-11
baseurl=https://mirrors.aliyun.com/centos/7.9.2009/sclo/x86_64/rh/
enabled=1
gpgcheck=0
```
### 2.2 配置T4C 安装yum epel源
```
[root@t4ccloud yum.repos.d]# cat t4c_epel.repo  
[epel]
name=Extra Packages for Enterprise Linux 7 - $basearch
baseurl=http://mirrors.aliyun.com/epel/7/$basearch
failovermethod=priority
enabled=1
gpgcheck=0
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-7
 
[epel-debuginfo]
name=Extra Packages for Enterprise Linux 7 - $basearch - Debug
baseurl=http://mirrors.aliyun.com/epel/7/$basearch/debug
failovermethod=priority
enabled=0
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-7
gpgcheck=0
 
[epel-source]
name=Extra Packages for Enterprise Linux 7 - $basearch - Source
baseurl=http://mirrors.aliyun.com/epel/7/SRPMS
failovermethod=priority
enabled=0
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-7
gpgcheck=0
```


### 2.3 清除旧yum缓存
```
[root@t4ccloud yum.repos.d]# yum clean all
Repository extras is listed more than once in the configuration
282 files removed
[root@t4ccloud yum.repos.d]# 
```

### 2.4 生产新yum缓存
```
[root@t4ccloud yum.repos.d]# yum makecache
Repository extras is listed more than once in the configuration
CentOS Stream 8 - AppStream                                                                            101 MB/s |  29 MB     00:00    
CentOS Stream 8 - BaseOS                                                                                57 MB/s |  10 MB     00:00    
CentOS Stream 8 - Extras                                                                               1.2 MB/s |  18 kB     00:00    
CentOS Stream 8 - Extras common packages                                                               184 kB/s | 8.0 kB     00:00    
Extra Packages for Enterprise Linux 8 - x86_64                                                          82 MB/s |  14 MB     00:00    
Extra Packages for Enterprise Linux Modular 8 - x86_64                                                  14 MB/s | 733 kB     00:00    
CentOS-8 - Base - mirrors.aliyun.com                                                                   1.2 MB/s |  10 MB     00:08    
CentOS-8 - Updates - mirrors.aliyun.com                                                                1.1 MB/s |  34 MB     00:30    
DevToolset-11                                                                                          1.1 MB/s |  18 MB     00:15    
Metadata cache created.
[root@t4ccloud yum.repos.d]# 
```
### 2.5 安装软件依赖包


```
[root@t4ccloud ~]# yum install -y mdadm  openssl*   fuse*   libxml*   libuuid*   libicu*   glibc*  nss-softokn-freebl*  
[root@t4ccloud ~]# yum install -y python*   python3  net-snmp*   lm_sensors-libs*  java   java-1.8.0*   gperftools-libs  
[root@t4ccloud ~]# yum install -y boost-serialization   boost-program-options  glibc-devel  gcc  rpm-build   kernel-devel  
[root@t4ccloud ~]# yum install -y kernel*   dracut*   abrt-addon*   net-tools  m4*  boost-filesystem*  openssh*  ksh*  nc  lsof  
[root@t4ccloud ~]# yum install -y bc attr* rpcbind* boost-thread*   boost-date-time* gcc-c++ 
[root@t4ccloud ~]# yum install -y boost-filesystem protobuf  protobuf-compiler  protobuf-devel  sqlite  sqlite-devel  fuse  fuse-devel  
[root@t4ccloud ~]# yum install -y libmount-devel  libxml2  libxml2-devel  boost-system  boost-thread  boost-system  boost-devel libuuid  libuuid-devel  libmount  

```

## 3. 关闭系统防火墙
```
[root@t4ccloud ~]# systemctl stop  firewalld 
[root@t4ccloud ~]# systemctl disable  firewalld 
[root@t4ccloud ~]# systemctl status firewalld 
● firewalld.service - firewalld - dynamic firewall daemon
   Loaded: loaded (/usr/lib/systemd/system/firewalld.service; disabled; vendor preset: enabled)
   Active: inactive (dead)
     Docs: man:firewalld(1)
[root@t4ccloud ~]# 
```

## 4. 创建缓存文件系统

### 4.1 查看系统硬盘
```
[root@t4ccloud ~]# lsblk
NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
vda    253:0    0   40G  0 disk 
├─vda1 253:1    0  200M  0 part /boot/efi
└─vda2 253:2    0 39.8G  0 part /
vdb    253:16   0   20G  0 disk 
```
### 4.2 格式化数据硬盘
```
[root@t4ccloud ~]# mkfs.xfs /dev/vdb
meta-data=/dev/vdb               isize=512    agcount=4, agsize=1310720 blks
         =                       sectsz=512   attr=2, projid32bit=1
         =                       crc=1        finobt=1, sparse=1, rmapbt=0
         =                       reflink=1    bigtime=0 inobtcount=0
data     =                       bsize=4096   blocks=5242880, imaxpct=25
         =                       sunit=0      swidth=0 blks
naming   =version 2              bsize=4096   ascii-ci=0, ftype=1
log      =internal log           bsize=4096   blocks=2560, version=2
         =                       sectsz=512   sunit=0 blks, lazy-count=1
realtime =none                   extsz=4096   blocks=0, rtextents=0
Discarding blocks...Done.
[root@t4ccloud ~]# 
```
### 4.3 挂载数据盘成缓存文件系统
```
[root@t4ccloud ~]# mkdir /cachefs
[root@t4ccloud ~]# mount /dev/vdb /cachefs
[root@t4ccloud ~]# df -h /cachefs
Filesystem      Size  Used Avail Use% Mounted on
/dev/vdb         20G  175M   20G   1% /cachefs
[root@t4ccloud ~]# 
```




