## Q1
CIA principle: 
Confidentiality: Information is secret
Integrity: Information is not tampered
Availability: System is usable

a.	1. Availability: stolen resources reduces the victim's system performance
	


Q1.
1. {a} Crypto-jacking
	1. {i} Availability: stolen resources reduces the victim's system performance
	2. Trojan horse: spreads by hiding within something desirable or legitimate, in this question, a useful webpage. Malicious script is executed in the background without their knowledge or consent.
	3. Using the script-blocking plugin for the browser side. Extensions like NoScript can be useful in this case.
2. Backdoor
	1. {i} 
			1. Confidentiality: Backdoor is the random number generator allows attacking to predict the cryptographic keys. With these keys, sensitive datas might be decrypted. 
			2. Integrity: attacker can modify the messages or the digital signatures.
	2. Supply-chain attack: vulnerability was distributed when the legitimate software was installed.
	3. stop using the compromised algorithm and any software that relies on it. One should rely on the cryptographic standards and algorithms that are known to be secure. 
3. Pegasus
	1. {i}  CIA
		1. {1} Confidentiality: designed to secretly access and exfiltrate the private and sensitive data, furthermore even activating the phone functionalities. 
		2. Integrity: malware installs itself on the operating system and can manipulate the device. 
	2. Spear-phishing/zero-click or one-click RCE : via messaging/rendering paths, exploiting 0 days. 
	3. keep softwares up-to-date, as software patches include the security response. 
	   Especially, in zero-day vulnerability, once discovered, vendors release security patches to close the hole. 
4. Zyxel Firewalls
	1. {i}CIA
	    1. {1} Confidentiality: Attacker gained the root control, which directly leads to the leakage of the private/sensitive data.
	    2. Integrity: With root access, an attacker has the authorization to modify the entire firewall configuration, alter logs, etc. 
	    3. Availability: The compromised devices are explicitly used to launch DDoS attacks, which are a direct attack on the availability of a target service. 
	2. Worm: it actively scans the internet for vulnerable devices. Upon finding one, it uses the exploit to infect new device. The newly infected device can then join the botnet and begins scanning for more victims.
	3. Patching the update immediately to fix the vulnerability is crucial. If not available, it can be mitigated by configuring the firewall rules to restrict the access to the device from the public internet, to prevent the malicious packet from the vulnerable service. 
	
---
Coding Question

Q1.

Q2. Canary is used

```
 struct {
          int32_t goodcanary;
          char password[25];
          int32_t canary;
          char good_username[25];
          char good_password[25];
          char username[25];
      } v;
      v.canary = 'b';
      v.goodcanary = 'b';
```
then reads the good user name from password file
then reads the good password from the password file 25 bytes both

읽은 바이트 수 25 최대, strlen 25, 마지막 25번째 원소를 eol.

first input is copied in to the v.username.
