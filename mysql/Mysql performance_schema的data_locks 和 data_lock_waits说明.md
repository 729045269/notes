# Mysql performance_schema的data_locks 和 data_lock_waits说明



#### data_locks

- 表结构

| Field                 | Type            | Null | Key  | Default | Extra |
| --------------------- | --------------- | ---- | ---- | ------- | ----- |
| ENGINE                | varchar(32)     | NO   | PRI  | NULL    |       |
| ENGINE_LOCK_ID        | varchar(128)    | NO   | PRI  | NULL    |       |
| ENGINE_TRANSACTION_ID | bigint unsigned | YES  | MUL  | NULL    |       |
| THREAD_ID             | bigint unsigned | YES  | MUL  | NULL    |       |
| EVENT_ID              | bigint unsigned | YES  |      | NULL    |       |
| OBJECT_SCHEMA         | varchar(64)     | YES  | MUL  | NULL    |       |
| OBJECT_NAME           | varchar(64)     | YES  |      | NULL    |       |
| PARTITION_NAME        | varchar(64)     | YES  |      | NULL    |       |
| SUBPARTITION_NAME     | varchar(64)     | YES  |      | NULL    |       |
| INDEX_NAME            | varchar(64)     | YES  |      | NULL    |       |
| OBJECT_INSTANCE_BEGIN | bigint unsigned | NO   |      | NULL    |       |
| LOCK_TYPE             | varchar(32)     | NO   |      | NULL    |       |
| LOCK_MODE             | varchar(32)     | NO   |      | NULL    |       |
| LOCK_STATUS           | varchar(32)     | NO   |      | NULL    |       |
| LOCK_DATA             | varchar(8192)   | YES  |      | NULL    |       |

- ENGINE: 存储引擎（INNODB）
- ENGINE_LOCK_ID  存储引擎内部的锁ID，该值会发生动态变化，外部系统不应该依赖该值
- ENGINE_TRANSACTION_ID:  2578 (`information_schema.innodb_trx`中的`trx_id`)
- THREAD_ID: 持有锁的线程ID
- EVENT_ID: 29
- OBJECT_SCHEMA:  数据库名（lock_test）
- OBJECT_NAME: 表名（first_table）
- PARTITION_NAME:  分区名
- SUBPARTITION_NAME:  子分区名
- INDEX_NAME: 索引名
- OBJECT_INSTANCE_BEGIN: 锁的内存空间起始地址（140373282389696）
- LOCK_TYPE: 锁类型（TABLE/RECORD）
- LOCK_MODE: 锁模式（`IX`: 表意向排它锁， `X`： NextKey-Lock, `X, REC_NOT_GAP`: 行锁， `X,GAP`: 间隙锁，LOCK_INSERT_INTENTION插入意向锁）
- LOCK_STATUS: `GRANTED`、`WAITING`
- LOCK_DATA： 锁的数据，当LOCK_TYPE为RECORD时才会有值（如果是聚族索引则直接显示主键，如果是非聚族索引则是，当前数据以及主键数据）

