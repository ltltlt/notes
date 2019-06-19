# iptables

## traverse map

![traverse-map](tables_traverse.jpg)

图上没包含nat表的INPUT和OUTPUT chain, 这两个chain是linux 2.6.34后加的. See [this link](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/?id=c68cd6cc21eb329c47ff020ff7412bf58176984e)

## note

- iptables 默认使用filter table

## MARK

MARK是IPTABLES的一种规则目标，它用于给匹配了相应规则的数据包设置标签。它只能用于mangle表中。然而，标签并不是设置于数据包内容中，而是设置在内核中数据包的载体上。如果需要在数据包内容中设置标签，可以使用TOS规则目标，它可以修改IP数据包头的TOS值。See [this link](http://www.just4coding.com/blog/2016/12/23/iptables-mark-and-polices-based-route/)

## 一些额外target

[new netfilter targets](https://netfilter.org/documentation/HOWTO/netfilter-extensions-HOWTO-4.html)

[netmap target](https://www.frozentux.net/iptables-tutorial/chunkyhtml/x4492.html)
