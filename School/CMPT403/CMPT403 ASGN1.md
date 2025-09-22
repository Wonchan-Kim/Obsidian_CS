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
Q2.
1. {a} Some buffer overflow attacks do not overwrite any return address at all is ===TRUE===. Overflows can target for various things, such as target function pointers, heap meta datas, or adjacent variables. 
2. Minimizing privileges in critical programs can help mitigate the impact of the buffer overflow attacks is ===TRUE===. Limiting what compromised code can do helps to mitigate the impact.
3. Return-oriented programming is able to defeat stack canaries is ===TRUE===. Canaries only detect overwriting the return address with arbitrary data but return oriented programming uses existing code fragments. Return oriented programming therefore can bypass the canary protection by hijacking legitimate instructions.
4. XSS attacks usually require the attacker to gain full control over the web server first is ===FALSE===. XSS exploits in web applications, and most cases requires to inject malicious scripts into content that other users use or view, but not gaining the control of entire server. 
5. If there is a format string vulnerability in OpenSSL, it would be more serious bug that Heartbleed is ===TRUE===. While the Heartbleed was an information disclosure bug, such as leaking memory contents, openSSL can lead to direct memory reads or writes. 
---
Q3.
1. {i} The first example is Wannacry RaNsomeware attack in 2017. It was an attack exploiting the vulnerability 'SMBv1' of Microsoft Windows. The patch was already distributed two months ago the attack, however, many industries and institutions didn't update accordingly. The second example is Equifax data leak that occured in the same year. Similar to the first case, the software update was already distributed to prevent the exploit of vulnerability of Apache Struts, however, Equifax did not update. As a result, private information of 100 million people got leaked. 
2. 
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


Q3. 
Note how the struct is composed. 

![[Pasted image 20250922140735.png]]
The input2 value, which is the second argument passed into the program is only accessed after setting up the v.username, v.goodcanary, v.canary. 

The key to exploit this question is to find the vulnerability in the following while loop.

![[Pasted image 20250922140951.png]]
The while loop checks if the written_char is less than 25, and iterates the second argument via modifying the 'ind' value. 
Note that ind value is always incremented and also breaks when it reaches the end of the string.

If the current index is the ascii value that is specified in the first validity check, it increases the written char value.
If the current index is either ',' or '.', it modifies the v.password[ind] to 0. 
Otherwise if it fails all validity check, it modifies the warn user value to 1.

Since the v.username is set, it is possible to modify the value stored in the v starting from the v.password, as it is sequentially connected.
Important point is to set the length of username and password to be below 24 to pass the while loop test.

Same as question b), there exists the padding. 
After changing the password value, and putting '.' or ',' to set the end of line, 
