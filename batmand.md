### How to install batmand: sudo util/install.sh -B

As we saw previously, batmand operates on layer 3 and it requires IP addresses for routing. You can run batmand with \textit{adhoc.py} by passing batmand as a argument, as follows.

```
~/mininet-wifi$ sudo python examples/adhoc.py batmand
```

Then, you can try communicate sta1 and sta3, as below.

```
mininet-wifi> sta1 ping -c2 sta3
PING 10.0.0.3 (10.0.0.3) 56(84) bytes of data.
64 bytes from 10.0.0.3: icmp_seq=1 ttl=63 time=3.57 ms
64 bytes from 10.0.0.3: icmp_seq=2 ttl=63 time=4.05 ms

--- 10.0.0.3 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1002ms
rtt min/avg/max/mdev = 3.572/3.809/4.046/0.237 ms
```

Although sta1 and sta3 can communicate with each other we cannot observe the route table with the ``route -n'' command. This happens due to the way batmand maintain the route table. It has an internaly engine that does the work and we can get the route table with command below.


```
mininet-wifi> sta3 batmand -c -d 1
  Originator  (#/255)         Nexthop [outgoingIF]:   Potential nexthops ... [B.A.T.M.A.N. df6fcb8, MainIF/IP: sta3-wlan0/10.0.0.3, UT: 0d 0h 2m] 
10.0.0.2    (255)        10.0.0.2 [sta3-wlan0]:        10.0.0.2 (255) 
10.0.0.1    (245)        10.0.0.2 [sta3-wlan0]:        10.0.0.2 (245)
```

The -c flag is used to make a connection via unix socket and -d means debug level. We invite you to visit the batmand website if you want to know more about the flags supported by batmand.
