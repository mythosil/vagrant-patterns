bind
===

Create machine.
```
$ vagrant up
```

Check configurations.
```
[vagrant@dns ~]$ sudo named-checkconf /etc/named.conf
[vagrant@dns ~]$ sudo named-checkzone mythosil.local /var/named/mythosil.local.zone
zone mythosil.local/IN: loaded serial 0
OK
```

Query.
```
$ host mythosil.local 192.168.30.2
Using domain server:
Name: 192.168.30.2
Address: 192.168.30.2#53
Aliases:

mythosil.local has address 192.168.30.2

$ host web.mythosil.local 192.168.30.2
Using domain server:
Name: 192.168.30.2
Address: 192.168.30.2#53
Aliases:

web.mythosil.local has address 192.168.30.2

$ host google.com 192.168.30.2
Using domain server:
Name: 192.168.30.2
Address: 192.168.30.2#53
Aliases:

google.com has address 172.217.24.142
google.com has IPv6 address 2404:6800:4004:806::200e
google.com mail is handled by 10 aspmx.l.google.com.
google.com mail is handled by 40 alt3.aspmx.l.google.com.
google.com mail is handled by 20 alt1.aspmx.l.google.com.
google.com mail is handled by 50 alt4.aspmx.l.google.com.
google.com mail is handled by 30 alt2.aspmx.l.google.com.
```

