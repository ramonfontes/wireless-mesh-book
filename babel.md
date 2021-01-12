### Babeld

![Network topology](https://github.com/ramonfontes/wireless-mesh-book/blob/main/minimal-topo.png?raw=true)

#### How to install babeld: 

```sudo util/install.sh -E```


The babel routing protocol also requires us to stop ```NetworkManager```. So let's do it with the command below.

```
~/mininet-wifi$ sudo service network-manager stop
```

Then, you run the babel protocol with ```examples/adhoc.py```.

```
~/mininet-wifi$ sudo python examples/adhoc.py babel
```


```
mininet-wifi> xterm sta1
```

```
sta1# ping -c2 10.0.0.3
PING 10.0.0.3 (10.0.0.3) 56(84) bytes of data.
64 bytes from 10.0.0.3: icmp_seq=1 ttl=63 time=3.77 ms
64 bytes from 10.0.0.3: icmp_seq=2 ttl=63 time=4.73 ms

--- 10.0.0.3 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1002ms
rtt min/avg/max/mdev = 3.772/4.409/4.731/0.331 ms
```


```
mininet-wifi> sta1 route -n
Kernel IP routing table
Destination Gateway  Genmask         Flags Metric Ref Use Iface
10.0.0.0    0.0.0.0  255.0.0.0       U     0      0   0   sta1-wlan0
10.0.0.2    10.0.0.2 255.255.255.255 UGH   2      0   0   sta1-wlan0
10.0.0.3    10.0.0.2 255.255.255.255 UGH   2      0   0   sta1-wlan0
```
