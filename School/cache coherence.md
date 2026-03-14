different caches can have different datas if we only have read operations, however if we have write operations, the cache will contain stale data. 

![[Pasted image 20260314143032.png]]
![[Pasted image 20260314143946.png]]
now B will have the stale data. 
1. invalidating all the other - write invalidate protocols
2. updated by A's write - write update protocols. 


