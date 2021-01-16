### Unmanned Aerial Vehicles (UAV)


![Network topology](https://github.com/ramonfontes/wireless-mesh-book/blob/main/coppeliaSim.png?raw=true)

Let's run ```uav.py``` as below:

```
~/mininet-wifi$ sudo python examples/uav/uav.py
```

In addition to create the UAV scenario with CoppeliSim as illustrated above, the script will also automatically create an wireless ad hoc network interface for the stations.

```
mininet-wifi> dr1 ping 192.168.123.2
PING 192.168.123.2 (192.168.123.2) 56(84) bytes of data.
From 192.168.123.1 icmp_seq=1 Destination Host Unreachable
From 192.168.123.1 icmp_seq=2 Destination Host Unreachable
From 192.168.123.1 icmp_seq=3 Destination Host Unreachable
From 192.168.123.1 icmp_seq=4 Destination Host Unreachable
From 192.168.123.1 icmp_seq=5 Destination Host Unreachable
From 192.168.123.1 icmp_seq=6 Destination Host Unreachable
From 192.168.123.1 icmp_seq=7 Destination Host Unreachable
From 192.168.123.1 icmp_seq=11 Destination Host Unreachable
From 192.168.123.1 icmp_seq=12 Destination Host Unreachable
From 192.168.123.1 icmp_seq=13 Destination Host Unreachable
From 192.168.123.1 icmp_seq=14 Destination Host Unreachable
From 192.168.123.1 icmp_seq=15 Destination Host Unreachable
From 192.168.123.1 icmp_seq=16 Destination Host Unreachable
From 192.168.123.1 icmp_seq=17 Destination Host Unreachable
From 192.168.123.1 icmp_seq=18 Destination Host Unreachable
From 192.168.123.1 icmp_seq=19 Destination Host Unreachable
From 192.168.123.1 icmp_seq=20 Destination Host Unreachable
From 192.168.123.1 icmp_seq=21 Destination Host Unreachable
From 192.168.123.1 icmp_seq=22 Destination Host Unreachable
From 192.168.123.1 icmp_seq=23 Destination Host Unreachable
From 192.168.123.1 icmp_seq=24 Destination Host Unreachable
From 192.168.123.1 icmp_seq=25 Destination Host Unreachable
From 192.168.123.1 icmp_seq=26 Destination Host Unreachable
From 192.168.123.1 icmp_seq=27 Destination Host Unreachable
From 192.168.123.1 icmp_seq=28 Destination Host Unreachable
From 192.168.123.1 icmp_seq=29 Destination Host Unreachable
From 192.168.123.1 icmp_seq=30 Destination Host Unreachable
From 192.168.123.1 icmp_seq=31 Destination Host Unreachable
From 192.168.123.1 icmp_seq=32 Destination Host Unreachable
From 192.168.123.1 icmp_seq=33 Destination Host Unreachable
64 bytes from 192.168.123.2: icmp_seq=37 ttl=64 time=145 ms
64 bytes from 192.168.123.2: icmp_seq=38 ttl=64 time=0.732 ms
64 bytes from 192.168.123.2: icmp_seq=39 ttl=64 time=1.44 ms
64 bytes from 192.168.123.2: icmp_seq=40 ttl=64 time=0.876 ms
64 bytes from 192.168.123.2: icmp_seq=41 ttl=64 time=0.801 ms
64 bytes from 192.168.123.2: icmp_seq=42 ttl=64 time=0.722 ms
```


```
mininet-wifi> dr1 batctl bat0 n
[B.A.T.M.A.N. adv 2020.2, MainIF/MAC: dr1-wlan0/00:00:00:00:00:02 (bat0/8e:c6:57:9a:47:88 BATMAN_IV)]
IF             Neighbor              last-seen
    dr1-wlan0	  00:00:00:00:00:03    0.200s
    dr1-wlan0	  00:00:00:00:00:04    0.296s
```


```
mininet-wifi> dr1 batctl o
[B.A.T.M.A.N. adv 2020.2, MainIF/MAC: dr1-wlan0/00:00:00:00:00:02 (bat0/8e:c6:57:9a:47:88 BATMAN_IV)]
   Originator        last-seen (#/255) Nexthop           [outgoingIF]
 * 00:00:00:00:00:03    0.428s   (170) 00:00:00:00:00:03 [ dr1-wlan0]
   00:00:00:00:00:04    0.552s   (  0) 00:00:00:00:00:03 [ dr1-wlan0]
 * 00:00:00:00:00:04    0.552s   (253) 00:00:00:00:00:04 [ dr1-wlan0]
```

