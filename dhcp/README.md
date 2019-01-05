dhcp
===

Vagrant project to look at DHCP([RFC2131](https://www.ietf.org/rfc/rfc2131.txt))'s behaviour.

1. client: create Vagrant machine
2. client: stop eth2 interface
3. server: create Vagrant machine
4. server: wait with tcpdump started
5. client: start eth2 interface
6. server: dump dhcp packets

```
$ vagrant up client
$ vagrant ssh client


[vagrant@client ~]$ ip addr show eth2
4: eth2: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 08:00:27:6e:20:c8 brd ff:ff:ff:ff:ff:ff
    inet6 fe80::a00:27ff:fe6e:20c8/64 scope link
       valid_lft forever preferred_lft forever

[vagrant@client ~]$ sudo ifdown eth2
Device 'eth2' successfully disconnected.

[vagrant@client ~]$ ip addr show eth2
4: eth2: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 08:00:27:6e:20:c8 brd ff:ff:ff:ff:ff:ff

[vagrant@client ~]$ sudo ifup eth2
Connection successfully activated (D-Bus active path: /org/freedesktop/NetworkManager/ActiveConnection/6)

[vagrant@client ~]$ ip addr show eth2
4: eth2: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 08:00:27:6e:20:c8 brd ff:ff:ff:ff:ff:ff
    inet 192.168.31.101/24 brd 192.168.31.255 scope global noprefixroute dynamic eth2
       valid_lft 28796sec preferred_lft 28796sec
    inet6 fe80::a00:27ff:fe6e:20c8/64 scope link
       valid_lft forever preferred_lft forever
```

```
$ vagrant up server
$ vagrant ssh server


[vagrant@server ~]$ sudo tcpdump -nlSvi eth2 dst port 67 or dst port 68
13:47:15.519374 IP (tos 0x10, ttl 128, id 0, offset 0, flags [none], proto UDP (17), length 328)
    0.0.0.0.bootpc > 255.255.255.255.bootps: BOOTP/DHCP, Request from 08:00:27:6e:20:c8, length 300, xid 0xd5ef767a, Flags [none]
          Client-Ethernet-Address 08:00:27:6e:20:c8
          Vendor-rfc1048 Extensions
            Magic Cookie 0x63825363
            DHCP-Message Option 53, length 1: Discover
            Parameter-Request Option 55, length 18:
              Subnet-Mask, BR, Time-Zone, Classless-Static-Route
              Domain-Name, Domain-Name-Server, Hostname, YD
              YS, NTP, MTU, Option 119
              Default-Gateway, Classless-Static-Route, Classless-Static-Route-Microsoft, Static-Route
              Option 252, NTP
13:47:16.521681 IP (tos 0x10, ttl 128, id 0, offset 0, flags [none], proto UDP (17), length 328)
    192.168.31.1.bootps > 192.168.31.101.bootpc: BOOTP/DHCP, Reply, length 300, xid 0xd5ef767a, Flags [none]
          Your-IP 192.168.31.101
          Client-Ethernet-Address 08:00:27:6e:20:c8
          Vendor-rfc1048 Extensions
            Magic Cookie 0x63825363
            DHCP-Message Option 53, length 1: Offer
            Server-ID Option 54, length 4: 192.168.31.1
            Lease-Time Option 51, length 4: 28800
            Subnet-Mask Option 1, length 4: 255.255.255.0
13:47:16.522538 IP (tos 0x10, ttl 128, id 0, offset 0, flags [none], proto UDP (17), length 328)
    0.0.0.0.bootpc > 255.255.255.255.bootps: BOOTP/DHCP, Request from 08:00:27:6e:20:c8, length 300, xid 0xd5ef767a, Flags [none]
          Client-Ethernet-Address 08:00:27:6e:20:c8
          Vendor-rfc1048 Extensions
            Magic Cookie 0x63825363
            DHCP-Message Option 53, length 1: Request
            Server-ID Option 54, length 4: 192.168.31.1
            Requested-IP Option 50, length 4: 192.168.31.101
            Parameter-Request Option 55, length 18:
              Subnet-Mask, BR, Time-Zone, Classless-Static-Route
              Domain-Name, Domain-Name-Server, Hostname, YD
              YS, NTP, MTU, Option 119
              Default-Gateway, Classless-Static-Route, Classless-Static-Route-Microsoft, Static-Route
              Option 252, NTP
13:47:16.524907 IP (tos 0x10, ttl 128, id 0, offset 0, flags [none], proto UDP (17), length 328)
    192.168.31.1.bootps > 192.168.31.101.bootpc: BOOTP/DHCP, Reply, length 300, xid 0xd5ef767a, Flags [none]
          Your-IP 192.168.31.101
          Client-Ethernet-Address 08:00:27:6e:20:c8
          Vendor-rfc1048 Extensions
            Magic Cookie 0x63825363
            DHCP-Message Option 53, length 1: ACK
            Server-ID Option 54, length 4: 192.168.31.1
            Lease-Time Option 51, length 4: 28800
            Subnet-Mask Option 1, length 4: 255.255.255.0
```

