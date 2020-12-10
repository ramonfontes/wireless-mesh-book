The SUMO version used in the writing of this tutorial was 1.8.0, but there should be no complications if you decide to use a more recent version. In case you notice any problems, feel free to contact the authors of this chapter. Now, assuming SUMO is already installed, let us run \textit{vanet-sumo.py}.


\begin{minted}[fontsize=\footnotesize,breaklines]{text}
    ~/mininet-wifi$ sudo python examples/vanet-sumo.py
\end{minted}

Now, SUMO should be running and you have an opportunity to learn more about \textit{sumo-gui}, SUMO's graphical interface. Using \textit{sumo-gui} you can start the simulation, pause, filter objects like cars, among other actions. As the simulated vehicles move, you can also use the Mininet-WiFi CLI to interact with them. The main integration between SUMO and Mininet-WiFi involves capturing vehicle positions. This means that if the Mininet-WiFi has the position data of the vehicles, the whole wireless part and network configuration in general is carried out by Mininet-WiFi.


Using the SUMO graphical interface, try to pause the simulation when the time reaches 130 seconds. You will surely notice that they are located relatively close to each other. Therefore, it should be possible to test V2V or vehicle-to-vehicle communication, which is done among the vehicles without the need for eNodeB (\textit{Evolved Node B}), the base station component of networks that use LTE (\textit{Long-Term Evolution}), a mobile phone network standard. For Mininet-WiFi, eNodeB is nothing more than an access point.
                        
                        
So let us try to \textit{ping} between cars 2 and 3 via xterm. 

\begin{minted}[fontsize=\footnotesize,breaklines]{text}
mininet-wifi> xterm car2
\end{minted}

And then you issue the ping command.

\begin{minted}[fontsize=\footnotesize,breaklines]{text}
car2# ping -c2 192.168.123.3
PING 192.168.123.3 (192.168.123.3) 56(84) bytes of data.
64 bytes from 192.168.123.3: icmp_seq=1 ttl=64 time=1.41 ms
64 bytes from 192.168.123.3: icmp_seq=2 ttl=64 time=1.36 ms

--- 192.168.123.3 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1002ms
rtt min/avg/max/mdev = 1.363/1.384/1.405/0.021 ms
\end{minted}

In the \textit{vanet-sumo.py} script you will notice that the \textit{batman-adv} routing protocol is being used. Batman-adv will be introduced at section~\ref{sec-batman-adv}.
