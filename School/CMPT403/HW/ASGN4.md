<center><h1>Assignment 4</h1></center>
<center>Wonchan Kim 301449586</center>

# Written Assignment
# Q1
1. {a} 
   ![[Pasted image 20251107125641.png]]
   Total amount of advertised bandwidth of relays with the guard flag is 249.635765144 Gbit/s.
   Total amount of advertised bandwidth of relays with Exit flag is 19.30846652Gbits/s.
   
   There is a significant difference between total advertised bandwidth of Guard only relays is much higher. This occurs because Guard relays serve as entry points for most Tor users. Hence the Tor network's health and user experience depends heavily on a reliable and fast first hop, and therefore handles a larger number of incoming circuits, requiring stable, high-capacity connections. 
   
   On the other hand, Exit relays are risker to operate due to its nature, traffic leaving the Tor network can be traced back to the operator, exposing them to legal or abuse complaints. Consequently, fewer operators are willing to run high bandwidth Exit relays, leading to a significantly lower total advertised bandwidth for Exit only relays compared to Guard only relays.
1. 
2. 
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
		
    