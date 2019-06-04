# network namespace

## capture traffic of a program

[use strace](https://unix.stackexchange.com/questions/233490/show-network-connections-of-a-process#answer-338626), this is hard to read, but pretty cool.

[use network namespace](https://askubuntu.com/questions/11709/how-can-i-capture-network-traffic-of-a-single-process/499850#499850)

[use lsof](https://unix.stackexchange.com/questions/233490/show-network-connections-of-a-process#answer-233498)

## some more about using network namespace

we can use this approach to do more, for example: different program use different network link.
we can use docker too, but it's too heavy, and I cannot properly use it.

1. create a namespace first:

    ```bash
    ip netns add test
    ```

2. create virtual ethernet pair:

    ```bash
    ip link add veth-a type veth peer name veth-b
    ```

3. add one virtual ethernet to netns:

    ```bash
    ip link set veth-a netns test
    ```

4. configure ip address of virtual ethernet pair:

    ```bash
    ip netns exec test ifconfig veth-a up 192.168.163.1 netmask 255.255.255.0
    ifconfig veth-b up 192.168.163.254 netmask 255.255.255.0
    ```

5. configure routing for this network namespace:

    ```bash
    ip netns exec test route add default gw 192.168.163.254 dev veth-a
    ```

6. activate ip forward:

    ```bash
    echo 1 > /proc/sys/net/ipv4/ip_forward
    ```

7. setup DNAT rule:

    ```bash
    iptables -t nat -A POSTROUTING -s 192.168.163.0/24 -o IFACE -j SNAT --to-source YOURIPADDRESS
    ```

    we can achieve this by using MASQUERADE rule too:

    ```bash
    iptables -t nat -A POSTROUTING -s 192.168.163.0/24 -o IFACE -j MASQUERADE
    ```

    about MASQUERADE

    >>> 在某些情况下，网关的外网IP地址可能不是固定的，列如使用ADSL宽带接入时，针对这这种需求，iptables提供了一个名为MASQUERASE(伪装)的数据包控制类型，MASQUERADE相当于SNAT的一个特例，同样同来修改数据包的源IP地址只过过它能过自动获取外网接口的IP地址。
    >>>
    >>> 参照上一个SNAT案例，若要使用MASQUERADE伪装策略，只需要去掉SNAT策略中的“--to-source IP地址”。然而改为“-j MASQUERADE”指定数据包的控制类型。对于ADSL宽带连接来说，连接名称通常为ppp0，ppp1等。

    quote from [here](https://blog.51cto.com/dengqi/1264838)

After all that, I still cannot get curl or ping work in this network namespace, the ip datagram can go outbound, but nothing coming back, after some digging, I find out I need some extra work(maybe because I have docker installed):

8. 在默认 FORWARD 规则为 DROP 时显式地允许 veth-b 和 IFACE 之间的 forwarding(see [here](https://www.cnblogs.com/sammyliu/p/5760125.html))

    ```bash
    iptables -t filter -A FORWARD -i IFACE -o veth-b -j ACCEPT
    iptables -t filter -A FORWARD -o IFACE -i veth-b -j ACCEPT
    ```

If you cannot get DNS work, you may need following steps:

1. let DNS server can receive dns query from your test network namespace
2. update /etc/resolv.conf(I update it by add a row to /etc/resolvconf/resolv.conf.d/head then run resolvconf -u)

## useful link

- [capture traffic of a program](https://unix.stackexchange.com/questions/233490/show-network-connections-of-a-process)
- [again](https://askubuntu.com/questions/11709/how-can-i-capture-network-traffic-of-a-single-process/499850)
- [https://www.cnblogs.com/sammyliu/p/5760125.html](https://www.cnblogs.com/sammyliu/p/5760125.html)
- [iptables snat](https://blog.51cto.com/dengqi/1264838)
- [iptables packet traverse map](http://www.adminsehow.com/2011/09/iptables-packet-traverse-map/)
