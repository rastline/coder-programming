http://www.tiantianbianma.com/

## 前言
>你现在的努力，是为了以后有更多的选择。

## 什么是事务？

事务(Transaction)是关系型数据库中，它是我们数据库中非常重要的一个概念。是由一组SQL语句组成的一个程序执行单元(unit);
该执行单元要么成功commit,要么失败rollback。

**执行单元**
- insert into
- delete
- update
- select


## 事务的ACID特性

事务应该具有四个属性：原子性、一致性、隔离性、持久性。这四个属性通常称为ACID特性。
- Atomicity(原子性)
- Consistency(一致性)
- Isolation(隔离性)
- Durability(持久性)

### Atomicity(原子性)
**原子性** 是指事务包含的所有操作要么全部成功，要么全部失败回滚。
比如：我们的`银行转账`，我把钱转给你，银行会把我的钱扣掉，然后把你的钱加上去。不能只做一半，只把我的钱扣掉，你的钱没有加上去。

### Consistency(一致性)
**一致性** 是指逻辑上的一致性，即所有操作是符合现实当中的期望的。
比如：我们的`银行转账`，假设用户A和用户B两者的钱加起来一共是1000，那么不管A和B之间如何转账，转几次账，事务结束后两个用户的钱相加起来应该还得是1000，这就是事务的一致性。
**ps:** `我们通常说的数据库事务的一致性和分布式系统的一致性并不是一个概念。`


### Isolation(隔离性)
**隔离性** 是指多个并发独立事务，相互独立，相互隔离，互不影响
当多个用户并发访问数据库时，比如操作同一张表时，数据库为每一个用户开启的事务，不能被其他事务的操作所干扰，多个并发事务之间要相互隔离
比如：我做一个事务，你做一个事务，这中间是相互独立的，不能相互影响。

### Durability(持久性)
**持久性** 是指一个事务一旦被提交了，那么对数据库中的数据的改变就是永久性的，即便是在数据库系统遇到故障的情况下也不会丢失提交事务的操作。
我将事务做完之后，这个结果是能持久下去的。能一直存下去。不管断电还是其他情况。

我们的关系型数据库都实现了ACID这样的一些事务特性。其中最关键的一点是Isolation。互相不影响。



## 事务的隔离级别


- Read uncommitted
- Read Committed
- Repeatable Reads



Read uncommitted：是隔离级别最低的一种事务级别。在这种隔离级别下，一个事务会读到另一个事务更新后但未提交的数据，如果另一个事务回滚，那么当前事务读到的数据就是脏数据，这就是脏读（Dirty Read）。简单点说：别人事务做到一半还没提交，这时候被我读到的值。

Read Committed：读到的都是别人提交后的值。这种隔离级别下，一个事务可能会遇到不可重复读（Non Repeatable Read）的问题。不可重复读是指，在一个事务内，多次读同一数据，在这个事务还没有结束时，如果另一个事务恰好修改了这个数据，那么，在第一个事务中，两次读取的数据就可能不一致。

Repeatable Reads：这种隔离级别下，一个事务可能会遇到幻读（Phantom Read）的问题。

幻读是指，在一个事务中，第一次查询某条记录，发现没有，但是，当试图更新这条不存在的记录时，竟然能成功，并且，再次读取同一条记录，它就神奇地出现了。

Serializable：是最严格的隔离级别。在Serializable隔离级别下，所有事务按照次序依次执行，因此，脏读、不可重复读、幻读都不会出现。



以下演示：



