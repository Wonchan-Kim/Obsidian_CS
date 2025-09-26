### <center><b><h1>Assignment 2</h1></b></center>
<center><b>Wonchan Kim 301449586</b></center>

## Q1

![[Pasted image 20250925144031.png]]

Using the GDB debugger, it is possible to get the function address. The buffer needs to be overflowed with the address of the target_function. From the following disassembly, we can check the address is 0x00000000004011bb.

![[Pasted image 20250925145859.png]]

Set the breakpoint at the vulnerable function and run the program.
Since the return address of the function is stored in ($rbp + 8), we can calculate how much we should fill up the buffer.

![[Pasted image 20250925152245.png]]

This indicates there is an 18-byte difference between the buffer and the return address of the vulnerable function.

Hence, we can use the following command:

```bash
printf 'AAAAAAAAAAAAAAAAAA\xbb\x11\x40\x00\x00\x00\x00\x00' > p.txt
```

Then, we can execute the program with the crafted input:

```bash
cat p.txt | ./sof
Enter some text: You entered: AAAAAAAAAAAAAAAAAA@
Yes! You did it!
```

<div class="page-break" style="page-break-before: always;"></div>

## Q2

This question is about exploiting a vulnerability that occurs from heap alignment. There are two malloc calls, and we can check that two variables are aligned on the memory.

![[Pasted image 20250925161409.png]]

We can find that the result of malloc is stored in rbp - 0x10 and 0x8 each. As the variable is a pointer, we can check the value stored in the variable with the following command:

![[Pasted image 20250925160211.png]]

We can observe two things:
1. Values are stored in little-endian (note that 'A' is stored in the third block).
2. The difference between the two memory allocations is 32 bytes, which is 16 in hex ('c' - 'a').

The memory alignment will be as follows:

![[Pasted image 20250925162106.png]]

Hence,

![[Pasted image 20250925162200.png]]

Therefore, we can add 28 random bytes and change the role to admin.

```bash
printf 'AAAAAAAAAAAAAAAAAAAAAAAAAAAA\x34\x13' > hof.txt
```

This worked.

<div class="page-break" style="page-break-before: always;"></div>

## Q3

The Delete user functionality uses the free function. However, looking at the code, there is a possibility that if the add user function is called directly after deletion, the new user will be added at the same memory address. This new user will then be considered the default user since checkprivilege function only uses the address of the default user.

![[Pasted image 20250926160037.png]]

Let's verify this idea.

![[Pasted image 20250926155930.png]]

It is clarified that this code is vulnerable to a Use-After-Free attack. Now, the only step necessary is to modify the privilege to root.

![[Pasted image 20250926160234.png]]

Putting the same inputs into the text file, the attack works.
![[Pasted image 20250926160958.png]]

<div class="page-break" style="page-break-before: always;"></div>

## LLM Usage
Overall, Gemini was used to lint the text, as there were some sentences that were not natural. 
AI was not used to solve any question. 
