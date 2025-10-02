<div><center><h1>CMPT454 Assignment 1</h1></center></div>
<div><center><h3>Wonchan Kim 301449586</h2></center></div>
## Q2
The most effective strategy is; Clustered B+tree on A with alternative 1, since the selectivity of A covers the largest portion of the table and benefits the most from clustering. 
For attribute B, a hash index with alternative 2 should be built as hash index supports the equality queries effectively. 
For attribute C, unclustered B+tree with alternative 2 should be built as it still works relatively efficient compared to using unclustered b+tree with index A, as the selectivity is only 5%. 

## Q3

1. I/O cost is 1 as there is no overflow pages. 
   Search procedure is as following; 
   First use h(0) 44 mod 4 = 0, 0 < Next=1.
   As the page has been split, use h(1) 44 mod 8 = 4
   Can find 44 at bucket 4(primary page).
2. Procedure is as following;
   Decide which bucket to put 20 -> 20 mod 4 = 0 < NEXT=1
   Use h(1), 20 mod 8 = 4 -> bucket 4.
   Since there are still 2 empty spaces in bucket 4, without overflow or split, 20 is inserted. 
   I/O is 1 read + 1 write => 2 I/O
3. Procedure is as following:
   h2(34) = 34 mod 4 = 2 >NEXT=1. 
   However, bucket 2 is full with {14,18,10,30}. Overflow occurred. (1I/O - read)
   Referring to the algorithm, split the bucket pointed by NEXT. 
   Read page in bucket 1 -> {9,25,41,17} (1 I/O - Read)
   Since h(1) is using mod 8, conduct all keys in bucket 1 with h(1), but still all keys remain in the bucket 1. 
   Write the bucket 1 and bucket 5 to the disk. (2 I/O - Write)
   
   Now increase the NEXT by 1, now NEXT  = 2.
   Coming back to insertion, since split of bucket 1 didn't solve the bucket 2 overflow, need to make a overflow page. 
   Make a new overwrite page with 34 (1 I/O- write)
   Add a pointer of overflow in bucket 2 and write in to disk(1 I/O - write).
   
   Total 6 I/O.
   
   Following figure is for Q3-2 and Q3-3. 
   ![[Pasted image 20251002153316.png]]
## Q4

1. Bucket number can be directly calculated with current level_i and value of next, h_i(k) or h_i+1(k). 
2. In case of overflow, bucket pointed by the NEXT is redistributed using one more hash bit. Repeated splits reduce overflow pages.
3. Extendible hashing uses directory to map hash values to the correct buckets. When a bucket overflows, the directory is doubled and only that bucket is split, with redistribution of the old and new bucket. Multiple directory entries can point to the same bucket. Without the directory, the system would not know which bucket to follow after splits. 
4. Data file itself must be sorted accordingly to the hash key. The data records are stored in the bucket directly, hence physical location of a record on disk can be found by applying the hash function on the key attribute. 
5. When a query retrieves almost the entire table. To be specific, suppose such query exists that read entire table. Regardless of whether the index is clustered or unclustered, nearly all data pages must be read in this case. 
6. RID changes when a data record is forced to relocate. In a clustered B+tree, this happens when the record is moved to another page due to a split or merge. Since the RID consists of the page and slot position, these operations can change the RID of the records.
7. While ISAM is static, a B+tree is dynamic. This requires next-leaf pointers because leaf pages in a B+tree may split and merge as data changes. Although new pages are logically sequential, their physical locations on disk are not guaranteed to be sequential. In contrast, ISAM builds all leaf pages at creation time, so sequential access does not need next-leaf pointers and can rely on the static physical order.
8. Let $d$ be the degree of the B+tree. Split of page A only happens when page A is full. With the current $2d$ entries, there will be $2d+1$ entries to distribute after the new insertion. One page will get $\lfloor \tfrac{2d+1}{2} \rfloor$ entries and the other will get $\lceil \tfrac{2d+1}{2} \rceil$. Both of these values are always greater than or equal to $d$, which satisfies the 50% occupancy requirement.
9. Let $d$ be the degree of the B+tree, so a page can hold up to $2d$ entries and must have at least $d$ entries (except the root). A merge is only triggered when two sibling pages together fit within one page. Suppose page $A$ drops to just below the minimum with $d-1$ entries, and its sibling $B$ has $d$ entries. Then the merged page will hold $(d-1) + d = 2d - 1$ entries, which is less than or equal to the maximum capacity $2d$. Therefore, all entries fit into a single page after the merge.