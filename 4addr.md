### 4-address


![Network topology](https://github.com/ramonfontes/wireless-mesh-book/blob/main/4addr-topo.png?raw=true)

Some scenarios require us to stop ```NetworkManager```. ```NetworkManager``` is a program for providing detection and configuration for systems to automatically connect to networks that can be useful for both wireless and wired networks. However, this nature of automatically trying to make certain configurations ends up impacting the expected operation of some scenarios. For this reason, we need to interrupt the ```NetworkManager``` process so that it does not interfere the way the virtual wireless network interfaces work. The command below will stop ```NetworkManager``` and you may lose Internet connection. The ```start``` command can be used to bring ```NetworkManager``` up again.

```
~/mininet-wifi$ sudo service network-manager stop
```

The 4-address scenario illustrated in the figure above. It consists in three access points and two stations associated to each of them. The 4-address scenario can be running as below:

```
~/mininet-wifi$ sudo python examples/4address.py
```

First of all we will confirm the association of the stations **sta1**, **sta3** and **sta5**.

```
mininet-wifi> sta1 iwconfig
lo        no wireless extensions.

sta1-wlan0  IEEE 802.11  ESSID:"ap1-ssid"  
          Mode:Managed  Frequency:2.412 GHz  Access Point: 02:00:00:00:06:00   
          Bit Rate:48 Mb/s   Tx-Power=14 dBm   
          Retry short limit:7   RTS thr:off   Fragment thr:off
          Encryption key:off
          Power Management:off
          Link Quality=70/70  Signal level=-31 dBm  
          Rx invalid nwid:0  Rx invalid crypt:0  Rx invalid frag:0
          Tx excessive retries:0  Invalid misc:16   Missed beacon:0
```

```
mininet-wifi> sta3 iwconfig
lo        no wireless extensions.

sta3-wlan0  IEEE 802.11  ESSID:"ap2-ssid"  
          Mode:Managed  Frequency:2.412 GHz  Access Point: 02:00:00:00:07:00   
          Bit Rate:54 Mb/s   Tx-Power=14 dBm   
          Retry short limit:7   RTS thr:off   Fragment thr:off
          Encryption key:off
          Power Management:off
          Link Quality=70/70  Signal level=-31 dBm  
          Rx invalid nwid:0  Rx invalid crypt:0  Rx invalid frag:0
          Tx excessive retries:0  Invalid misc:30   Missed beacon:0
```

```
mininet-wifi> sta5 iwconfig
lo        no wireless extensions.

sta5-wlan0  IEEE 802.11  ESSID:"ap3-ssid"  
          Mode:Managed  Frequency:2.412 GHz  Access Point: 02:00:00:00:08:00   
          Bit Rate:54 Mb/s   Tx-Power=14 dBm   
          Retry short limit:7   RTS thr:off   Fragment thr:off
          Encryption key:off
          Power Management:off
          Link Quality=70/70  Signal level=-31 dBm  
          Rx invalid nwid:0  Rx invalid crypt:0  Rx invalid frag:0
          Tx excessive retries:0  Invalid misc:26   Missed beacon:0
```

As we can see, the stations are associated to **ap1**, **ap2** and **ap3**, respectively. However, they can communicate with each other by being associated with different access points. This is possible due to the 4-address mode of operation, where the access points are able to allow communication among clients wirelessly.


We can confirm this statement with the ```ping``` command:

```
mininet-wifi> sta1 ping -c1 sta3
PING 192.168.0.3 (192.168.0.3) 56(84) bytes of data.
64 bytes from 192.168.0.3: icmp_seq=1 ttl=64 time=3.41 ms

--- 192.168.0.3 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 3.414/3.414/3.414/0.000 ms
```

```
mininet-wifi> sta1 ping -c1 sta5
PING 192.168.0.5 (192.168.0.5) 56(84) bytes of data.
64 bytes from 192.168.0.5: icmp_seq=1 ttl=64 time=32.0 ms

--- 192.168.0.5 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 31.965/31.965/31.965/0.000 ms
```
