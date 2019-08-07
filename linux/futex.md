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

```manual
One use of futexes is for implementing locks.  The state of the  lock  (i.e.,  acquired  or  not
acquired)  can  be  represented  as an atomically accessed flag in shared memory.  In the uncon‐
tended case, a thread can access or modify the lock state with atomic instructions, for  example
atomically  changing  it  from  not  acquired  to  acquired using an atomic compare-and-exchange
instruction.  (Such instructions are performed entirely in user mode, and the  kernel  maintains
no  information  about  the lock state.)  On the other hand, a thread may be unable to acquire a
lock because it is already acquired by another thread.  It then may pass the lock's  flag  as  a
futex word and the value representing the acquired state as the expected value to a futex() wait
operation.  This futex() operation will block if and only if the lock is still  acquired  (i.e.,
the  value  in  the  futex word still matches the "acquired state").  When releasing the lock, a
thread has to first reset the lock state to not acquired and then execute a futex operation that
wakes  threads  blocked  on the lock flag used as a futex word (this can be further optimized to
avoid unnecessary wake-ups).  See futex(7) for more detail on how to use futexes.
```

go 使用futex实现的runtime.mutex

```go
const (
    mutex_unlocked = 0
	mutex_locked   = 1
	mutex_sleeping = 2 // 非常可能有至少一个睡眠线程(调用futex)
)

func lock(l *mutex) {
    // ...

	// Speculative grab for lock.
	// 尝试将key改为锁定状态, 如果原状态是未锁定, 则表示此线程得到了锁, 直接返回即可
	v := atomic.Xchg(key32(&l.key), mutex_locked)
	if v == mutex_unlocked {
		return
	}

	// wait is either MUTEX_LOCKED or MUTEX_SLEEPING
	// depending on whether there is a thread sleeping
	// on this mutex. If we ever change l->key from
	// MUTEX_SLEEPING to some other value, we must be
	// careful to change it back to MUTEX_SLEEPING before
	// returning, to ensure that the sleeping thread gets
	// its wakeup call.
	wait := v

	// On uniprocessors, no point spinning.
	// On multiprocessors, spin for ACTIVE_SPIN attempts.
	// 单处理器时, 不会自旋; 多处理器会自选active_spin次
	spin := 0
	if ncpu > 1 {
		spin = active_spin
	}
	for {
        // code to spin some times ...

		// Sleep.
		v = atomic.Xchg(key32(&l.key), mutex_sleeping)
		if v == mutex_unlocked {
			return
		}
		wait = mutex_sleeping
		// 只有状态为mutex_sleeping时才被内核阻塞
		futexsleep(key32(&l.key), mutex_sleeping, -1) // 可能被错误唤醒, 所以要在循环里
	}
}

func unlock(l *mutex) {
	v := atomic.Xchg(key32(&l.key), mutex_unlocked)
	if v == mutex_unlocked {
		throw("unlock of unlocked lock")
	}
	// 只有原状态为mutex_sleeping时才唤醒一个线程
	if v == mutex_sleeping {
		futexwakeup(key32(&l.key), 1)
    }
    
    // ...
}
```

## reference link

- [linux futex浅析](https://yq.aliyun.com/articles/6043)
