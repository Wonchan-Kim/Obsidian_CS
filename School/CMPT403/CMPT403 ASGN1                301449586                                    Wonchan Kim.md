Q1.
1. {a} Crypto-jacking
	1. {i} Availability: stolen resources reduces the victim's system performance
	2. Trojan horse: spreads by hiding within something desirable or legitimate, in this question, a useful webpage. Malicious script is executed in the background without their knowledge or consent.
	3. Using the script-blocking plugin for the browser side. Extensions like NoScript can be useful in this case.
2. Backdoor
	1. {i} 
			1. Confidentiality: Backdoor is the random number generator allows attacking to predict the cryptographic keys. With these keys, sensitive datas might be decrypted. 
			2. Integrity: attacker can modify the messages or the digital signatures.
	2. Weakness was deliberately introduced into a trusted standard and then was distributed through legitimate software. This attack can be classified as supply-chain attack.
	3. One should rely on the cryptographic standards and algorithms that are known to be secure that are thoroughly tested, and widely trusted by the secure community.
3. Pegasus
	1. {i}  CIA
		1. {1} Confidentiality: designed to secretly access and exfiltrate the private and sensitive data, furthermore even activating the phone functionalities. 
		2. Integrity: malware installs itself on the operating system and can manipulate the device. 
	2. Pegasus is well-known zero click exploits. Malware spreads through sending victims malicious file disguised as a GIF file. Clicking or accessing such file will lead to installation of the surveillance software. 
	3. keep softwares up-to-date, as software patches include the security response. 
	   Especially, in zero-day vulnerability, once discovered, vendors release security patches to close the hole. 
4. Zyxel Firewalls
	1. {i} CIA
	    1. {1} Confidentiality: Attacker gained the root control, which directly leads to the leakage of the private/sensitive data.
	    2. Integrity: With root access, an attacker has the authorization to modify the entire firewall configuration, alter logs, etc. 
	    3. Availability: The compromised devices are explicitly used to launch DDoS attacks, which are a direct attack on the availability of a target service. 
	2. Worm: it actively scans the internet for vulnerable devices. Upon finding one, it uses the exploit to infect new device. The newly infected device can then join the botnet and begins scanning for more victims.
	3. Patching the update immediately to fix the vulnerability is crucial. If not available, it can be mitigated by configuring the firewall rules to restrict the access to the device from the public internet, to prevent the malicious packet from the vulnerable service. 
---
Q2.
1. {a} Some buffer overflow attacks do not overwrite any return address at all is ==TRUE==. Overflows can target for various things, such as target function pointers, heap meta datas, or adjacent variables. 
2. Minimizing privileges in critical programs can help mitigate the impact of the buffer overflow attacks is ==TRUE==. Limiting what compromised code can do helps to mitigate the impact. Also known as Principle of Least Privilege. 
3. Return-oriented programming is able to defeat stack canaries is ==TRUE==. Canaries only detect overwriting the return address with arbitrary data but return oriented programming uses existing code fragments. Return oriented programming therefore can bypass the canary protection by hijacking legitimate instructions.
4. XSS attacks usually require the attacker to gain full control over the web server first is ==FALSE==. XSS exploits in web applications, and most cases requires to inject malicious scripts into content that other users use or view, but not gaining the control of entire server. 
5. If there is a format string vulnerability in OpenSSL, it would be more serious bug that Heartbleed is ==TRUE==. While the Heartbleed was an information disclosure bug, such as leaking memory contents, openSSL can lead to direct memory reads or writes. 
----------------------
Q3.
1. The first example is Wannacry Ransomeware attack in 2017. It was an attack exploiting the vulnerability 'SMBv1' of Microsoft Windows. The patch was already distributed two months ago the attack, however, many industries and institutions didn't update accordingly. 
   The second example is Equifax data breach that occured in the same year, compromising the personal information of approximately 148 million people. Attackers managed to steal sensitive data from the credit reporting agency by exploiting a vulnerability within the company’s system. Equifax failed to patch its systems months after a fix was released for this vulnerability, resulting in successful exploitation.
   
2. Malware are designed to bypass or disable the antivirus detection.
   First of all, while anti virus software relies on the signature-based detection, malware can modify the binary easily each time it spreads(polymorphism & metamorphism). This creates a new binary signature for each variant, rendering existing signature useless.  
   
   Secondly, antivirus is inherently reactive. Zero-days exploits can not be successfully prevented by anti virus software as the vulnerability is not known, and no antivirus can detect the malware that exploits is. 
   
   Thirdly, malware often installs at a low system level such as rootkits making it available to hide from the antivirus software, allowing them to intercept the system calls and files. Code packing can also be used to compress the malicious code.

1. From the attackers' perspective, when the login name or user id is exposed, for example can try brute-force method to generate the fairly well known combinations that can be obtained from the information leaked. Also since most of people use the same password across the sites with least possible modifications, there might be several valid options attackers could try. Even with the strong server security, if the password is not complex or diverse enough, there is a high possibility that the attacker could log in with valid credential.  
   
   A password manager is a crucial defense method. It mitigates this risk by generating and storing a unique, long, and random password for each individual service. This practice minimizes the damage from a breach, even if the password for Site A is compromised, it has no value for an attacker trying to access other sites.
