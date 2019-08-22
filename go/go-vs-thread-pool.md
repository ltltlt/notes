# go vs thread pool

In my mind, go's runtime is basically a thread pool.
Our program runs on this thread pool, we start a job(goroutine) wait for it to finish. The main function is just a job.
Go is actually a n x m thread model, map n go thread(goroutine) with m os thread.
The different between go and java(or other language) with thread pool language is:

- go's "thread pool" is more native, and have support better;
- go's GMP model is more flexible;
- the P helps avoid lots of lock operations;
- G can be preempted(though not that perfect), this is quite different, this is benefit from native thread pool support;
- go support netpool, when a network io happens, system thread will not block, only go thread(goroutine) blocks, this is the reason why go is quite good at network development;
- CSP style concurrency;

Generally, I would say there are no much difference between go runtime and thread pool.

Java native support thread, this is better than C++(before C++11), Go native support thread pool, this is better than Java somehow.
