# println与fmt.Println的区别

fmt.Println是调用fmt的包里的单独实现, 这个包和用户单独能实现的效果一样, 结果会先往一个buffer里写, 之后将整个[]byte写到File中,
是否线程安全取决于对File写一个[]byte是否线程安全, File应该直接调syscall来写, 而linux的write是线程安全的, 所以fmt.Println一般是并发安全的,
但毕竟文档中没有保证, 可能有些系统的write是非并发安全的, 所以使用时需要保证这是非并发安全的

println会被编译器改写为对runtime/print.go下的printlock, print*, printsp, printnl, printunlock的调用
由于在runtime包里, 可以访问一些底层的结构, 输出的内容不太友好, 应该也是线程安全的

## 关于fmt.FPrint*线程安全的讨论可见

- <https://github.com/golang/go/issues/398>
- <https://stackoverflow.com/questions/14694088/is-it-safe-for-more-than-one-goroutine-to-print-to-stdout>
