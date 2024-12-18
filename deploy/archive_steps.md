# 数据归档配置步骤


## T4C数据归档Agent

调用T4C归档引擎的Agent内容如下：

[root@t4ccloud ~]# cat /opt/Tape4Cloud/bin/ArchiveAgent.sh 

```
[root@t4ccloud ~]# cat /opt/Tape4Cloud/bin/ArchiveAgent.sh

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

## 归档Agent的定期执行

- 设置Agent为可执行脚本

```
[root@t4ccloud ~]# chmod +x /opt/Tape4Cloud/bin/ArchiveAgent.sh
[root@t4ccloud ~]# ls -l  /opt/Tape4Cloud/bin/ArchiveAgent.sh
-rwxr-xr-x 1 root root 737 Dec 15 17:30 /opt/Tape4Cloud/bin/ArchiveAgent.sh
[root@t4ccloud ~]# 
```

- 设置Agent为crontab执行周期为5分钟

```
[root@t4ccloud ~]# crontab -l
*/5 * * * * sh /opt/Tape4Cloud/bin/ArchiveAgent.sh >> /var/log/t4cadmin/archive_agent.log
[root@t4ccloud ~]# 
```
