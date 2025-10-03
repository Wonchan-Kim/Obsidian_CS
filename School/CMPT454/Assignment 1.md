<div><center><h1>CMPT454 Assignment 1</h1></center></div>
<div><center><h3>Wonchan Kim 301449586</h2></center></div>

## Q1

1. Index page will have x branches and x-1 keys. Since key size is 8 and pointer size is 4, $8*(x-1) + 4*x <= 512$. $12x <= 520$. Hence max branch of index page is 43.
2. Since minimum threshold is 50%, $\lceil 43/2 \rceil = 22$. Minimum is 22 branches.
3. Since the index is using Alternative 2 <key,rid>, each entry size is 14 bytes. Also 4 bytes should be reserved to point to the next leaf pointer. 
   $\lfloor (512-4)/14 \rfloor$ = 36.
4. Since maximum is 36, the minimum is 50% of it. 18
5. While the maximum capacity of a leaf page is 36 entries, only 67% is expected to be used, which is about $0.67 \times 36 \approx 24$ entries per leaf. Since the relation contains 1,000,000 records, we need $\lceil 1{,}000{,}000 / 24 \rceil = 41{,}667$ leaf pages. An index page can hold up to 43 pointers, so with 67% utilization the average fanout is approximately $0.67 \times 43 \approx 29$. Starting from the leaves, 41,667 pages reduce to $\lceil 41{,}667 / 29 \rceil = 1{,}437$ at the next level, then $\lceil 1{,}437 / 29 \rceil = 50$, then $\lceil 50 / 29 \rceil = 2$, and finally $\lceil 2 / 29 \rceil = 1$ at the root. Therefore, the B+tree requires a total height of 5.
6. Minimum height would be utilizing 100% of each pages, hence index page having 43 branches and leaf page can have 36 entires. 
   $\lceil 1,000,000 \div 36\rceil = 27778$. 
   $43 ^ {x-1}>=27778$ x = 4.
   Therefore, the minimum height of B+tree is 4.
7. Minimum records with height 3. Since the minimum root pointer is 2, internal minimum pointer is 22, and leaf minimum entries is 18, the answer would be $2\times22\times18=792$ . 
8. Maximum records under the same condition as Q1-7, number of leaves would be $43\times43$ , hence the maximum records would be $1849 \times 36=66,564$. 

<div class="page-break" style="page-break-before: always;"></div>

## Q2
The most effective strategy is; Clustered B+tree on A with alternative 1, since the selectivity of A covers the largest portion of the table and benefits the most from clustering. 
For attribute B, a hash index with alternative 2 should be built as hash index supports the equality queries effectively. 
For attribute C, unclustered B+tree with alternative 2 should be built as it still works relatively efficient compared to using unclustered b+tree with index A, as the selectivity is only 5%. 

<div class="page-break" style="page-break-before: always;"></div>

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

<div class="page-break" style="page-break-before: always;"></div>

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

<div class="page-break" style="page-break-before: always;"></div>

## Q5

 1. The index is clustered and data entries use Alternative 2 `<key, rid>`. When reading the index, we need to access the root, the first leaf page (24, 27, 29), and the second leaf page (33, 34, 38, 39), which results in 3 I/Os. For the data entries, since there are 7 records in total, the best case is when they are stored in 2 pages ($\lceil 7/4 \rceil = 2$). The worst case is when the records are split across three pages (e.g., [x x x 24], [27 29 33 34], [38 39 x x]). Hence, the best case requires 5 I/Os in total, and the worst case requires 6 I/Os in total.

 2. A similar approach is used as in Q5-1, with 3 I/Os for the index (root + two leaf pages). The best case is the same, when all 7 records are located in only 2 pages. The worst case occurs when each of the 7 records is on a different page, leading to 7 I/Os. Hence, the best case requires 3+2=5 I/Os, and the worst case requires 3+7=10 I/Os.
 
 3. If the index uses Alternative 1, the leaf pages already store the records directly in sorted order. Therefore, both best and worst cases are the same: reading the root and the two leaf pages, for a total of 3 I/Os.