### How to install batman-adv: sudo util/install.sh -B


We can start the batman-adv protocol with the command below.

```
~/mininet-wifi$ sudo python examples/adhoc.py batman_adv
```

After starting the script you can alternatively run ifconfig.

```
mininet-wifi> sta1 ifconfig
bat0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.123.1  netmask 255.255.255.0  broadcast 0.0.0.0
        inet6 fe80::e4db:d4ff:fe1c:8f83  prefixlen 64  scopeid 0x20<link>
        ether e6:db:d4:1c:8f:83  txqueuelen 1000  (Ethernet)
        RX packets 2  bytes 84 (84.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 1  bytes 90 (90.0 B)
        TX errors 0  dropped 7 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

sta1-wlan0: flags=4099<UP,BROADCAST,MULTICAST>  mtu 1500
        inet 10.0.0.1  netmask 255.0.0.0  broadcast 10.255.255.255
        inet6 fe80::1  prefixlen 64  scopeid 0x20<link>
        ether 02:00:00:00:00:00  txqueuelen 1000  (Ethernet)
        RX packets 28  bytes 2076 (2.0 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 18  bytes 1518 (1.4 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```

In addition to the wlan interface we can notice an interface called bat0. Internally, Mininet-WiFi runs batctl to tell to batman-adv which interfaces it should use to build the mesh network. According to the batman-adv's documentation we can use any interface we can find with ``ifconfig'' (even pan0 for bluetooth if you like B.A.T.M.A.N. more than the normal, build-in ``mesh-protocol'' of bluetooth).

Despite being up, those interfaces that have been added using batctl do not need any ip-address configured as batman-adv is operating on layer 2 (which is a common mistake by people who are more familiar with batmand or other layer 3 routing protocols)! Those interfaces are bridge-interfaces - we just must not use those plain interfaces for routing anymore.

That's where the virtual bat0 interface (created by batman-adv) is getting into the game. Usually you are going to assign IP adresses to this one - either manually or via dhcpv4 / avahi autoconfiguration / dhcpv6 / ipv6 autoconfiguration. Any packet that enters this interface will be examined by the batman-adv kernel module for its destination mac address and will be forwarded with the help of B.A.T.M.A.N.'s routing voodoo then, so that finally, magically it pops out at the right destination's bat0 interface.

```
mininet-wifi> sta1 ping -c2 192.168.123.3
  0PING 192.168.123.3 (192.168.123.3) 56(84) bytes of data.
64 bytes from 192.168.123.3: icmp_seq=1 ttl=64 time=4.97 ms
64 bytes from 192.168.123.3: icmp_seq=2 ttl=64 time=4.67 ms

--- 192.168.123.3 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1001ms
rtt min/avg/max/mdev = 4.674/4.824/4.974/0.150 ms
```

Batman-adv includes a tool called batctl that offers a convenient way to configure the batman-adv kernel module as well as displaying debug information such as originator tables, translation tables and the debug log. For example, the command below displays the neighbors of a node.


```
mininet-wifi> sta2 batctl meshif bat0 neighbors
[B.A.T.M.A.N. adv 2020.2, MainIF/MAC: sta2-wlan0/02:00:00:00:01:00 (bat0/32:bc:18:c1:a3:16 BATMAN_IV)]
IF           Neighbor              last-seen
sta2-wlan0   02:00:00:00:02:00     0.284s
sta2-wlan0   02:00:00:00:00:00     0.156s
```

A comprehensive list of the features supported by batctl can be obtained through the \textit{batctl -h} command.
