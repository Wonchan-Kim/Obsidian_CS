<center><h1>Assignment3</h1></center>
<center>Wonchan Kim 301449586</center>

<h2>Q1</h2>

1. {a}
Superfish installs its own root certificate into the local trusted certificates store. Hence, the superfish could act as man in the middle between the user's browser and any HTTPS website. As given in the hint, if the user is talking to the website using the website's encryption key, Superfish can't modify the contents because Superfish doesn't have website's decryption key. So instead, browser connects to Superfish instead of directly to the wbesite. Superfish then connects separately to the real website using a legitimate HTTPS connection and receives the genuine encrypted content (intercept).Superfish then decrypts the content using the session established with the real website, then reads and modifies the webpage.  It then encrypts the modified data using a certificate that is generates for the website, signed bby its own trusted root certificate. As the browser already trusts the Superfish's root certificate, it accepts the fake certificate and allows the modification. 
2. Superfish used the same signing key pair on every Lenovo laptop. If an attacker could extract the private key which was the same for everyone, then attacker can use the superfish root private key to generate a fake certificate for any domain. Since every Lenovo laptop with Superfish trust the root certificate, the fake certificate will be accepted as valid. Then the attacker can now conduct Man-in-the-middle attack, presenting the forged certificate and decrypting user's HTTPS traffic. 
3. Careful users could detect in several ways; when clicking the padlock icon on the browser, the user can check the details of certificate. The user would then notice the certificate for the specific website was issued by the Superfish. Also user would be able to check installed certificates in the browser's certificate manager. The user would be able to find the root certificate named Superfish. 


<div class="page-break" style="page-break-before: always;"></div>

## Q3

Two time pad is created when the same encryption key is reused to encrypt two different plaintexts, resulting in two ciphertexts. 

First step 
