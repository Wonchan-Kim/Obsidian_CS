## Q1
CIA principle: 
Confidentiality: Information is secret
Integrity: Information is not tampered
Availability: System is usable

(a)
	 i. Availability: Consumes the victim's cpu resources. 

	 ii. Can be classified as the Trojan Horse. It spreads by hiding, in this cas a useful webpage. 
	 iii.  

{a}
{i}

	 Availability: consumes the victim's cpu resources
	integrity: 


1. {A} a list with an upper-roman list ordering
2. This item will show as "B"
3. and this one 
----
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
