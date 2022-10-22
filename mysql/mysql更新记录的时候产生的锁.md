作者：浮生若梦
链接：https://www.zhihu.com/question/312483079/answer/2550637461
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。



**面试官：** 看你简历上面写着精通MySQL，我问你一个MySQL锁相关的问题，你看一下这条SQL会对哪些数据加锁？

```text
update user set name='一灯' where age=5;
```

表结构是这样的：

```text
CREATE TABLE `user` (
  `id` int NOT NULL AUTO_INCREMENT COMMENT '主键',
  `name` varchar(255) DEFAULT NULL COMMENT '姓名',
  `age` int DEFAULT NULL COMMENT '年龄',
  PRIMARY KEY (`id`),
  KEY `idx_age` (`age`)
) ENGINE=InnoDB COMMENT='用户表';
```

**我：** age是非唯一性索引，MySQL的锁是加在索引上面的，应该只会对age=10的数据加锁。

**面试官：** 确定吗？

**我：** 嗯...，应该是的。

**面试官：** 【嘲讽】，这就是你精通MySQL的水平吗？今天面试就先到这里吧，后面有消息会主动联系你。

> 后面还可能有消息吗？你们啥时候主动联系过我？
> 实话实说的被拒，[八股文](https://www.zhihu.com/search?q=八股文&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A2550637461})背得溜反而被录取。
> 好吧，等我看看一灯怎么总结的MySQL的八股文。

**我：** 这条SQL具体对哪些数据加锁，还需要看表中有哪些数据。

MySQL有三种类型的行锁：

**[记录锁](https://www.zhihu.com/search?q=记录锁&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A2550637461})（Record Locks）：**

即对某条记录加锁。

```text
# 对id=1的用户加锁
update user set age=age+1 where id=1;
```

**间隙锁（Gap Locks）：**

即对某个范围加锁，但是不包含范围的临界数据。

```text
# 对id大于1并且小于10的用户加锁
update user set age=age+1 where id>1 and id<10;
```

上面SQL的加锁范围是(1,10)。

**临键锁（Next-Key Locks）：**

由记录锁和间隙锁组成，既包含记录本身又包含范围，左开右闭区间。

```text
# 对id大于1并且小于等于10的用户加锁
update user set age=age+1 where id>1 and id<=10;
```

假如表中只有这样两条数据的话：



| id   | name | age  |
| ---- | ---- | ---- |
| 1    | 张三 | 1    |
| 10   | 李四 | 10   |



针对age索引，很产生这样三个索引范围：

> (-∞,1]，(1,10]，(10,+∞)

刚才的这条SQL：

```text
update user set name='一灯' where age=5;
```

由于表中不存在age=5的记录，并且age=5刚好落在 **(1,10]** 的区间范围内，所以会对 **(1,10]** 的范围加锁。

我们可以用实际数据测试一下：



![img](https://pic1.zhimg.com/50/v2-f023d2275b0752a3587e93c266834e12_720w.jpg?source=1940ef5c)![img](https://pic1.zhimg.com/80/v2-f023d2275b0752a3587e93c266834e12_720w.jpg?source=1940ef5c)



当我们执行update语句的时候，age=2和age=8的数据范围都被加锁了。

**面试官：** 小伙子回答的不错啊。如果已经存在age=5的数据，刚才的那条update语句会对哪些数据加锁？

**我：** 假如表中数据是这样的。



| id   | name     | age  |
| ---- | -------- | ---- |
| 1    | 张三     | 1    |
| 5    | 一灯架构 | 5    |
| 10   | 李四     | 10   |



针对age索引，很产生这样四个索引范围：

> (-∞,1]，(1,5]，(5,10]，(10,+∞)

刚才的这条SQL：

```text
update user set name='一灯' where age=5;
```

age=5的数据落在 **(1,5]** 的区间范围内，所以会对 **(1,5]** 的范围加锁。

你以为这就完了吗？MySQL锁为了保证数据的安全性，还会向右遍历到不满足条件为止，还会再加一个间隙锁，也就是 **(5,10]** 的范围。

所以，这条SQL的加锁返回是 **(1,5]** 和 **(5,10]** 。

跟刚才age=5不存在的加锁范围 **(1,10]** 是一样的。不信可以再用刚才的测试用例跑一遍。



![img](https://pic1.zhimg.com/50/v2-f023d2275b0752a3587e93c266834e12_720w.jpg?source=1940ef5c)![img](https://pic1.zhimg.com/80/v2-f023d2275b0752a3587e93c266834e12_720w.jpg?source=1940ef5c)



**面试官：** 小伙子有点东西。如果我把SQL中where条件换成主键ID，加锁范围是什么样的？

```text
update user set name='一灯' where id=5;
```

**我：** 由于锁是加在索引上面的。

如果不存在id=5的数据，加锁范围跟上条SQL是一样的， **(1,10]** 。

如果存在id=5的数据，MySQL的 **Next-Key Locks** 会退化成 **Record Locks** ，也就是只在id=5的这一行记录上加锁。

**面试官：** 小伙子，升级加薪的机会就是留给你这样的人。薪资double，明天就来上班吧。

**知识点总结：**

1. MySQL锁是加在索引记录上面的。
2. 如果是非唯一性索引，不论表中是否存在该记录，除了会对该记录所在范围加锁，还会向右遍历到不满足条件的范围进行加锁。
3. 如果是唯一索引，如果表中存在该记录，只对该行记录加锁。如果表中不存在该记录，除了会对该记录所在范围加锁，还会向右遍历到不满足条件的范围进行加锁。