---

# Programming Assignment

Q1.
Looking at the code, 
![[Pasted image 20250922154605.png]]
The strcpy function is used to copy the user-provided username into a 16-byte buffer named v.username. However, there is no bounds checking, meaning if the input username is longer than 15 characters, total 16 including null, it will write past the end of the username buffer. The check_failed integer variable, which controls login access, is located directly after the username buffer in the stack's memory layout.

![[Pasted image 20250922154645.png]]
While the login is only denied by checking the check_failed flag, it is located under the username input. By changing the check_failed flag in to 0 using buffer overflow, it is possible to login to the system.
Since uname_len is 16bytes enter 17bytes total with 0(null byte) at the end.

WCK51234123412340 would be my username to login.
Password here is irrelevant. 

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

first input is copied in to the v.username. 
Using buffer overflow, we can modify the value stored in the v struct as the memory are consecutive. 

![[Pasted image 20250922153847.png]]
Looking at the canary, we can know it is time based and username based. 
The most important stuff is that the code prints the canary value on the terminal. 

Through debugging, I could find out that the paddings are added after the password in order to make it 28 bytes and the little endian is used for the memory layout. Putting the Canary value and modifying v.goodusername, v.good_password, v.username, I could able to login. Modifying the v.username to the same value of the v.goodusername also enable to log in to the system without needing to know the original username. 

With canary value HVPX

My final answer would be

wck5
AAAAAAAAAAAAAAAAAAAAAAAAABBBHVPXwck5CCCCCCCCCCCCCCCCCCCCCAAAAAAAAAAAAAAAAAAAAAAAADwck5CCCCCCCCCCCCCCCCCCCCC


Q3. 
Note how the struct is composed. 

![[Pasted image 20250922140735.png]]
The input2 value, which is the second argument passed into the program is only accessed after setting up the v.username, v.goodcanary, v.canary. 

The key to exploit this question is to find the vulnerability in the following while loop.

![[Pasted image 20250922140951.png]]
The while loop checks if written_char is less than 25 and iterates through the second command-line argument by modifying the ind value. Note that the ind value is always incremented, and the loop breaks only when it reaches the end of the input string.

If the character at the current index passes the validity check, the written_char counter is incremented. If the character is a ',' or '.', it writes a null terminator to v.password[ind]. Otherwise, if the character fails all validity checks, the warn_user flag is set to 1.

Since the members of the v struct are located sequentially in memory, it is possible to modify values starting from v.password onwards. An important point is that the number of valid characters used for the username and password must be less than 25 to pass the while loop's test.

Similar to the previous question, memory padding exists between the struct's members. To access the memory after the canary without modifying the canary itself, we can use invalid characters in our input to advance the ind pointer over the canary's memory location.

Taking memory padding into consideration, I first constructed the string 
wck54.--------------------- (27 bytes). 
To bypass the 4-byte canary without changing its value, I needed to add 4 more invalid characters, resulting in: 
wck54.------------------------- (31 bytes).

Now that the payload is positioned past the canary, we can access v.goodusername and v.goodpassword.

We can manipulate both of these variables by writing valid ASCII values. Since my username is wck5, I can input wck5 and terminate it with a .. Adding this to the payload, the second argument now becomes:
wck54.-------------------------wck5. (36 bytes).

I terminated v.goodusername with a . to act as a null terminator, then added more invalid characters to fill the space up to v.goodpassword:
wck54.-------------------------wck5.-------------------- (56 bytes).

After that, we add the desired password, followed by a ., to finish the process:
wck54.-------------------------wck5.--------------------wck54. (62 bytes).

My final answer would be
wck5
wck54.-------------------------wck5.--------------------wck54.

---





# LLM USAGE 

Q1. 
	Overall, AI was used to lint the text. 
	(a) AI was used to crosscheck the CIA principle and method of spread
	(b) AI was used to crosscheck the CIA principle and provide the idea of the method of spread. I crosschecked through checking on the internet.
	(c) AI was used to cross check the CIA principle and method of spread. Also provided the idea of how zero-day attack can be prevented through the software update.
	(d) AI was used to cross check the CIA principle and method of spread. Also provided the idea of how configuring firewall rules can prevent the attack. 
Q2.
	Overall, AI was used to lint the text.
	For (b), AI provided the idea of Principle of Least Privilege.
	For (c), AI crosschecked with the concept I was not fully sure, how ROP can defeat stack canary.
	For (e), AI crosschecked with the concept I was not fully sure, of how more risky it is for openSSL to be vulnerable. 
Q3.
	Overall, AI was used to lint the text.
	For i), while I was familiar with the Equifax data breach, AI provided the example of the Wannacry Ransomware. I went over the case after.
	For ii), the concept of polymorphism and metamorphism was introduced by AI. 
Q4. 
	Overall, AI was used to lint the text.
	
 
