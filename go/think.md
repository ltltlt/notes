# some idea

## go build tag

springboot 配置文件写法里有这样两种写法, 一种用yaml, 所有配置都写一个文件里, 然后用profile来区分(不支持变量引用是一大痛点), 另一种是写在不同文件里, 用文件名base的后缀对应profile.

go build tag 大致就相当于第二种, 这样通过冗余增加了清晰

C的宏就相当于第一种, 简短了, 但看起来很乱
