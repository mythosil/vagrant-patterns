dnsmasq
===

Launch server.
```
$ vagrant up
```

Query.
```
$ host mythosil.com 192.168.30.2
Using domain server:
Name: 192.168.30.2
Address: 192.168.30.2#53
Aliases:

mythosil.com has address 192.168.123.123

$ host google.com 192.168.30.2
Using domain server:
Name: 192.168.30.2
Address: 192.168.30.2#53
Aliases:

google.com has address 172.217.25.206
google.com has IPv6 address 2404:6800:400a:806::200e
google.com mail is handled by 50 alt4.aspmx.l.google.com.
google.com mail is handled by 20 alt1.aspmx.l.google.com.
google.com mail is handled by 40 alt3.aspmx.l.google.com.
google.com mail is handled by 10 aspmx.l.google.com.
google.com mail is handled by 30 alt2.aspmx.l.google.com.
```

See logs.
```
[vagrant@dns ~]$ sudo tail -f /var/log/messages
Jan  5 03:07:45 localhost dnsmasq[5475]: query[A] mythosil.com from 192.168.30.1
Jan  5 03:07:45 localhost dnsmasq[5475]: config mythosil.com is 192.168.123.123
Jan  5 03:07:45 localhost dnsmasq[5475]: query[AAAA] mythosil.com from 192.168.30.1
Jan  5 03:07:45 localhost dnsmasq[5475]: config mythosil.com is NODATA-IPv6
Jan  5 03:07:45 localhost dnsmasq[5475]: query[MX] mythosil.com from 192.168.30.1
Jan  5 03:07:45 localhost dnsmasq[5475]: config mythosil.com is NODATA
Jan  5 03:08:26 localhost dnsmasq[5475]: query[A] google.com from 192.168.30.1
Jan  5 03:08:26 localhost dnsmasq[5475]: forwarded google.com to 10.0.2.3
Jan  5 03:08:26 localhost dnsmasq[5475]: reply google.com is 172.217.25.206
Jan  5 03:08:26 localhost dnsmasq[5475]: query[AAAA] google.com from 192.168.30.1
Jan  5 03:08:26 localhost dnsmasq[5475]: forwarded google.com to 10.0.2.3
Jan  5 03:08:26 localhost dnsmasq[5475]: reply google.com is 2404:6800:400a:806::200e
Jan  5 03:08:26 localhost dnsmasq[5475]: query[MX] google.com from 192.168.30.1
Jan  5 03:08:26 localhost dnsmasq[5475]: forwarded google.com to 10.0.2.3
```
