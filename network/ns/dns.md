# This topic focus on how to setup dns on ubuntu 18.04

I use NetworkManager and dnsmasq

## Why NetworkManager

This is default, and easy to use

## Why dnsmasq

Default is systemd-resolv, this is easy to use, the only problem is it cannot listen on other address except loopback address
because it is intended to be used locally.

## How to stop systemd-resolve

[stop systemd-resolve](https://askubuntu.com/questions/907246/how-to-disable-systemd-resolved-in-ubuntu#answer-907249)

## How

1. update /etc/NetworkManager/NetworkManager.conf

    add(or update) `dns=dnsmasq` under [main] block

2. stop and disable dnsmasq(because this is controlled by systemd, not NetworkManager)

    ```bash
    systemctl stop dnsmasq
    systemctl disable dnsmasq
    ```

3. extra

    - The configuration file of dnsmasq under NetworkManager is at /etc/NetworkManager/dnsmasq.d, not using /etc/dnsmasq.conf by default, because this instance is controlled by NetworkManager. You can check out by running `ps aux | grep dnsmasq`  
    - The instance of dnsmasq will listen on 127.0.1.1, which is loopback, you may want to configure it to listen on some extra ip address, just place `listen-address=...,...' in some configuration file
    - To let dnsmasq not using /etc/resolv.conf, add `no-resolv` into config file(if dnsmasq using /etc/resolv.conf file, this can lead to bad result if it point to this dnsmasq), this is default set by NetworkManager
    - Note that dnsmasq also provide dhcp server function

## How to update /etc/resolv.conf

update /etc/resolvconf/resolv.conf.d/head

then run resolvconf -u

## How to view dnsmasq log

Since dnsmasq use syslog under linux, you can use `tail -F /var/log/syslog | grep -i dnsmasq` to view log.

## Problems have not been solved(solved)

- [x] dnsmasq always use upstream nameserver 127.0.0.1(it listen on 127.0.1.1), and some other dns nameserver(these are not problems, because they are from dhcp). So where is 127.0.0.1 comes from.
    Turns out it's from my dhclient.conf(/etc/dhcp/dhclient.conf), following line:

    ```bash
    prepend domain-name-servers 127.0.0.1;
    ```

- [x] dig get no result, `FORMERR`, because dig send cookie dns option, but dnsmasq doesn't support this option, and give back an error. See [this][dns FORMERR].
    Solution is simple, use `dig -nocookie url` instead

## Useful links

- <https://www.hi-linux.com/posts/30947.html>
- [dns FORMERR]: https://kevinlocke.name/bits/2017/01/20/formerr-from-microsoft-dns-server-for-dig/ "dns FORMERR"
