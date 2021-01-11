### How to install batman-adv: 
sudo util/install.sh -B


You can start the batman-adv protocol with the command below.

```
mininet-wifi$ sudo python examples/adhoc.py batman_adv
```

After starting the script you can alternatively run ```ifconfig```.

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

In addition to the wlan interface we can notice an interface called bat0. Internally, Mininet-WiFi runs batctl to tell to batman-adv which interfaces it should use to build the mesh network. According to the batman-adv's documentation we can use any interface we can find with ```ifconfig``` (even pan0 for bluetooth if you like B.A.T.M.A.N. more than the normal, build-in _mesh-protocol_ of bluetooth).

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

A comprehensive list of the features supported by ```batctl``` can be obtained through the ```batctl -h``` command.

```
mininet-wifi> sta1 batctl -h
Usage: batctl [options] command|debug table [parameters]
options:
 	-h print this help (or 'batctl <command|debug table> -h' for the parameter help)
 	-v print version

commands:
 	meshif <netdev> aggregation|ag             [0|1]             	display or modify aggregation setting
 	meshif <netdev> ap_isolation|ap            [0|1]             	display or modify ap_isolation setting
 	vlan <vdev> ap_isolation|ap                [0|1]             	display or modify ap_isolation setting for vlan device or id
 	meshif <netdev> vid <vid> ap_isolation|ap  [0|1]             	display or modify ap_isolation setting for vlan device or id
 	meshif <netdev> bonding|b                  [0|1]             	display or modify bonding setting
 	meshif <netdev> bridge_loop_avoidance|bl   [0|1]             	display or modify bridge_loop_avoidance setting
 	meshif <netdev> distributed_arp_table|dat  [0|1]             	display or modify distributed_arp_table setting
 	hardif <netdev> elp_interval|et            [interval]        	display or modify elp_interval setting
 	event|e                                                      	display events from batman-adv
 	meshif <netdev> fragmentation|f            [0|1]             	display or modify fragmentation setting
 	meshif <netdev> gw_mode|gw                 [mode]            	display or modify the gateway mode
 	meshif <netdev> hop_penalty|hp             [penalty]         	display or modify hop_penalty setting
 	hardif <netdev> hop_penalty|hp             [penalty]         	display or modify hop_penalty setting
 	meshif <netdev> interface|if               [add|del iface(s)]	display or modify the interface settings
 	meshif <netdev> isolation_mark|mark        [mark]            	display or modify isolation_mark setting
 	meshif <netdev> loglevel|ll                [level]           	display or modify the log level
 	meshif <netdev> multicast_fanout|mo        [fanout]        	display or modify multicast_fanout setting
 	meshif <netdev> multicast_forceflood|mff   [0|1]             	display or modify multicast_forceflood setting
 	meshif <netdev> network_coding|nc          [0|1]             	display or modify network_coding setting
 	meshif <netdev> orig_interval|it           [interval]        	display or modify orig_interval setting
 	meshif <netdev> ping|p                     <destination>     	ping another batman adv host via layer 2
 	routing_algo|ra                            [mode]            	display or modify the routing algorithm
 	meshif <netdev> statistics|s                                 	print mesh statistics
 	tcpdump|td                                 <interface>       	tcpdump layer 2 traffic on the given interface
 	hardif <netdev> throughput_override|to     [mbit]        	display or modify throughput_override setting
 	meshif <netdev> throughputmeter|tp         <destination>     	start a throughput measurement
 	meshif <netdev> traceroute|tr              <destination>     	traceroute another batman adv host via layer 2
 	meshif <netdev> translate|t                <destination>     	translate a destination to the originator responsible for it

debug tables:                                   	display the corresponding debug table
 	meshif <netdev> backbonetable|bbt          
 	meshif <netdev> claimtable|cl              
 	meshif <netdev> dat_cache|dc               
 	meshif <netdev> gateways|gwl               
 	meshif <netdev> mcast_flags|mf             
 	meshif <netdev> nc_nodes|nn                
 	meshif <netdev> neighbors|n                
 	meshif <netdev> originators|o              
 	meshif <netdev> transglobal|tg             
 	meshif <netdev> translocal|tl 
 ```
