<center><h1>Assignment 4</h1></center>  
<center>Wonchan Kim 301449586</center>

# Written Assignment

## Q1

(a)
   ![[Pasted image 20251107125641.png]]  
   Total amount of advertised bandwidth of relays with the guard flag is **249.635765144 Gbit/s**.  
   Total amount of advertised bandwidth of relays with Exit flag is **19.30846652 Gbit/s**.

   There is a significant difference: the total advertised bandwidth of Guard-only relays is much higher. This occurs because Guard relays serve as entry points for most Tor users. Hence the Tor network's health and user experience depends heavily on a reliable and fast first hop, and therefore guards tend to have stable, high-capacity connections.

   Exit relays are riskier to operate because traffic leaving the Tor network can be traced back to the operator, exposing them to legal or abuse complaints. Consequently, fewer operators are willing to run high-bandwidth Exit relays, producing a significantly lower total advertised bandwidth for Exit-only relays compared to Guard-only relays.
--- 
(b)
   For a 50 KiB file, the median download time is 5.315 s, giving a median download rate  
   $$
   \text{MDR} = \frac{50 \times 1024}{5.315} = 9633.11\ \text{bytes/s}.
   $$  
   For a 5 MiB file, the median download time is 28.53 s, resulting in  
   $$
   \text{MDR} = \frac{5 \times 1024^2}{28.53} = 183445.77\ \text{bytes/s}.
   $$

   Under the assumption that both downloads have the same latency $L$ and transfer rate $R$, the total load times can be expressed as  
   $$
   T_{50} = L + \frac{S_{50}}{R},\quad T_{5M} = L + \frac{S_{5M}}{R}.
   $$

   Solving these equations gives  
   $$
   R = \frac{S_{5M} - S_{50}}{T_{5M} - T_{50}} = \frac{5242880 - 51200}{28.53 - 5.315} = 223154\ \text{bytes/s},
   $$  
   and  
   $$
   L = T_{50} - \frac{S_{50}}{R} = 5.315 - \frac{51200}{223154} = 5.0856\ \text{s}.
   $$

   For the 50 KiB file, the latency fraction is  
   $$
   \frac{L}{T_{50}} = \frac{5.0856}{5.315} = 0.957\ (\text{95.7\%}),
   $$  
   indicating that most of the load time is due to latency. For the 5 MiB file, the latency fraction is  
   $$
   \frac{L}{T_{5M}} = \frac{5.0856}{28.53} = 0.178\ (\text{17.8\%}),
   $$  
   showing that the larger file’s load time is dominated by data transfer rather than latency.

---

(c)
   Assume the per-connection latency is identical and total latency scales with the number of connections in the circuit.

   - A 3-node circuit has **4** connections (user → guard → middle → exit → server).  
   - A 2-node circuit has **3** connections (user → entry → exit → server).

   Using values from part (2): $L_3 = 5.0856\ \text{s}$ and $R = 223{,}154\ \text{bytes/s}$.

   The 2-node latency:
   $$
   L_2 = \frac{3}{4} L_3 = \frac{3}{4}\times 5.0856 = 3.8142\ \text{s}.
   $$

   For a 50 KiB file ($S = 50\times1024 = 51{,}200\ \text{bytes}$), the load time is
   $$
   T_2 = L_2 + \frac{S}{R} = 3.8142 + \frac{51{,}200}{223{,}154} = 3.8142 + 0.2294 \approx 4.04\ \text{s}.
   $$
---

(d)
   Using 4 nodes instead of 3 increases the circuit connections from 4 to 5 (user → node1 → node2 → node3 → node4 → server). Assuming each connection has the same latency $L_c$ and all nodes share the same transfer rate $R$, the total latency becomes $L_4 = \tfrac{5}{4}L_3$ while $R$ remains unchanged.

   Therefore,
   $$
   T_4 = L_4 + \frac{S}{R} = \frac{5}{4}L_3 + \frac{S}{R},
   $$
   so performance decreases slightly due to the additional hop and higher cumulative latency.
   Moreover, each relay includes the encryption and traffic forwarding, leading to higher bandwidth overhead, also contributing to the lwoer performance.

   Privacy improves because an extra relay increases path diversity and makes end-to-end correlation attacks more difficult.

---

## Q2

**(a)** **k-anonymity**  
k-anonymity is suitable because it allows the company to release a dataset for public use while ensuring that each user’s record is indistinguishable from at least $k-1$ others based on key identifiers. This prevents contestants from identifying specific individuals while still allowing data analysis for improving algorithms.

SMPC secures multiparty computation protects data during joint computation between parties without sharing raw data, but it is not designed for releasing large static datasets to the public. It would be too complex and inefficient for a public competition scenario.


---

**(b)** **differential privacy**  
Differential privacy is appropriate because it allows the company to collect and analyze movement data while adding calibrated noise, preventing the app from learning any specific individual’s exact location. This keeps aggregate trends accurate but hides personal trajectories.

PIR protects the user’s query from the server, not the server’s collection of continuous user data like location tracking. It cannot prevent the device from knowing real-time movements.


---

**(c)** PIR 
PIR allows a user to query a database without revealing which specific entry they are interested in. This prevents the DNS server from inferring your target domain and mitigates cybersquatting risks.

k-anonymity provides anonymity in shared datasets but does not hide which query user sent. It cannot protect the specific domain name being checked from the DNS operator.

---

**(d)** SMPC
SMPC allows multiple parties to jointly compute whether a match exists without revealing any other private information about either party.

Differential privacy adds noise to protect aggregate statistics, not to enable exact private matches. It would fail to provide the precise yes/no answer needed in this scenario.

---

# Programming

**(a)**  
Subnet: `192.168.122.0/24`.  
Note that the subnet mask is 24, which denotes that the first 24 bits are the network portion; the remaining 8 bits provide 256 addresses in the subnet. To check if an address is inside the internal subnet we can use:

```python
return ipaddress.IPv4Address(ip_str) in INTERNAL_NET
```

Then only allow packet with condition; IP source is subnet & IP destination is not in subnet and vice versa.

---

**(b)**  
Two notable attacks (alphabetized):

1. **Ping of death**  
   Ping of death uses ICMP Echo Request with manipulated fragmented packets. The IPv4 total length field (header + payload) has a maximum value of 65535. If fragments are constructed such that the reassembled packet would exceed 65535 bytes, overflowing buffers, causing crashes or other undefined behaviour. The packet below was identified as part of a ping-of-death sequence.  
   ![[Pasted image 20251105190709.png]]

2. **Smurf attack**  
   A Smurf attack: an attacker sends ICMP Echo Requests with a spoofed source IP and destination set to the network broadcast address. Hosts on the broadcast network reply to the victim, amplifying traffic toward the victim and potentially overwhelming it. The packets highlighted below shows the packets that should be dropped.   
   ![[Pasted image 20251105125112.png]]

---

**(c)** SYN floods
Using two data structures, conn_state and half_open, which each tracks TCP connection states and counting current half open connections per inter destination. 

When an external client sends a SYN to an internal host, if the internal host already has or more half_open connections, the SYN is dropped. Otherwise the connection is marked as SYN_SENT and allowed. 

On receiving SYN+ACK or ACK, the connection state is updated and the half open count is decreased once the handshake completes. 
FIN or RST packets remove the connection from the tracking table. 

 <div class="page-break" style="page-break-before: always;"></div>

# LLM Usage

For writing part I used LLM for fixing the grammar, and verification of my answers.

For programming part, I had to use LLM to get the correct syntax of python for some of the codes. Main idea and logic was written by myself. To be specific, I was not familiar with the dictionary syntax. 




