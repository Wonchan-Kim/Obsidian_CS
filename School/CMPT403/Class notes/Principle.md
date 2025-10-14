
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
Dos attack - Availability was violated. If the passwords were leaked, the Confidentiality was also violated.

Money stolen from an online exchange the owner was forced to shutdown the service - Integrity assures the data is trustworthy and has not been modified by unauthorized persons, confidentiality from the hot wallet key theft

It turns out the administrator opened a fake update file : Integrity + availability 
관리자가 악성 업데이트 실행(integrity), 시스템이 감염 후 중단(availablity)



### How to violate these properties?

Every violation of CIA originates from a vulnerability - and every vulnerability originates from a wrong assumption : 보안은 기술적 실패가 아니라 사고의 실패에서 출발한다.
![[Pasted image 20251014115037.png]]

| Design          | Concept / model   | Not secured                     |
| --------------- | ----------------- | ------------------------------- |
| Implementation  | code/ hardware    | Buffer overflow, race condition |
| Human / Process | Operatoin/ policy |                                 |

----
![[Pasted image 20251014121250.png]]
Spoofing - Confidentiality, Integrity ( Credential theft, phishing)
Tampering - Integrity (DB modification, firmware patch)
Repudiation - Integrity (False denial)
Information Disclosure - Confidentiality (SQLi,)
Denial of service - Availability (Flooding, resource exhaustion)
Escalation of Privilege - Integrity, Confidentiality 

---
## Principles of Secure design
Security should be considered starting from the design phase. 즉 보안은 소프트웨어의 기능적 속성이 아니라 구조적 제약으로 취급되어야 한다. 

