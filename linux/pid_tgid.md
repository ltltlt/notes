# pid and tgid

pid is process id, all thread have same pid

from linux kernel view, each thread(task) in fact have different pid, but may share same tgid(task group id)

the `getpid` system call in fact return tgid

the `gettid` system call in fact return kernel view pid

the `top` command shows all process by default(in the pid column, it is in fact tgid), you can press `h` to switch to view all thread

the `htop` by default shows all thread

## related link

- [see details of all threads](https://unix.stackexchange.com/questions/892/is-there-a-way-to-see-details-of-all-the-threads-that-a-process-has-in-linux)
- [how to identify thread](https://stackoverflow.com/questions/9305992/if-threads-share-the-same-pid-how-can-they-be-identified)
