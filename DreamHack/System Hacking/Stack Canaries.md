## Why Stack Canary was introduced

  

![](https://dreamhack-lecture.s3.amazonaws.com/media/2e6f80229f8350969409af607b251cc2cd7f3fe4c82484f5bc52b466dbffe72d.gif)

  

Exploiting stack buffer overflow allowed to modify the return address of the stack. Stack canary was the protection method against stack buffer overflow. 

  

As shown in the gif file above, when the attacker tries to exploit the return address (rbp + 4 or rbp + 8 depending on the x32/x64 system), the attacker who doesn’t know the value stored in the canary will modify the value stored. 

  

> [!note] Canary name origin

> From the bird name yes, Canaries react more sensitive to the carbon monoxide than human, allowing miners to avoid the risk of being addicted to it. 

  

## Basic Canary

  

```c title:canary.c

#include <unistd.h>

  

int main() {

char buf[8];

read(0, buf, 32);

return 0;

}

```

  

Canary is applied naturally in Ubuntu 22.04 gcc compiler. In order to compile without canary applied, need -fno-stack-protector as option.

  

```

$ gcc -o no_canary canary.c -fno-stack-protector

```

  

While we will observe segmentation fault when write more than 32 bytes in no_canary file, compiling again and executing canary binary file will show

  

```

$ gcc -o canary canary.c

$ ./canary

HHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHH

*** stack smashing detected ***: <unknown> terminated

Aborted

```

  

Check the assembly code (disass main)

```

   push   rbp

   mov    rbp,rsp

   sub    rsp,0x10

+  mov    rax,QWORD PTR fs:0x28

+  mov    QWORD PTR [rbp-0x8],rax

+  xor    eax,eax

+  lea    rax,[rbp-0x10]

-  lea    rax,[rbp-0x8]

   mov    edx,0x20

   mov    rsi,rax

   mov    edi,0x0

   call   read@plt

   mov    eax,0x0

+  mov    rcx,QWORD PTR [rbp-0x8]

+  xor    rcx,QWORD PTR fs:0x28

+  je     0x6f0 <main+70>

+  call   __stack_chk_fail@plt

   leave

   ret

  

```

  

## Analyzing Canary Dynamically

![[Pasted image 20250725153404.png]]

```

$ gdb -q ./canary

pwndbg> break *main+12

```
b+ 0x555555555175 <main+12>    mov    rax, qword ptr fs:[0x28]     RAX, [0x7ffff7fb0768] => 0x331ea1c9f446f300

This indicates that the 0x331ea1c9f446f300 (Note that the first byte is Null Byte) is stored in RAX register. ![[Pasted image 20250725153906.png]]
The gdb debugger doesn't allow us to look at the value stored in rax register. 

Going to <main +21> line, note that the value stored in rax is moved to rbp-8. 
[[Segment Registers]]

On <main+54>, move the last 8 bytes to the rdx and compare it with canary value. If not the same call the stack check failed. 