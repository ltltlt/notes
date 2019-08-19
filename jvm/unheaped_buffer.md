# direct buffer and mapped buffer

## direct buffer

不由java堆管理的空间，这块空间由java和操作系统共享, 这样在socket拷贝数据时会很高效

## mapped buffer

由java FileChannel实例使用的内存

文件映射到内存中的一块区域

## reference

- [stackoverflow](https://stackoverflow.com/questions/15657837/what-is-mapped-buffer-pool-direct-buffer-pool-and-how-to-increase-their-size)
