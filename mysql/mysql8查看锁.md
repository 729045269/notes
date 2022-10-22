### mysql8查看锁

```

# 查询死锁表
select * from performance_schema.data_locks;

# 查询死锁等待时间
select * from performance_schema.data_lock_waits;


```

