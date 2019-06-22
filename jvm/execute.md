# 执行

## Hotspot JVM 是半编译半解释型的

![execute procedure](execute.png)

[深入浅出 JIT 编译器](https://www.ibm.com/developerworks/cn/java/j-lo-just-in-time/index.html)

## JIT

JIT编译与自适应编译都属于“动态编译”（dynamic compilation），或者叫“运行时编译”的范畴。特点是在程序运行的时候进行编译，而不是在程序开始运行之前就完成了编译；后者也叫做“静态编译”（static compilation）或者AOT编译（ahead-of-time compilation）。

JIT只是hotspot jvm执行字节码的一部分. 探测热点这些都不是jit
