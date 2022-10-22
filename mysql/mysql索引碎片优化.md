### Mysql索引碎片的优化

产生碎片的原因

```
由于表上有过度地插入、修改和删除操作,索引页被分成多抉就形成了索引碎片
如果索引碎片严重,那扫描索引的时间就会变长,甚至导致索引不可用,因此数据检索操作就慢了
简单的说，删除数据必然会在数据文件中造成不连续的空白空间，而当插入数据时，这些空白空间则会被利用起来。于是造成了数据的存储位置不连续，以及物理存储顺序与理论上的排序顺序不同，这种是数据碎片。

```

1. 查看碎片

   ```
   SHOW TABLE STATUS LIKE '表名';
   #当Data_free 列值大于0时表示有碎片
   ```

2. 修复方法

   ```
   1. alter table 表名 engine = InnoDB/myisam
   # 例如之前表的引擎是innodb，执行 alter table xxx engine  = InnoDB ，可以起到修复碎片作用的
   
   2.optimize table 表名 
   # 这两种方法都会把所有的数据文件重新整理一遍，使之对齐 这个过程是比较耗资源的，不要频繁操作，可以按月为单位操作,InnoDB：用这个命令会提示
   “Table does not support optimize, doing recreate + analyze instead”，意思就是InnoDB不支持optimize
   ```

   