virtual memory

init 48M memory 9700 9704, 9705, 9706

insert into zset
redis version 5.0.5

first insert 10,000 elements cost 0.44s, each 4.41e-05, memory -> 51M
insert 100,000 elements cost 3.5229, each 3.5229e-05, memory -> 61M
insert 1000,000 elements cost 35.36s, each 3.546e-05, memory -> 190M, cpu 30%
insert 10,000,000 elements cost 369s, each 3.69e-05, memory -> 2046M cpu 30%
insert 30,000,000 elements cost 1094s, each 3.64e-05, memory -> 6152M cpu 30%
conclusion 100,000,000 elements will cost 20+G memory
dump.rdb is compressed, which only cost 600+M disk space
