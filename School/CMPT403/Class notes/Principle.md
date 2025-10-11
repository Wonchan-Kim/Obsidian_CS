
----
### Bitstamp Compromise
![[Pasted image 20251011150059.png]]

Attack on Bitstamp was 'Social Engineering attack'. Unrecoverable damage. 
Attackers sent 6 different ===customized phishing mail===, including VBA script. 

Technical flow was
1. User checks the attached file.
2. VBA script auto runs
3. Remote malicious code download & execution
4. Intra-hotwallet was accessed. 

While the security system was there at technical level, attack vector was 'human factor'.

![[Pasted image 20251011152254.png]]
Primary vulnerability: 
	Human-level vulnerability: Spear phishing(targeted towards individual, group, or organization. Highly personalized)
	System-level vulnerability: VBA was auto run in the email. Infected host -> internal wallet server. 

Patch:
	human factor mitigation (security awareness training)
	regulatory phishing simulation



---
![[Pasted image 20251011151845.png]]
Ransomeware attack -> all pipeline operations halted

Vulnerability
1. password reuse
2. MFA x

Security is not merely a technical prevention, the authentication system and design was fundamental problem. 

===The Weakest link defines the system's strength.===

---
<h1> Security is multi faceted. </h1>
Network, software, hardware, data, crypto, and even people, since the attacker only needs to find the weakest point, the defence needs to understand all involved technologies. 

---
# CIA
Confidentiality ( information is secret)
Integrity (Information/system is not tampered) - not changed/modified
Availability (System is usable) - legitimate user can access to the service

![[Pasted image 20251011155022.png]]

![[Pasted image 20251011155706.png]]
Dos attack - Availability was violated. If the passwords were leaked, the