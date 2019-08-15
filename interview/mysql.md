## mysql innodb 隔离级别

- uncommitted read(未commit都能看到)
- committed read(commit后就能看到， 即使事务是后启的)
- read reaptable(commit后也看不到， 一个事务里重复读的结果一样), 可能发生幻读(phantom read, 即后起的事务会受到前一个事务的插入更新操作的影响， 虽然看不到， 解决方法是手动加间隙锁)
- 串行(所有操作都加锁， 或者用读写锁， 读会被写阻塞，不过没有任何问题)

## MVCC和乐观锁

innodb中， 两个事务， a先开始， b随后开始， 原先表test中有一行数2; b执行了更新，将2更新为3， a执行select * from .. where col=2能否查到结果?

答案是能的，我尝试用MVCC来解释, 但a的版本号都 < b的版本号, 有可能是在rr中事务只能读到已经提交的事务，但这解释不通为什么会产生幻读, 看来还是提交读比较自然

## 引用

- [mysql 幻读的详解、实例及解决办法](https://segmentfault.com/a/1190000016566788)
