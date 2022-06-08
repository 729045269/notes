### mysql查看数据库所有表的内存大小

```
select TABLE_NAME,concat(round((DATA_LENGTH+INDEX_LENGTH)/1024/1024,2),'MB') as data,TABLE_ROWS from TABLES where TABLE_SCHEMA='数据库名称' order by data desc
```

