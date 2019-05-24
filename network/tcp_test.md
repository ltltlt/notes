# tcp test

## tcp_syn_retries

This flag controls how many times syn segment retries to establish connection.
When I change it by 1 on my archlinux, this doesn't work.
But when I change it on my ubuntu, this works.
First retry wait for 1s, the next wait for 2s, 4s. Use exponential backoff.