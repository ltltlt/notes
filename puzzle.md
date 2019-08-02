# 仍然困惑

## 指令重排

这个应该是编译器进行的, CPU也会进行, 这很容易导致程序不可靠

目前go 编译器给我的感觉是不太会进行这一优化, 但不确定, 而go没有volatile, 我也不知道有什么read before, write after机制

## redis 哨兵
