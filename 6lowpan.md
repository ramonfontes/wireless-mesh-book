The network topology of this scenario is the same as that illustrated previously below.

![](https://github.com/ramonfontes/wireless-mesh-book/blob/main/minimal-topo.eps)

However, we are now dealing with sensors that have 6LoWPAN connections instead of WiFi. Let's run the 6LoWPAN scenario by running ```examples/6LoWPan.py```, as below.

```
~/mininet-wifi$ sudo python examples/6LoWPan.py
```

Now, you can check the interfaces added to the sensors.

```
mininet-wifi> sensor1 ifconfig
lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

sensor1-pan0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1280
        inet6 2001::1  prefixlen 64  scopeid 0x0<global>
        unspec CE-48-77-61-E3-5A-D5-74-00-00-00-00-00-00-00-00  txqueuelen 1000  (UNSPEC)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 4  bytes 340 (340.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

sensor1-wpan0: flags=195<UP,BROADCAST,RUNNING,NOARP>  mtu 123
        unspec CE-48-77-61-E3-5A-D5-74-00-00-00-00-00-00-00-00  txqueuelen 300  (UNSPEC)
        RX packets 8  bytes 402 (402.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 4  bytes 5197825008 (4.8 GiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```

Since the hardware and driver were configured correctly your system will come up with a node type wpan interface, most likely called wpan0. This explains the sensor1-wpan0 interface we can see above. Internally, Mininet-WiFi will setup the 6LoWPAN interface with a 6LoWPAN 1280 MTU which runs on top the \texttt{sensor1-wpan0} interface, called \texttt{sensor1-pan0}.


Now, you can use iwpan to get some information of the network interface. For example, the info command displays a set of information that can be useful for diagnosis.

```
mininet-wifi> sensor1 iwpan sensor1-wpan0 info
Interface sensor1-wpan0
	ifindex 126
	wpan_dev 0x2
	extended_addr 0xce487761e35ad574
	short_addr 0xffff
	pan_id 0xbeef
	type node
	max_frame_retries 3
	min_be 3
	max_be 5
	max_csma_backoffs 4
	lbt 0
	ackreq_default 0
```

And finally you can use the ping6 command to exchange icmp packets over ipv6.

```ninet-wifi> sensor1 ping6 -c2 sensor2
PING 2001::2(2001::2) 56 data bytes
64 bytes from 2001::2: icmp_seq=1 ttl=64 time=0.099 ms
64 bytes from 2001::2: icmp_seq=2 ttl=64 time=0.156 ms

--- 2001::2 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1030ms
rtt min/avg/max/mdev = 0.099/0.127/0.156/0.028 ms
```
