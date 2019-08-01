# thread

## interruptable

- 当线程在Thread.sleep时, 可以用thread.interrupt来中断这个线程
- 当线程在sychronized尝试获得锁时, 线程不可被打断, 这会导致一些问题
- j.u.c下面的lock有lockInterruptibly方法就是为了解决这个问题的, 这个方法在尝试获得锁时可以被打断
