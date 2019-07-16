# faq

## Why does a syn or fin bit in a tcp segment consume a byte in the sequence number ?

it's so that the SYN and FIN bits themselves can be acknowledged. [see](https://stackoverflow.com/questions/2352524/why-does-a-syn-or-fin-bit-in-a-tcp-segment-consume-a-byte-in-the-sequence-number)

若不这样, 一个ack，无法分辨这个是响应FIN的(这个FIN segment无payload), 还是响应FIN前面一个segment的.
