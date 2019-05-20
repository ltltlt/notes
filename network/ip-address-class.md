# ip address class

## class A

start with 0, cover from 0.* to 126.*
the net id take up 1 octet

## class B

start with 10, cover 128.* to 191.*
the net id take up 2 octets

## class C

start with 110, cover 192.* to 223.*
the net id take up 3 octets

## class D

start with 1110, cover 224.* to 239.*
used as multicast address

## class E

all remaining address, start with 1111*
test purpose

## extra

ip consists of net id(we can tell directly from ip address), subnet id(use subnet mask we can identify it)
and host id(other bits except net id and subnet id)

## CIDR

With CIDR, the network ID and subnet ID are combined into one. CIDR is where we get this shorthand slash notation(9.100.100.100/24) that we discussed in the earlier video on subnetting. This slash notation is also known as CIDR notation. CIDR basically just abandons the concept of address classes entirely, allowing an address to be defined by only two Individual IDs. Which is network id(combine original network id and subnet id to this) and host id.
CIDR 一方面减少了路由表中条目的数量(原先的多个C类或其他类地址可以合并为一个)， 也增加了网络可用的主机数目. 比如9.100.100.100/23网络中的主机数是2^9-2=510. 没有CIDR时使用两个C类地址，主机数只有(2^8-2)*2=508.