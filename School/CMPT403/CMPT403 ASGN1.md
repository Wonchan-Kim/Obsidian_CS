Q1.
1. {a} Crypto-jacking
	1. {i} Availability: stolen resources reduces the victim's system performance
	2. Trojan horse: spreads by hiding within something desirable or legitimate, in this question, a useful webpage. Malicious script is executed in the background without their knowledge or consent.
	3. Using the script-blocking plugin for the browser side. Extensions like NoScript can be useful in this case.
2. Backdoor
	1. {i} 
			1. Confidentiality: Backdoor is the random number generator allows attacking to predict the cryptographic keys. With these keys, sensitive datas might be decrypted. 
			2. Integrity: attacker can modify the messages or the digital signatures.
	2. Supply-chain attack: vulnerability was distributed when the legitimate software was installed, with the malicious algorithm included.
	3. One should rely on the cryptographic standards and algorithms that are known to be secure. 
3. Pegasus
	1. {i}  CIA
		1. {1} Confidentiality: designed to secretly access and exfiltrate the private and sensitive data, furthermore even activating the phone functionalities. 
		2. Integrity: malware installs itself on the operating system and can manipulate the device. 
	2. Malware spreads through sending victims malicious file disguised as a GIF file. Clicking or accessing such file will lead to installation of the surveillance software. 
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
1. {a} Some buffer overflow attacks do not overwrite any return address at all is ==TRUE==. Overflows can target for various things, such as target function pointers, heap meta datas, or adjacent variables. 
2. Minimizing privileges in critical programs can help mitigate the impact of the buffer overflow attacks is ==TRUE==. Limiting what compromised code can do helps to mitigate the impact.
3. Return-oriented programming is able to defeat stack canaries is ==TRUE==. Canaries only detect overwriting the return address with arbitrary data but return oriented programming uses existing code fragments. Return oriented programming therefore can bypass the canary protection by hijacking legitimate instructions.
4. XSS attacks usually require the attacker to gain full control over the web server first is ==FALSE==. XSS exploits in web applications, and most cases requires to inject malicious scripts into content that other users use or view, but not gaining the control of entire server. 
5. If there is a format string vulnerability in OpenSSL, it would be more serious bug that Heartbleed is ==TRUE==. While the Heartbleed was an information disclosure bug, such as leaking memory contents, openSSL can lead to direct memory reads or writes. 
---
Q3.
1. {i} The first example is Wannacry Ransomeware attack in 2017. It was an attack exploiting the vulnerability 'SMBv1' of Microsoft Windows. The patch was already distributed two months ago the attack, however, many industries and institutions didn't update accordingly. The second example is Equifax data breach that occured in the same year, compromising the personal information of approximately 148 million people. Attackers managed to steal sensitive data from the credit reporting agency by exploiting a vulnerability within the company’s system. Equifax failed to patch its systems months after a fix was released for this vulnerability, resulting in successful exploitation.
2. Malware are designed to bypass or disable the antivirus detection.
   First of all, while anti virus software relies on the signature-based detection, malware can modify the binary easily each time it spreads, making the malware look different to any known signature. 
   
   Secondly, antivirus is the late response to the known malware. Zero-days exploits can not be successfully prevented by anti virus software.
   
   Thirdly, malware often installs at a low system level such as rootkits making it available to hide from the antivirus software, allowing them to intercept the system calls and files. Code packing can also be used to compress the malicious code.  
3. 
---

Coding Question

Q1.
Looking at the code, 
![[Pasted image 20250922154605.png]]
the vulnerability turns out to be in the part that while the first argument has the buffer overflow vulnerability, yet is modified at the end of the function. 

![[Pasted image 20250922154645.png]]
While the login is only denied by checking the check_failed flag, it is located under the username input. By changing the check_failed flag in to 0 using buffer overflow, it is possible to login to the system.
Since uname_len is 16bytes enter 17bytes total with 0 at the end.

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

My final answer would be

wck5

AAAAAAAAAAAAAAAAAAAAAAAAABBBHVXPwck5CCCCCCCCCCCCCCCCCCCCCAAAAAAAAAAAAAAAAAAAAAAAADwck5CCCCCCCCCCCCCCCCCCCCC


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
Otherwise if it fails all validity check, it modifies the warn_user value to 1.

Since the v.username is set, it is possible to modify the value stored in the v starting from the v.password, as it is sequentially connected.
Important point is to set the length of username and password to be below 24 to pass the while loop test.

Same as question b), there exists the padding. 
After changing the password value, and putting '.' or ',' to set the end of line, 
Hence the second argument should be in the format in the
password. 

Looking back at the code, in order to access the canary without touching it, we can just put the invalid characters to igo over the second argument. 

In my case for the answer, I chose the character '.' Which ascii value is 95.

Taking padding in to consideration, I first wrote
wck54.--------------------- (27).
The reason why it is 27 bytes is because there is one int32 and two char[25], making 54. So two more bytes are needed to match the padding
To invade the canary without changing the value, need to add 4 bytes of invalid character more. 
wck54.-------------------------  (31)

Now we bypassed the canary, hence we can access to the v.goodusername and v.goodpassword. 

We can manipulate both of them by putting the valid Ascii value 
Since my user name is wck5,
I can input wck5 and terminate with either '.' or ','.
Adding this information, now my second argument becomes 
wck54.-------------------------wck5. (36)

I terminated the v.goodusername with '.' so it will be end of line, with invalid characters required to fill up to the v.goodpassword.
wck54.-------------------------wck5.-------------------- (56)

After that, we can add the password. to finish the reading process.
wck54.-------------------------wck5.--------------------wck54. (62)

My final answer would be 
wck5
wck54.-------------------------wck5.--------------------wck54.


