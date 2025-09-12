## 9.1 The memory hierarchy
![[Pasted image 20250911163211.png]]
At top, we have primary storage which consists of cache and main memory providing very fast access to the data. Then comes the secondary storage, which consists of slower devices such as magnetic disks. Tertiary storage is the slowest class of storage devices, such as tapes. 

### 9.1.1 Magnetic disks
Disks support direct access to a desired location and are widely used for database applications. A DBMS provides seamless access to data on disk, applications need not worry about whether the data is in memory or disk. 
![[Pasted image 20250911171210.png]]
> Time
> Seek time: time taken to move the disk heads to the track on which a desired block is located
> Rotational delay: waiting time for the desired block to rotate under the disk head
> Transfer time: actually read or write the data in the block once the head is positioned

DBMS 
Access time = seek time + rotational delay + transfer time

1. Data must be in memory for the DBMS to operate on it
2. The unit for data transfer between disk and main memory is a block; if a single item on a block is needed, the entire block is transferred. Reading or writing a disk block is called an I/O operation. 
3. The time to read or write a block varies, depending on the location of the data

To minimize the time, it is necessary to locate data records on disk. Two 