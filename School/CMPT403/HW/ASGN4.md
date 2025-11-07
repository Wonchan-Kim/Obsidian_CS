<center><h1>Assignment 4</h1></center>
<center>Wonchan Kim 301449586</center>

# Written Assignment
# Q1
1. {a}  
   asd
   ![[Pasted image 20251107125641.png]]
   Total amount of advertised bandwidth of relays with the guard flag is 249.635765144 Gbit/s.
   Total amount of advertised bandwidth of relays with Exit flag is 19.30846652Gbits/s.
   
   There is a significant difference between total advertised bandwidth of Guard only relays is much higher. This occurs because Guard relays serve as entry points for most Tor users. Hence the Tor network's health and user experience depends heavily on a reliable and fast first hop, and therefore handles a larger number of incoming circuits, requiring stable, high-capacity connections. 
   
   On the other hand, Exit relays are risker to operate due to its nature, traffic leaving the Tor network can be traced back to the operator, exposing them to legal or abuse complaints. Consequently, fewer operators are willing to run high bandwidth Exit relays, leading to a significantly lower total advertised bandwidth for Exit only relays compared to Guard only relays.
2. 
   For a 50 KiB file, the median download time is 5.315 s, giving a median download rate  
$MDR = \frac{50 \times 1024}{5.315} = 9633.11\ \text{bytes/s}$.  
For a 5 MiB file, the median download time is 28.53 s, resulting in  
$MDR = \frac{5 \times 1024^2}{28.53} = 183445.77{bytes/s}$

Under the assumption that both downloads have the same latency $L$ and transfer rate $R$,  
the total load times can be expressed as  
$T_{50} = L + \frac{S_{50}}{R}$ and $T_{5M} = L + \frac{S_{5M}}{R}$.

Solving these equations gives  
$R = \frac{S_{5M} - S_{50}}{T_{5M} - T_{50}} = \frac{5242880 - 51200}{28.53 - 5.315} = 223154\ \text{bytes/s}$,  
and  
$L = T_{50} - \frac{S_{50}}{R} = 5.315 - \frac{51200}{223154} = 5.0856\ \text{s}$.

For the 50 KiB file, the latency fraction is  
$\frac{L}{T_{50}} = \frac{5.0856}{5.315} = 0.957$ (95.7 %), indicating that most of the load time is due to latency.  
For the 5 MiB file, the latency fraction is  
$\frac{L}{T_{5M}} = \frac{5.0856}{28.53} = 0.178$ (17.8 %), showing that the larger file’s load time is dominated by data transfer rather than latency.

   
2. Assume the per-connection latency is identical and total latency scales with the number of connections in the circuit.  
A 3-node circuit has **4** connections (user→guard, guard→middle, middle→exit, exit→server);  
a 2-node circuit has **3** connections (user→entry, entry→exit, exit→server).

Using the values from (b), $L_{3}=5.0856\ \text{s}$ and $R=223{,}154\ \text{bytes/s}$.

Thus the 2-node latency is
$L_{2}=\frac{3}{4}L_{3}=\frac{3}{4}\times 5.0856=3.8142\ \text{s}$.

For a 50 KiB file ($S=50\times1024=51{,}200\ \text{bytes}$), the load time is
$T_{2}=L_{2}+\frac{S}{R}=3.8142+\frac{51{,}200}{223{,}154}
=3.8142+0.2294 \approx 4.04\ \text{s}$.

2. Using 4 nodes instead of 3 would increase the total number of connections in the Tor circuit from 4 to 5  
(user→node1, node1→node2, node2→node3, node3→node4, node4→server).  
Assuming each connection has the same latency $L_c$ and all nodes share the same transfer rate $R$,  
the total latency becomes $L_4 = \frac{5}{4}L_3$ while $R$ remains unchanged.

Therefore, the total load time increases slightly:
$T_4 = L_4 + \frac{S}{R} = \frac{5}{4}L_3 + \frac{S}{R}$,
meaning performance (speed) decreases due to the additional hop and higher cumulative latency.

However, privacy improves because an extra relay increases path diversity and makes end-to-end correlation attacks more difficult.  
In short, **4 nodes reduce performance slightly but strengthen anonymity** by adding one more layer of relay separation between the user and destination.


# Programming 
(a)
192.168.122.0/24.
Note that the subnet mask is 24. This denotes that the first 24 bits in the network address, hence each network has 8 bit, having 256 addresses. Hence, for Ingress and Egress, 
Need to check if the address is in the internal subnet. This can be done in 
```python
return ipaddress.IPv4Address(ip_str) in INTERNAL_NET
```
(b)
 1. Smurf attack
    Smurf attack is done as followed; attacker sends the ping request(ICMP Echo Request), change the source address of packet, and set the IP destination to the network broadcast address. Then all of the hosts of the subnet will send Echo Reply to the victim's address. It will amplify to the victim's ip. 
    Packets highlighted below is the typical Smurf attack.
    ![[Pasted image 20251105125112.png]]
 2. Ping of death
	Ping of death uses ICMP Echo Request with manipulated fragmented packet. The header of IPv4 header total length = header + payload, with maximum value 65535. Each fragment consists of fragment offset and MF(more fragment) bit in order to reassemble after transmission. The attack is using this weakness; making the packets bigger than 65535 after reassembly causing buffer overflow. 
	![[Pasted image 20251105190709.png]]
	Using this filter in the Wireshark, the packet above is used for the ping of death attack.
(c) SYN floods
	pseudocode is as follows:
		check src, s-port, dst, dst-port
		
    