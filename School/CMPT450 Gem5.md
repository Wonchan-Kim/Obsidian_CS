![[Pasted image 20260116194518.png]]
OOO ( Out of order - Default03CPU): Supports out of order execution. 
INFMemory: Infinite memory, No cache misses, Instantaneous(latency to access data is 0)

CPU connected with dcache_port and icache_port directly without requiring the L1... cache (how 0 latency is implemented)
Interrupt module 

![[Pasted image 20260116194852.png]]
L1 cache (l1i, l1d): now dcache port and ichace_port is not directly connected to mem_ctrl, but to the corresponding L1 cache. 
Data went through L1 cache goes to L2 Cache via L2XBar bus(bus plays the role of mediator).

The first model plays as the simulation testing, while the second model is used for architectural simulation. 


![[Pasted image 20260116201634.png]]

Assign 8 FU in total.