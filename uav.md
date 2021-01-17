### Unmanned Aerial Vehicles (UAV)


![Network topology](https://github.com/ramonfontes/wireless-mesh-book/blob/main/coppeliaSim.png?raw=true)

Let's run ```uav.py``` as below:

```
~/mininet-wifi$ sudo python examples/uav/uav.py
```

In addition to create the UAV scenario with CoppeliaSim as illustrated above, the `uav.py` script will also open a GUI provided by Mininet-WiFi. At this moment you can use the CoppeliaSim's GUI to visualize the cameras on board the drones as well as use the Mininet-WiFi's GUI to visualize the signal range of them. 


Eventually, you can try ping **dr1** and **dr2** as below:

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
64 bytes from 192.168.123.2: icmp_seq=8 ttl=64 time=145 ms
64 bytes from 192.168.123.2: icmp_seq=9 ttl=64 time=0.732 ms
64 bytes from 192.168.123.2: icmp_seq=10 ttl=64 time=1.44 ms
64 bytes from 192.168.123.2: icmp_seq=11 ttl=64 time=0.876 ms
64 bytes from 192.168.123.2: icmp_seq=12 ttl=64 time=0.801 ms
64 bytes from 192.168.123.2: icmp_seq=13 ttl=64 time=0.722 ms
```

Ping will work when the nodes are able to see each other and will not work when they cannot see each other.

#### List of neighbors
The list of neighbors can be seen with the command below. Each batman node maintains a list of all single hop neighbors it detects. Whether or not a single hop neighbor is routed to directly or via another single hop neighbor is decided based on the link quality. The printed table begins with a header line with some more or less useful status data, followed by the single hop neighbor table:

```
mininet-wifi> dr1 batctl bat0 n
[B.A.T.M.A.N. adv 2020.2, MainIF/MAC: dr1-wlan0/00:00:00:00:00:01 (bat0/8e:c6:57:9a:47:88 BATMAN_IV)]
IF             Neighbor              last-seen
    dr1-wlan0	  00:00:00:00:00:02    0.200s
    dr1-wlan0	  00:00:00:00:00:03    0.296s
```

#### List of best next neighbors
Each batman node maintains a list of all other nodes in the network and remembers in which direction to send the packets if data should be transmitted. The direction manifests itself in the form of the "best next neighbor" which basically is the next step towards the destination. You can retrieve batman's internal originator table by reading the originators file. The printed table begins with a header line with some more or less useful status data, followed by the originator table. Each line contains information regarding a specific originator:

```
mininet-wifi> dr1 batctl o
[B.A.T.M.A.N. adv 2020.2, MainIF/MAC: dr1-wlan0/00:00:00:00:00:01 (bat0/8e:c6:57:9a:47:88 BATMAN_IV)]
   Originator        last-seen (#/255) Nexthop           [outgoingIF]
 * 00:00:00:00:00:02    0.428s   (170) 00:00:00:00:00:02 [ dr1-wlan0]
   00:00:00:00:00:03    0.552s   (  0) 00:00:00:00:00:02 [ dr1-wlan0]
 * 00:00:00:00:00:03    0.552s   (253) 00:00:00:00:00:03 [ dr1-wlan0]
```

