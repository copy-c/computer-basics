# zookeeper

## 1.特点

1.数据保存在内存中

## 2.ZAB协议 -> 支持崩溃恢复的原子广播协议

### 1.角色

leader + follower  

### 2.规则

1.假设有一个事务proposal在任何一台上已经写入成功，那么应该在所有的机器上都保证成功，因此新选的leader一定需要有最大的zxid(事务ID)

### 3.流程

#### 1.崩溃恢复

1）选举时需要ZXID最大原因是：新选出来的leader可以不用检查proposql的提交和检查工作  
2) 数据同步完成后，leader会将自身提交的最大的ZXID事务发送给follower，follower会根据leader消息进行回退或更新

#### 2.消息广播

1）由leader接受客户端的值，生成一个proposql提案，并为proposql生成一个全局唯一ID，即ZXID  
2）leader与每个follower之间都有一个队列，leader将消息发送到该队列  
3）follower从队列拿出消息处理完后，向leader发送ack确认  
4）leader收到半数以上的ack后，向所有follower发送commit

类似两段提交，都交给leader处理，再给follower  

## 3.选举

1）首先每个server都会选自己为leader，并将票广播出去(myid, zxid)  
2）server收到其他票后与自己的票作对比，先比较zxid，再比较myid  
3）server选出较大的票后修改自己的票重新广播  
4）再次统计票数，当收到的某一台的票数超过一半，那么此台就被认定为leader  

## 4.脑裂问题解决

- 通过过半机制，只有一个leader或者没有leader
- A（3台） + B（3台） -> 脑裂后，A和B都不能过半，因此没有leader
- A（3台） + B（2台） -> 脑裂后，只有A能选出leader