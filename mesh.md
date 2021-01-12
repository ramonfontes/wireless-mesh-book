### Mesh Point (802.11p)


![Network topology](https://github.com/ramonfontes/wireless-mesh-book/blob/main/minimal-topo.png?raw=true)

Let's run ```mesh.py``` as below:

```
~/mininet-wifi$ sudo python examples/mesh.py
```


In addition to create the network topology illustrated above, the script will also automatically create an wireless mesh network interface for the stations. The wireless mesh interface is created by iw is a virtual interface that was created from the wireless physical interface. As we can see below there are two wireless network interfaces named **sta1-wlan0** and **sta1-mp0**. The wlan interface is the physical interface and it is down. On the other hand, the **mp** (mesh point) interface is up and working as mesh point.

```
mininet-wifi> sta1 iwconfig
lo        no wireless extensions.

sta1-mp0  IEEE 802.11  Mode:Auto  Tx-Power=15 dBm   
          Retry short limit:7   RTS thr:off   Fragment thr:off
          Power Management:off
          
sta1-wlan0  IEEE 802.11  ESSID:off/any  
          Mode:Managed  Access Point: Not-Associated   Tx-Power=15 dBm   
          Retry short limit:7   RTS thr:off   Fragment thr:off
          Encryption key:off
          Power Management:off
```


Just for the sake of curiosity, the following command was responsible for creating the mp interface. Since this command is included in the mininet-wifi source code, you do not have to worry about issuing it manually.

```
iw dev sta1-wlan0 interface add sta1-mp0 type mp
```


You can use the info command provided by ```iw``` to confirm that the network interface is working on mesh mode and the wireless mesh network is operational.

```
mininet-wifi> sta1 iw dev sta1-mp0 info
Interface sta1-mp0
	ifindex 2
	wdev 0x7000000002
	addr 02:00:00:00:00:00
	type mesh point
	wiphy 112
	channel 5 (2432 MHz), width: 40 MHz, center1: 2442 MHz
	txpower 15.00 dBm
```



Then try to ```ping``` between **sta1** and **sta3**, as follows.

```
mininet-wifi> sta1 ping -c1 sta3
PING 10.0.0.3 (10.0.0.3) 56(84) bytes of data.
64 bytes from 10.0.0.3: icmp_seq=1 ttl=64 time=2.66 ms

--- 10.0.0.3 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 2.662/2.662/2.662/0.000 ms
```


As you can see, **sta1** is able to communicate with **sta3**, since **sta2** is located between **sta1** and **sta3**, enabling **sta1** to also communicate with **sta3** by means of **sta2**. Technically, we can see that the **sta2** node is actually located between **sta1** and **sta3** using the following command.

```
mininet-wifi> sta1 iw dev sta1-mp0 mpath dump
DEST ADDR         NEXT HOP          IFACE    SN METRIC QLEN EXPTIME  DTIM DRET FLAGS
02:00:00:00:01:00 02:00:00:00:01:00 sta1-mp0 3  171    0    1504 0   0	  0x14
02:00:00:00:02:00 02:00:00:00:01:00 sta1-mp0 4  4268   0    1504 0   0	  0x14
```



This command prints the mesh paths as if they were a routing table linked to sta1. By observing this table, you can notice that the next hop (**NEXT HOP**) for **sta2** (02:00:00:00:01:00) is **sta2** itself, whereas the next hop for **sta3** (02:00:00:00:02:00) is also **sta2**, since this node is located between **sta1** and **sta3**.

You can also run the ```station dump``` command to list all stations know. For example, you can see below that **sta1** can see only **sta2** that has mac address equals to 02:00:00:00:01:00.

```
mininet-wifi> sta1 iw dev sta1-mp0 station dump
Station 02:00:00:00:01:00 (on sta1-mp0)
	inactive time:   440 ms
	rx bytes:        21976412
	rx packets:      16463
	tx bytes:        792971
	tx packets:      8094
	signal avg:      -79 dBm
```


Considering that there are more two stations at the network topology illustrated above, we can observe how much the transmission rate decreases as long as more hops the packet has to pass through. You can get to the results presented below if you add two more stations and run Iperf. In this example, terminals were opened for all the nodes via ```xterm``` and **sta1** acted as a server and the other nodes acted as clients.

```a1# iperf -s
------------------------------------------------------------
Server listening on TCP port 5001
TCP window size: 85.3 KByte (default)
------------------------------------------------------------
[ ID] Interval       Transfer     Bandwidth
[  6] local 10.0.0.1 port 5001 connected with 10.0.0.2 port 41168
[  6]  0.0-12.0 sec  2.50 MBytes  1.75 Mbits/sec
[  6] local 10.0.0.1 port 5001 connected with 10.0.0.3 port 37528
[  6]  0.0-13.2 sec  1.38 MBytes   874 Kbits/sec
[  6] local 10.0.0.1 port 5001 connected with 10.0.0.4 port 58486
[  6]  0.0-12.3 sec   896 KBytes   596 Kbits/sec
[  6] local 10.0.0.1 port 5001 connected with 10.0.0.5 port 57372
[  6]  0.0-14.0 sec   768 KBytes   448 Kbits/sec
```
Based on the results we can notice the throughput drastically decreases as the number of hops increases. Obviously, the results obtained in the test performed above may differ slightly from the results you may find, however, you can clearly notice the difference that the number of hops makes.
