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
    