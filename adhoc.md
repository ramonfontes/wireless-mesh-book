The network topology represented by the IBSS scenario is the same as previously illustrated in Figure~\ref{fig:mesh}. In order to run the IBSS scenario you have to run \textit{examples/adhoc.py}, as follows.

\begin{minted}[fontsize=\footnotesize,breaklines]{text}
~/mininet-wifi$ sudo python examples/adhoc.py
\end{minted}

\noindent Now, try to establish communication by connecting \texttt{sta1} to \texttt{sta2} and \texttt{sta3}, exactly as done below. 

\begin{minted}[fontsize=\footnotesize,breaklines]{text}
mininet-wifi> sta1 ping -c1 sta2
PING 10.0.0.2 (10.0.0.2) 56(84) bytes of data.
64 bytes from 10.0.0.2: icmp_seq=1 ttl=64 time=2046 ms

--- 10.0.0.2 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 2046.137/2046.137/2046.137/0.000 ms
mininet-wifi> sta1 ping -c1 sta3
PING 10.0.0.3 (10.0.0.3) 56(84) bytes of data.
From 10.0.0.1 icmp_seq=1 Destination Host Unreachable

--- 10.0.0.3 ping statistics ---
1 packets transmitted, 0 received, +1 errors, 100% packet loss, time 0ms
\end{minted}

\noindent As we can see, \texttt{sta1} cannot communicate with \texttt{sta3}, since \texttt{sta2} was not instructed on how to forward data traffic to \texttt{sta3} in order to allow \texttt{sta1} and \texttt{sta3} to establish communication with each other.
\\
\\
In wireless \textit{ad-hoc} networks, intermediate nodes do not automatically route traffic, as is done in wireless mesh networks. To do so, you first need to either setup a routing protocol or configure routing tables. So let us look at how this configuring can be done.
\\
\\
To do so, execute the following commands.

\begin{minted}[fontsize=\footnotesize,breaklines]{text}
mininet-wifi> sta1 ip route add 10.0.0.3 via 10.0.0.2
mininet-wifi> sta3 ip route add 10.0.0.1 via 10.0.0.2
mininet-wifi> sta2 echo 1 > /proc/sys/net/ipv4/ip_forward
\end{minted}

\noindent The first command instructs \texttt{sta1} that, in case it needs to talk to \texttt{sta3}, the packet must first pass through \texttt{sta2}. The second, in turn, indicates that for the packet to arrive at \texttt{sta1}, from \texttt{sta3}, it must also pass through \texttt{sta2}. The third one instructs \texttt{sta2} to forward packets addressed to \texttt{sta1} and \texttt{sta3}.
\\
\\
Once all the necessary routing settings have been made, we can confirm them by viewing their routing tables, as follows.

\begin{minted}[fontsize=\footnotesize,breaklines]{text}
mininet-wifi> sta1 route -n
Kernel IP routing table
Destination Gateway  Genmask         Flags Metric Ref  Use Iface
10.0.0.0    0.0.0.0  255.0.0.0       U     0      0    0 sta1-wlan0
10.0.0.3    10.0.0.2 255.255.255.255 UGH   0      0    0 sta1-wlan0

mininet-wifi> sta3 route -n
Kernel IP routing table
Destination Gateway  Genmask         Flags Metric Ref  Use Iface
10.0.0.0    0.0.0.0  255.0.0.0       U     0      0    0 sta3-wlan0
10.0.0.1    10.0.0.2 255.255.255.255 UGH   0      0    0 sta3-wlan0
\end{minted}

\noindent Now, let us try again to \textit{ping} between \texttt{sta1} and \texttt{sta3}. 

\begin{minted}[fontsize=\footnotesize,breaklines]{text}
mininet-wifi> sta1 ping -c1 sta3
PING 10.0.0.3 (10.0.0.3) 56(84) bytes of data.
From 10.0.0.2: icmp_seq=1 Redirect Host(New nexthop: 10.0.0.3)
64 bytes from 10.0.0.3: icmp_seq=1 ttl=63 time=20.1 ms

--- 10.0.0.3 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 20.167/20.167/20.167/0.000 ms
\end{minted}

\noindent As you can see, communication can now be successfully established thanks to the static routing configured on the nodes.
