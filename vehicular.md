### Realistic Vehicular Experimentation

The network topology of tge Realistic Vehicular Experimentation consists of 10 vehicles and will be running with SUMO. 

![Network topology](https://github.com/ramonfontes/wireless-mesh-book/blob/main/sumo.png?raw=true)

In order to run the IBSS scenario you have to run ```examples/vanet-sumo.py```, as follows.

```
~/mininet-wifi$ sudo python examples/vanet-sumo.py
```

Now, try communicating **car1** and **car2**, as below. 


```
mininet-wifi> car1 iw dev car1-wlan0 info
Interface car1-wlan0
	ifindex 12
	wdev 0x500000001
	addr 02:00:00:00:00:00
	ssid vanet-ssid
	type managed
	wiphy 5
	channel 6 (2437 MHz), width: 20 MHz (no HT), center1: 2437 MHz
	txpower 14.00 dBm
```

```
mininet-wifi> car1 iw dev car1-wlan1 info
Interface car1-wlan1
	ifindex 13
	wdev 0x600000001
	addr 02:00:00:00:01:00
	type outside context of a BSS
	wiphy 6
	txpower 20.00 dBm
```
