different caches can have different datas if we only have read operations, however if we have write operations, the cache will contain stale data. 

![[Pasted image 20260314143032.png]]
![[Pasted image 20260314143946.png]]
now B will have the stale data. 
1. invalidating all the other - write invalidate protocols
2. updated by A's write - write update protocols. 

When will the invalidate-update protocol is better and worse?
: update is an issue if # of processor is high
: update sent to processors that might not need update. 

scenario better for write-update: write interchangeably. (++ -- single write each access, waste of traffic and overhead getting the exclusive permission to write)

![[Pasted image 20260314145935.png]]
can't have more than  one processor accessing the data. Others will have cache miss
need share state - MSI protocol![[Pasted image 20260314150203.png|400]]













