<center><h1>Assignment 4</h1></center>
<center>Wonchan Kim 301449586</center>

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
		
    