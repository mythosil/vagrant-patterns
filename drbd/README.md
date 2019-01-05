drbd
===

*DRBD 8.4*


Launch nodes.
```
$ vagrant up node1 & vagrant up node2
```

See status. Both nodes are in `Secondary` role.
```
[vagrant@node1 ~]$ drbdadm status r0
r0 role:Secondary
  disk:Inconsistent
  peer role:Secondary
    replication:Established peer-disk:Inconsistent

[vagrant@node2 ~]$ drbdadm status r0
r0 role:Secondary
  disk:Inconsistent
  peer role:Secondary
    replication:Established peer-disk:Inconsistent
```

Start the initial full synchronization.  
https://docs.linbit.com/docs/users-guide-8.4/#s-initial-full-sync
```
[vagrant@node1 ~]$ sudo drbdadm primary --force r0
[vagrant@node1 ~]$ drbdadm status r0
r0 role:Primary
  disk:UpToDate
  peer role:Secondary
    replication:SyncSource peer-disk:Inconsistent done:1.58

[vagrant@node2 ~]$ drbdadm status r0
r0 role:Secondary
  disk:Inconsistent
  peer role:Primary
    replication:SyncTarget peer-disk:UpToDate done:6.45
```

After the synchronization, node1 is in `Primary` role and node2 is in `Secondary` role.  
Both nodes' disks are `UpToDate` which means synchronization has been done.
```
[vagrant@node1 ~]$ drbdadm status r0
r0 role:Primary
  disk:UpToDate
  peer role:Secondary
    replication:Established peer-disk:UpToDate

[vagrant@node2 ~]$ drbdadm status r0
r0 role:Secondary
  disk:UpToDate
  peer role:Primary
    replication:Established peer-disk:UpToDate
```

Create filesystem on `drbd0` device and mount it.  
This has to be done on `Primary` node only.
```
[vagrant@node1 ~]$ sudo mkfs.xfs /dev/drbd0
meta-data=/dev/drbd0             isize=512    agcount=4, agsize=327540 blks
         =                       sectsz=512   attr=2, projid32bit=1
         =                       crc=1        finobt=0, sparse=0
data     =                       bsize=4096   blocks=1310159, imaxpct=25
         =                       sunit=0      swidth=0 blks
naming   =version 2              bsize=4096   ascii-ci=0 ftype=1
log      =internal log           bsize=4096   blocks=2560, version=2
         =                       sectsz=512   sunit=0 blks, lazy-count=1
realtime =none                   extsz=4096   blocks=0, rtextents=0

[vagrant@node1 ~]$ sudo mount /dev/drbd0 /mnt/drbd0
```

Write a file.
```
[vagrant@node1 ~]$ sudo sh -c 'echo "Hello" > /mnt/drbd0/hello.txt'
```

Switch roles with manual fail-over.  
https://docs.linbit.com/docs/users-guide-8.4/#s-manual-fail-over
```
[vagrant@node1 ~]$ sudo umount /mnt/drbd0
[vagrant@node1 ~]$ sudo drbdadm secondary r0
[vagrant@node1 ~]$ sudo drbdadm status r0
r0 role:Secondary
  disk:UpToDate
  peer role:Secondary
    replication:Established peer-disk:UpToDate

[vagrant@node2 ~]$ sudo drbdadm primary r0
[vagrant@node2 ~]$ sudo drbdadm status r0
r0 role:Primary
  disk:UpToDate
  peer role:Secondary
    replication:Established peer-disk:UpToDate
[vagrant@node2 ~]$ sudo mount /dev/drbd0 /mnt/drbd0
[vagrant@node2 ~]$ cat /mnt/drbd0/hello.txt
Hello
```

