# futex(Fast User-space muTEXes)

快速用户空间锁

在futex诞生前, linux同步机制分用户态同步机制和内核态同步机制.

## 用户态同步机制利用原子指令实现的spinlock(自旋锁)

```go
func trylock(val *int) bool {
    return cas(val, 0, 1)
}
func lock(val *int) {
    for trylock(val) {}
}
```

好处是不用切换到内核态, 减少了开销, 坏处是线程是忙等的, 占用处理器资源

## 内核态同步机制

semaphore等. 好处是不占用处理器资源. 坏处是每次调用都是一次系统调用.

## 理想情况

在无冲突的时候用户态原子指令就能解决, 有冲突才进入内核态, 让内核挂起线程

## reference link

- [linux futex浅析](https://yq.aliyun.com/articles/6043)
