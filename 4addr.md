\begin{figure}[!h]
\includegraphics[width=7cm]{figures/4addr.eps}
\centering
\caption{4-address network topology}
\label{fig:4addr}
\end{figure}

\subsection{4-address}


 The advantage of this mode compared to regular WDS mode is that it is easier to configure and does not require a static list of peer MAC addresses on any side. 4-address mode is incompatible with WDS....



Some scenarios require us to stop NetworkManager. NetworkManager is a program for providing detection and configuration for systems to automatically connect to networks that can be useful for both wireless and wired networks. However, this nature of automatically trying to make certain configurations ends up impacting the expected operation of some scenarios. For this reason, we need to interrupt NetworkManager so that it does not interfere the way the virtual wireless network interfaces work. The command below will stop NetworkManager and you may lose Internet connection. The start command can use used to bring NetworkManager up again.

\begin{minted}[fontsize=\footnotesize,breaklines]{text}
~/mininet-wifi$ sudo service network-manager stop
\end{minted}

The 4-address scenario illustrated in Figure~\ref{fig:4addr} consists in three access points and two stations associated to each of them. The 4-address scenario can be running as below:

\begin{minted}[fontsize=\footnotesize,breaklines]{text}
~/mininet-wifi$ sudo python examples/4address.py
\end{minted}

And now we will confirm the association of the stations \texttt{sta1}, \texttt{sta3} and \texttt{sta5}.

\begin{minted}[fontsize=\footnotesize,breaklines]{text}
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
\end{minted}


\begin{minted}[fontsize=\footnotesize,breaklines]{text}
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
\end{minted}


\begin{minted}[fontsize=\footnotesize,breaklines]{text}
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
\end{minted}

As we can see, the stations are associated to \texttt{ap1}, \texttt{ap2} and \texttt{ap3}, respectively. However, they can communicate with each other by being associated with different access points. This is possible due to the 4-address mode of operation, where the access points are able to allow communication between clients wirelessly.


We can confirm this statement with the ping command:

\begin{minted}[fontsize=\footnotesize,breaklines]{text}
mininet-wifi> sta1 ping -c1 sta3
PING 192.168.0.3 (192.168.0.3) 56(84) bytes of data.
64 bytes from 192.168.0.3: icmp_seq=1 ttl=64 time=3.41 ms

--- 192.168.0.3 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 3.414/3.414/3.414/0.000 ms
\end{minted}

\begin{minted}[fontsize=\footnotesize,breaklines]{text}
mininet-wifi> sta1 ping -c1 sta5
PING 192.168.0.5 (192.168.0.5) 56(84) bytes of data.
64 bytes from 192.168.0.5: icmp_seq=1 ttl=64 time=32.0 ms

--- 192.168.0.5 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 31.965/31.965/31.965/0.000 ms
\end{minted}
