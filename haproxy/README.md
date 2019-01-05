vagrant-haproxy-test
===

Network diagram.
```
               +----------+
               | *client* |
               +----+-----+
                    |
             +------+-------+
             | 192.168.30.2 |
             |   *proxy*    |
             | 192.168.31.2 |
             +------+-------+
                    |            
        +-----------+-----------+
        |                       |
+-------+------+        +-------+------+
| 192.168.31.3 |        | 192.168.31.4 |
+--------------+        +--------------+
|   *web1*     |        |    *web2*    |
+--------------+        +--------------+
```

Launch servers.
```
$ vagrant up

$ curl 192.168.30.2
<h1>Hello World</h1>
```

See statistics.
```
[vagrant@proxy ~]$ echo "show info" | sudo socat stdio /var/lib/haproxy/stats
Name: HAProxy
Version: 1.5.18
Release_date: 2016/05/10
Nbproc: 1
Process_num: 1
Pid: 5692
Uptime: 0d 0h04m44s
Uptime_sec: 284
Memmax_MB: 0
Ulimit-n: 4033
Maxsock: 4033
Maxconn: 2000
Hard_maxconn: 2000
CurrConns: 0
CumConns: 13
CumReq: 22
MaxSslConns: 0
CurrSslConns: 0
CumSslConns: 0
Maxpipes: 0
PipesUsed: 0
PipesFree: 0
ConnRate: 0
ConnRateLimit: 0
MaxConnRate: 4
SessRate: 0
SessRateLimit: 0
MaxSessRate: 4
SslRate: 0
SslRateLimit: 0
MaxSslRate: 0
SslFrontendKeyRate: 0
SslFrontendMaxKeyRate: 0
SslFrontendSessionReuse_pct: 0
SslBackendKeyRate: 0
SslBackendMaxKeyRate: 0
SslCacheLookups: 0
SslCacheMisses: 0
CompressBpsIn: 0
CompressBpsOut: 0
CompressBpsRateLim: 0
ZlibMemUsage: 0
MaxZlibMemUsage: 0
Tasks: 7
Run_queue: 1
Idle_pct: 100
node: proxy
description:

[vagrant@proxy ~]$ echo "show stat" | sudo socat stdio /var/lib/haproxy/stats
# pxname,svname,qcur,qmax,scur,smax,slim,stot,bin,bout,dreq,dresp,ereq,econ,eresp,wretr,wredis,status,weight,act,bck,chkfail,chkdown,lastchg,downtime,qlimit,pid,iid,sid,throttle,lbtot,tracked,type,rate,rate_lim,rate_max,check_status,check_code,check_duration,hrsp_1xx,hrsp_2xx,hrsp_3xx,hrsp_4xx,hrsp_5xx,hrsp_other,hanafail,req_rate,req_rate_max,req_tot,cli_abrt,srv_abrt,comp_in,comp_out,comp_byp,comp_rsp,lastsess,last_chk,last_agt,qtime,ctime,rtime,ttime,
proxy,FRONTEND,,,0,1,2000,8,608,2096,0,0,0,,,,,OPEN,,,,,,,,,1,2,0,,,,0,0,0,4,,,,0,8,0,0,0,0,,0,4,8,,,0,0,0,0,,,,,,,,
app,web1,0,0,0,1,,4,304,1048,,0,,0,0,0,0,UP,1,1,0,0,0,149,0,,1,3,1,,4,,2,0,,2,L7OK,200,1,0,4,0,0,0,0,0,,,,0,0,,,,,3,OK,,0,0,1,1,
app,web2,0,0,0,1,,4,304,1048,,0,,0,0,0,0,UP,1,1,0,0,0,149,0,,1,3,2,,4,,2,0,,2,L7OK,200,1,0,4,0,0,0,0,0,,,,0,0,,,,,3,OK,,0,0,1,1,
app,BACKEND,0,0,0,1,200,8,608,2096,0,0,,0,0,0,0,UP,2,2,0,,0,149,0,,1,3,0,,8,,1,0,,4,,,,0,8,0,0,0,0,,,,,0,0,0,0,0,0,3,,,0,0,1,1,
```
