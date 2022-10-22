# mysql8定时备份并清理3天前的备份

如果不导出指定表。指定忽略多个表时，需要重复多次，每次一个表。每个表必须同时指定数据库和表名。例如：--ignore-table=database.table1 --ignore-table=database.table2 …… 

备份命令：

```
#!/bin/bash

# 备份存储路径

backupDir=/home/wwwroot/mysql-backup

# 数据库账户

mysqlUsername=root

# 数据库密码 如果密码有特殊字符（！@#￥%……&），需要换一个没有特殊字符的密码，否则会提示错误：mysqldump: Got error: 1045: Access denied for user 'r'@'localhost' (using password: YES) when trying to connect

mysqlPwd=123456

if [ ! -d $backupDir ]; then

  mkdir $backupDir

  chmod -R 777 $backupDir

fi

time=$(date +%Y%m%d%H%M)

# test_db是数据库

mysqldump test_db -u$mysqlUsername -p$mysqlPwd --ignore-table=test_db.lms_admin_access --ignore-table=test_db.ams_admin_login_log | gzip >$backupDir/test_db_$time.sql.gz

# 删除超过3天的备份

find $backupDir -name "obc_*.sql.gz" -type f -mtime +3 -exec rm {} \; >/dev/null 2>&1
```

