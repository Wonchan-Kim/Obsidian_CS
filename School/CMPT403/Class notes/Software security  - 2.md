Cross-site Scripting (XSS)
input the code in input boxes on a webpage
XSS occurs when a users can write code on a web page

Persistent XSS vulnerability (stored XSS)
Javascript can be injected into the stored data, the content of a webpage is then changed persistently. Eg. comment boxes, profile descriptions, message boards

Reflexed XSS vulnerability
Inject Javascript into a URL through a parameter, victims may not realize it is a malicious URL. 

Rootkits -> Piece of malware for maintaining C&C over a target system(root)
User Rootkits - change, files, programs, libraries...
Kernel Rootkits - change system calls
Changes the behavior of system functionalities to hide itself/some other malware

Bombs - destructive payloads usually in the context of a trojan
Zip bombs - Unzipping creates a very large file
Compiler bombs - Compiling the bomb creates a very large file
Besides destruction, can be used to break certain scans

Spyware - secretly collects data about the user
Pegasus -> developed by software company NSO Group
Jailbreak Iphones with a malicious URL
Reads text msgs, traces the phone, can enable microphone and camera, etc. 
Flexispy -> call logs/recordings, ;...

Ransomware 
1. Encryption
2. Ransom
3. Blackmail
4. Intrusion path

APT ( advanced persistent threat) 
Combinations of multiple infection vectors and spreading strategies, usually using several zero-day vulnerabilities. 
Focused, long-duration attacks
Achieves political/industrial goal (target high-profile personnel)

Covert channel (은밀한 채널)
resources that are not intended to for communication that are used by an attacker to communicate information in a monitored environment without alerting the victim. 
- To retrieve stolen data
- To receive commands
- To update malware
TCP initial sequence number, size of packets, timing, port knocking
sending information via blinking printer




Stream Cipher
> Generates keystream of any length from random seed
> Keystream is pseudorandom
> Seed and Nonce are used only once
> ENC(seed, Nonce)M = Keystream(seed, Nonce) xor M
