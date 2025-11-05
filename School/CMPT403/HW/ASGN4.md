<center><h1>Assignment 4</h1></center>
<center>Wonchan Kim 301449586</center>

# Programming 
192.168.122.0/24.
Note that the subnet mask is 24. This denotes that the first 24 bits in the network address, hence each network has 8 bit, having 256 addresses. Hence, for Ingress and Egress, 
Need to check if the address is in the internal subnet. This can be done in 
```python
return ipaddress.IPv4Address(ip_str) in INTERNAL_NET
```
