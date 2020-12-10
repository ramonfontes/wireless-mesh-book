You can run olsrd by running \textit{examples/adhoc.py}, as follows:

\begin{minted}[fontsize=\footnotesize,breaklines]{text}
~/mininet-wifi$ sudo python examples/adhoc.py olsrd
\end{minted}


Then, you can try communicate \texttt{sta1} and \texttt{sta3} with the ping command.

\begin{minted}[fontsize=\footnotesize,breaklines]{text}
mininet-wifi> sta1 ping -c2 sta3
PING 10.0.0.3 (10.0.0.3) 56(84) bytes of data.
64 bytes from 10.0.0.3: icmp_seq=1 ttl=63 time=2.14 ms
64 bytes from 10.0.0.3: icmp_seq=2 ttl=63 time=2.93 ms

--- 10.0.0.3 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1000ms
rtt min/avg/max/mdev = 2.139/2.532/2.926/0.393 ms
\end{minted}

And run the ``route -n'' to get some information about the routes.


\begin{minted}[fontsize=\footnotesize,breaklines]{text}
mininet-wifi> sta1 route -n
Kernel IP routing table
Destination Gateway  Genmask         Flags Metric Ref Use Iface
10.0.0.0    0.0.0.0  255.0.0.0       U     0      0   0   sta1-wlan0
10.0.0.2    10.0.0.2 255.255.255.255 UGH   2      0   0   sta1-wlan0
10.0.0.3    10.0.0.2 255.255.255.255 UGH   2      0   0   sta1-wlan0
\end{minted}