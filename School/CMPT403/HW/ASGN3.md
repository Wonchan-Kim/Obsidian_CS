<center><h1>Assignment3</h1></center>
<center>Wonchan Kim 301449586</center>

<h2>Q1</h2>

1. {a}
Superfish installs its own root certificate into the local trusted certificates store. Hence, the superfish could act as man in the middle between the user's browser and any HTTPS website. As given in the hint, if the user is talking to the website using the website's encryption key, Superfish can't modify the contents because Superfish doesn't have website's decryption key. So instead, browser connects to Superfish instead of directly to the website. Superfish then connects separately to the real website using a legitimate HTTPS connection and receives the genuine encrypted content (intercept).Superfish then decrypts the content using the session established with the real website, then reads and modifies the webpage.  It then encrypts the modified data using a certificate that is generates for the website, signed by its own trusted root certificate. As the browser already trusts the Superfish's root certificate, it accepts the fake certificate and allows the modification. 
2. Superfish used the same signing key pair on every Lenovo laptop. If an attacker could extract the private key which was the same for everyone, then attacker can use the superfish root private key to generate a fake certificate for any domain. Since every Lenovo laptop with Superfish trust the root certificate, the fake certificate will be accepted as valid. Then the attacker can now conduct Man-in-the-middle attack, presenting the forged certificate and decrypting user's HTTPS traffic. 
3. Careful users could detect in several ways; when clicking the padlock icon on the browser, the user can check the details of certificate. The user would then notice the certificate for the specific website was issued by the Superfish. Also user would be able to check installed certificates in the browser's certificate manager. The user would be able to find the root certificate named Superfish. 


<div class="page-break" style="page-break-before: always;"></div>

## Q3

Two time pad is created when the same encryption key is reused to encrypt two different plaintexts, resulting in two ciphertexts. 

First step is to XOR the two ciphertexts together. This cancels out the reused key leaving  with the XOR of two plaintexts. 
C1​=P1​⊕K
C2​=P2​⊕K
C1​⊕C2​=(P1​⊕K)⊕(P2​⊕K)=P1​⊕P2​

Crib is a piece of text that you guess or know is part of one of the plaintexts. We can drag this crib across the Xor text by XORing at every possible position. When the crib is aligned a the correct position, the output of the XOR operation will reveal the corresponding part of the other plaintext. 

The hint was given on the question, "Padding "s are used if the length of the plaintext was shorter than 650 bytes. 

I could try entering several Padding 
resulting of "__nd of the buffer."

This gave me a great hint to find the article about the buffer. 
Testing more Paddings as a crib, I could get 'at produces more data could cause it to write past the end of the buffer.'
This was the description about the buffer overflow.

I searched up the article of buffer overflow in wikiepdia. 
Could find the corresponding paragraph
"In programming and information security, a buffer overflow or buffer overrun is an anomaly whereby a program writes data to a buffer beyond the buffer's allocated memory, overwriting adjacent memory locations.

Buffers are areas of memory set aside to hold data, often while moving it from one section of a program to another, or between programs. Buffer overflows can often be triggered by malformed inputs; if one assumes all inputs will be smaller than a certain size and the buffer is created to be that size, then an anomalous transaction that produces more data could cause it to write past the end of the buffer."

Using this paragraph, it was possible to find the another plaintext, which was the paragraph of Vigenere-cipher article. 

"The very first well-documented description of a polyalphabetic cipher was by Leon Battista Alberti around 1467 and used a metal cipher disk to switch between cipher alphabets. Alberti's system only switched alphabets after several words, and switches were indicated by writing the letter of the corresponding alphabet in the ciphertext. Later, Johannes Trithemius, in his work Polygraphia (which was completed in manuscript form in 1508 but first published in 1518),[6] invented the tabula recta, a critical component of the Vigenere cipher.[7]
"
These were two plain texts included with "Padding "s used as the Padding. 

<div class="page-break" style="page-break-before: always;"></div>

## Q4

Following the instruction step by step was enough to generate the working code. 

The only function that was not described in detail was to decrypt the entire text part. However, since having the other function completed,

Taking the full IV||ciphertext, chops it into 16byte blocks, and then recovers the message one block at a time by calling the block recovery function on each adjacent pair (previous and target). It collects the recovered plaintext blocks in order, making into the complete plaintext. 
The important part is removing the padding bytes to return the original message. 

