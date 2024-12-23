# NFS Ganesha配置步骤

**Ganesha构规划**

设计Ganesha磁带归档时，需要确定整个环境。 

[开源NFS Ganesha参考](https://github.com/nfs-ganesha/nfs-ganesha)


## 1. Ganesha 安装依赖包

安装依赖包 

```
yum install gcc git cmake autoconf libtool bison flex
yum install libgssglue-devel openssl-devel nfs-utils-lib-devel doxygen redhat-lsb gcc-c++
yum -y install libcephfs-devel.x86_64 librgw-devel.x86_64 libuuid-devel userspace-rcu-devel

```

依赖包安装结果

```
[root@install-machine ~]# yum install gcc git cmake autoconf libtool bison flex
Loaded plugins: langpacks, product-id, search-disabled-repos, subscription-manager

This system is not registered with an entitlement server. You can use subscription-manager to register.

Repository epel is listed more than once in the configuration
Repository epel-debuginfo is listed more than once in the configuration
Repository epel-source is listed more than once in the configuration
Repository base is listed more than once in the configuration
Package gcc-4.8.5-44.el7.x86_64 already installed and latest version
Package git-1.8.3.1-25.el7_9.x86_64 already installed and latest version
Package cmake-2.8.12.2-2.el7.x86_64 already installed and latest version
Package autoconf-2.69-11.el7.noarch already installed and latest version
Package libtool-2.4.2-22.el7_3.x86_64 already installed and latest version
Package bison-3.0.4-2.el7.x86_64 already installed and latest version
Package flex-2.5.37-6.el7.x86_64 already installed and latest version
Nothing to do
[root@install-machine ~]# 

[root@install-machine ~]# yum install libgssglue-devel openssl-devel nfs-utils-lib-devel doxygen redhat-lsb gcc-c++
Loaded plugins: langpacks, product-id, search-disabled-repos, subscription-manager

This system is not registered with an entitlement server. You can use subscription-manager to register.

Repository epel is listed more than once in the configuration
Repository epel-debuginfo is listed more than once in the configuration
Repository epel-source is listed more than once in the configuration
Repository base is listed more than once in the configuration

。。。

Installed:
  redhat-lsb.x86_64 0:4.1-27.el7.centos.1                                                                                                      

Dependency Installed:
  foomatic-filters.x86_64 0:4.0.9-10.el7_9                              gdbm-devel.x86_64 0:1.10-8.el7                                         
  libdb-devel.x86_64 0:5.3.21-25.el7                                    libmng.x86_64 0:1.0.10-14.el7                                          
  mailcap.noarch 0:2.1.41-2.el7                                         mesa-libGLU.x86_64 0:9.0.0-4.el7                                       
  perl-B-Lint.noarch 0:1.17-3.el7                                       perl-Business-ISBN.noarch 0:2.06-2.el7                                 
  perl-Business-ISBN-Data.noarch 0:20120719.001-2.el7                   perl-CGI.noarch 0:3.63-4.el7                                           
  perl-CPAN.noarch 0:1.9800-299.el7_9                                   perl-Class-ISA.noarch 0:0.36-1010.el7                                  
  perl-Digest-SHA.x86_64 1:5.85-4.el7                                   perl-Encode-Locale.noarch 0:1.03-5.el7                                 
  perl-Env.noarch 0:1.04-2.el7                                          perl-ExtUtils-Install.noarch 0:1.58-299.el7_9                          
  perl-ExtUtils-MakeMaker.noarch 0:6.68-3.el7                           perl-ExtUtils-Manifest.noarch 0:1.61-244.el7                           
  perl-ExtUtils-ParseXS.noarch 1:3.18-3.el7                             perl-FCGI.x86_64 1:0.74-8.el7                                          
  perl-File-CheckTree.noarch 0:4.42-3.el7                               perl-File-Listing.noarch 0:6.04-7.el7                                  
  perl-HTML-Parser.x86_64 0:3.71-4.el7                                  perl-HTML-Tagset.noarch 0:3.20-15.el7                                  
  perl-HTTP-Cookies.noarch 0:6.01-5.el7                                 perl-HTTP-Daemon.noarch 0:6.01-8.el7                                   
  perl-HTTP-Date.noarch 0:6.02-8.el7                                    perl-HTTP-Message.noarch 0:6.06-6.el7                                  
  perl-HTTP-Negotiate.noarch 0:6.01-5.el7                               perl-IO-HTML.noarch 0:1.00-2.el7                                       
  perl-IO-Socket-IP.noarch 0:0.21-5.el7                                 perl-IO-Socket-SSL.noarch 0:1.94-7.el7                                 
  perl-LWP-MediaTypes.noarch 0:6.02-2.el7                               perl-Locale-Codes.noarch 0:3.26-2.el7                                  
  perl-Locale-Maketext.noarch 0:1.23-3.el7                              perl-Module-Pluggable.noarch 1:4.8-3.el7                               
  perl-Mozilla-CA.noarch 0:20130114-5.el7                               perl-Net-HTTP.noarch 0:6.06-2.el7                                      
  perl-Net-LibIDN.x86_64 0:0.12-15.el7                                  perl-Net-SSLeay.x86_64 0:1.55-6.el7                                    
  perl-Pod-Checker.noarch 0:1.60-2.el7                                  perl-Pod-LaTeX.noarch 0:0.61-2.el7                                     
  perl-Pod-Parser.noarch 0:1.61-2.el7                                   perl-Pod-Plainer.noarch 0:1.03-4.el7                                   
  perl-Sys-Syslog.x86_64 0:0.33-3.el7                                   perl-Test-Simple.noarch 0:0.98-243.el7                                 
  perl-Text-Soundex.x86_64 0:3.04-4.el7                                 perl-Text-Unidecode.noarch 0:0.04-20.el7                               
  perl-TimeDate.noarch 1:2.30-2.el7                                     perl-URI.noarch 0:1.60-9.el7                                           
  perl-WWW-RobotRules.noarch 0:6.02-5.el7                               perl-XML-LibXML.x86_64 1:2.0018-5.el7                                  
  perl-XML-NamespaceSupport.noarch 0:1.11-10.el7                        perl-XML-SAX.noarch 0:0.99-9.el7                                       
  perl-XML-SAX-Base.noarch 0:1.08-7.el7                                 perl-autodie.noarch 0:2.16-2.el7                                       
  perl-devel.x86_64 4:5.16.3-299.el7_9                                  perl-libwww-perl.noarch 0:6.05-2.el7                                   
  perl-local-lib.noarch 0:1.008010-4.el7                                pyparsing.noarch 0:1.5.6-9.el7                                         
  qt.x86_64 1:4.8.7-9.el7_9                                             qt-settings.noarch 0:19-23.12.el7.centos                               
  qt-x11.x86_64 1:4.8.7-9.el7_9                                         qt3.x86_64 0:3.3.8b-51.el7                                             
  redhat-lsb-core.x86_64 0:4.1-27.el7.centos.1                          redhat-lsb-cxx.x86_64 0:4.1-27.el7.centos.1                            
  redhat-lsb-desktop.x86_64 0:4.1-27.el7.centos.1                       redhat-lsb-languages.x86_64 0:4.1-27.el7.centos.1                      
  redhat-lsb-printing.x86_64 0:4.1-27.el7.centos.1                      redhat-lsb-submod-multimedia.x86_64 0:4.1-27.el7.centos.1              
  redhat-lsb-submod-security.x86_64 0:4.1-27.el7.centos.1               spax.x86_64 0:1.5.2-13.el7                                             
  systemtap-sdt-devel.x86_64 0:4.0-13.el7                              

Complete!
[root@install-machine ~]# 
[root@install-machine ~]# 
[root@install-machine ~]# 
[root@install-machine ~]# yum -y install libcephfs-devel.x86_64 librgw-devel.x86_64 libuuid-devel userspace-rcu-devel
Loaded plugins: langpacks, product-id, search-disabled-repos, subscription-manager

This system is not registered with an entitlement server. You can use subscription-manager to register.

Repository epel is listed more than once in the configuration
Repository epel-debuginfo is listed more than once in the configuration
Repository epel-source is listed more than once in the configuration
Repository base is listed more than once in the configuration
No package libcephfs-devel.x86_64 available.
No package librgw-devel.x86_64 available.
Package libuuid-devel-2.23.2-65.el7_9.1.x86_64 already installed and latest version
Resolving Dependencies
--> Running transaction check
---> Package userspace-rcu-devel.x86_64 0:0.7.16-1.el7 will be installed
--> Processing Dependency: userspace-rcu(x86-64) = 0.7.16-1.el7 for package: userspace-rcu-devel-0.7.16-1.el7.x86_64
--> Processing Dependency: liburcu-bp.so.1()(64bit) for package: userspace-rcu-devel-0.7.16-1.el7.x86_64
--> Processing Dependency: liburcu-cds.so.1()(64bit) for package: userspace-rcu-devel-0.7.16-1.el7.x86_64
--> Processing Dependency: liburcu-common.so.1()(64bit) for package: userspace-rcu-devel-0.7.16-1.el7.x86_64
--> Processing Dependency: liburcu-mb.so.1()(64bit) for package: userspace-rcu-devel-0.7.16-1.el7.x86_64
--> Processing Dependency: liburcu-qsbr.so.1()(64bit) for package: userspace-rcu-devel-0.7.16-1.el7.x86_64
--> Processing Dependency: liburcu-signal.so.1()(64bit) for package: userspace-rcu-devel-0.7.16-1.el7.x86_64
--> Processing Dependency: liburcu.so.1()(64bit) for package: userspace-rcu-devel-0.7.16-1.el7.x86_64
--> Running transaction check
---> Package userspace-rcu.x86_64 0:0.7.16-1.el7 will be installed
--> Finished Dependency Resolution

Dependencies Resolved

===============================================================================================================================================
 Package                                   Arch                         Version                               Repository                  Size
===============================================================================================================================================
Installing:
 userspace-rcu-devel                       x86_64                       0.7.16-1.el7                          epel                        48 k
Installing for dependencies:
 userspace-rcu                             x86_64                       0.7.16-1.el7                          epel                        73 k

Transaction Summary
===============================================================================================================================================
Install  1 Package (+1 Dependent package)

Total download size: 121 k
Installed size: 454 k
Downloading packages:
(1/2): userspace-rcu-devel-0.7.16-1.el7.x86_64.rpm                                                                      |  48 kB  00:00:00     
(2/2): userspace-rcu-0.7.16-1.el7.x86_64.rpm                                                                            |  73 kB  00:00:00     
-----------------------------------------------------------------------------------------------------------------------------------------------
Total                                                                                                          344 kB/s | 121 kB  00:00:00     
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Installing : userspace-rcu-0.7.16-1.el7.x86_64                                                                                           1/2 
  Installing : userspace-rcu-devel-0.7.16-1.el7.x86_64                                                                                     2/2 
  Verifying  : userspace-rcu-devel-0.7.16-1.el7.x86_64                                                                                     1/2 
  Verifying  : userspace-rcu-0.7.16-1.el7.x86_64                                                                                           2/2 

Installed:
  userspace-rcu-devel.x86_64 0:0.7.16-1.el7                                                                                                    

Dependency Installed:
  userspace-rcu.x86_64 0:0.7.16-1.el7                                                                                                          

Complete!
[root@install-machine ~]# 

```

## 2. Github克隆Ganesha代码

下载代码： wget https://github.com/nfs-ganesha/nfs-ganesha/archive/refs/heads/next.zip

```
[root@install-machine nfs-ganesha]# wget https://github.com/nfs-ganesha/nfs-ganesha/archive/refs/heads/next.zip
--2024-12-23 18:37:23--  https://github.com/nfs-ganesha/nfs-ganesha/archive/refs/heads/next.zip
Resolving github.com (github.com)... 20.205.243.166
Connecting to github.com (github.com)|20.205.243.166|:443... connected.
HTTP request sent, awaiting response... 302 Found
Location: https://codeload.github.com/nfs-ganesha/nfs-ganesha/zip/refs/heads/next [following]
--2024-12-23 18:37:24--  https://codeload.github.com/nfs-ganesha/nfs-ganesha/zip/refs/heads/next
Resolving codeload.github.com (codeload.github.com)... 20.205.243.165
Connecting to codeload.github.com (codeload.github.com)|20.205.243.165|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: unspecified [application/zip]
Saving to: ‘next.zip’

    [                 <=>                                                                                  ] 2,947,244    903KB/s   in 3.3s   

2024-12-23 18:37:28 (864 KB/s) - ‘next.zip’ saved [2947244]

[root@install-machine nfs-ganesha]# 
[root@install-machine nfs-ganesha]# unzip next.zip 
Archive:  next.zip
178e02de97b9c29a4643ac0191a2be84ef2aec95
   creating: nfs-ganesha-next/
  inflating: nfs-ganesha-next/.git-blame-ignore-revs  
 。。。。。。
   
   
```
