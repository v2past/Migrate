# 参考链接
- [percona_xtrabackup](https://docs.percona.com/percona-xtrabackup/8.0/server-backup-version-comparison.html)
- [多云迁移服务商](https://console.squids.cn/rds/database/object-storage/create?resource=access-key)
- 


如何备份MySQL到对象存储
准备工作
你可以使用Percona XtraBackup工具备份到MySQL并上传至S3/OSS
安装XtraBackup之前请按照下表确定工具版本，版本不匹配可能导致备份失败

Percona XtraBackup 8.x	MySQL 8.0
Percona XtraBackup >= 2.4.23	MySQL 5.6/5.7
安装工具
wget https://www.percona.com/downloads/XtraBackup/Percona-XtraBackup-8.0.4/binary/redhat/7/x86_64/percona-xtrabackup-80-8.0.4-1.el7.x86_64.rpm
yum localinstall percona-xtrabackup-80-8.0.4-1.el7.x86_64.rpm
备份MySQL到对象存储
xtrabackup --user=root --password=password --backup --stream=xbstream --extra-lsndir=/tmp --target-dir=/tmp | \
xbcloud put --storage=s3 \
--s3-region=ap-east-1 \
--s3-endpoint='s3.ap-east-1.amazonaws.com' \
--s3-access-key='YOUR-ACCESSKEYID' \
--s3-secret-key='YOUR-SECRETACCESSKEY' \
--s3-bucket='mysql_backups' \
--parallel=4 \
$(date -I)-full_backup
