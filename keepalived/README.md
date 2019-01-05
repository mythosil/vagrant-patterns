vagrant-keepalived-test
===

Start the machines.
```
$ vagrant up
```

The active machine has a vip.
```
[vagrant@active ~]$ ip addr show dev eth1
3: eth1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 08:00:27:99:58:dc brd ff:ff:ff:ff:ff:ff
    inet 192.168.30.2/24 brd 192.168.30.255 scope global noprefixroute eth1
       valid_lft forever preferred_lft forever
    inet 192.168.30.101/32 scope global eth1
       valid_lft forever preferred_lft forever
    inet6 fe80::a00:27ff:fe99:58dc/64 scope link
       valid_lft forever preferred_lft forever
```

The standby machine doesn't have a vip.
```
[vagrant@standby ~]$ ip addr show eth1
3: eth1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 08:00:27:1d:12:d3 brd ff:ff:ff:ff:ff:ff
    inet 192.168.30.3/24 brd 192.168.30.255 scope global noprefixroute eth1
       valid_lft forever preferred_lft forever
    inet6 fe80::a00:27ff:fe1d:12d3/64 scope link
       valid_lft forever preferred_lft forever
```

Stop keepalived process on the active machine.
```
[vagrant@active ~]$ sudo systemctl stop keepalived
```

Failing over happens and the standby machine has the vip.
```
[vagrant@standby ~]$ ip addr show eth1
3: eth1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 08:00:27:1d:12:d3 brd ff:ff:ff:ff:ff:ff
    inet 192.168.30.3/24 brd 192.168.30.255 scope global noprefixroute eth1
       valid_lft forever preferred_lft forever
    inet 192.168.30.101/32 scope global eth1
       valid_lft forever preferred_lft forever
    inet6 fe80::a00:27ff:fe1d:12d3/64 scope link
       valid_lft forever preferred_lft forever
```

Start keepalived process on the active machine.  
Failing over happens and the active machine has the vip.
```
[vagrant@active ~]$ sudo systemctl start keepalived

[vagrant@active ~]$ ip addr show eth1
3: eth1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 08:00:27:99:58:dc brd ff:ff:ff:ff:ff:ff
    inet 192.168.30.2/24 brd 192.168.30.255 scope global noprefixroute eth1
       valid_lft forever preferred_lft forever
    inet 192.168.30.101/32 scope global eth1
       valid_lft forever preferred_lft forever
    inet6 fe80::a00:27ff:fe99:58dc/64 scope link
       valid_lft forever preferred_lft forever
```

