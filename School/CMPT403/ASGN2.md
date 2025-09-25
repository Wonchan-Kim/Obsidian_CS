### <center><b><h1>Assignment2</h1></b></center>
<center><b>Wonchan Kim 301449586 </b></center>
<h2>Q1</h2>
![[Pasted image 20250925144031.png]]
![[Pasted image 20250925144137.png]]
with gdb debugger, it is possible to get the function address. Need to overflow the buffer with the address of target_function.
![[Pasted image 20250925145859.png]]
setting the breakpoint at the vulnerable function and running
![[Pasted image 20250925152245.png]]
indicates there are 18 bytes difference between the return address of the vulnerable function. 

Hence, 

printf 'AAAAAAAAAAAAAAAAAA\xbb\x11\x40\x00\x00\x00\x00\x00' > p.txt

cat p.txt |./sof
Enter some text: You entered: AAAAAAAAAAAAAAAAAA�@
Yes! You did it!


<h2>Q2</h2>
This question is exploiting the vulnerability that occurs from heap alignment. There are two mallocs call, and we can check two variables are aligned on the memory.
![[Pasted image 20250925161409.png]]
We can find that the result of malloc is stored in rbp - 0x10 and 0x8 each.
As the variable is the pointer, we can check the value stored in the variable with following command. 
![[Pasted image 20250925160211.png]]
We can check two things: 
1. values are stored in little endian (note that A is stored in the third block)
2. difference between two memory allocation is 32, 16('c'-'a')
The memory alignment will be as following:


printf 'AAAAAAAAAAAAAAAAAAAAAAAAAAAA\x34\x13' >hof.txt

