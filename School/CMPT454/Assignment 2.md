<div><center><h1>Assignment 2</h1></center></div> <div><center><h3>Wonchan Kim 301449586</h3></center></div>

---

## Q1

### 1. Block Nested Loops (BNL) Join

The BNL join reads a block of the outer relation and then scans the entire inner relation to find matches, repeating this process for every block of the outer relation. To minimize the number of outer blocks, as much memory as possible should be allocated to the outer relation. Specifically, $(B - 2)$ pages are used for the outer relation’s block, one page is used as the input buffer for the inner relation, and one page is used as the output buffer. With $B$ buffer pages, $(B - 2)$ pages of the outer relation can be held in memory at once, allowing the inner relation to be scanned per chunk. When $S$ is chosen as the outer relation (since $|S| < |R|$), the estimated I/O cost is $I/O \approx |S| + \left\lceil \frac{|S|}{B - 2} \right\rceil \cdot |R|$. If $R$ were used as the outer relation, the multiplier would increase, resulting in a higher I/O cost. As $B$ increases, the outer block size $(B - 2)$ grows, reducing the number of outer blocks. Since this term determines how many times the entire inner relation $R$ must be scanned, a larger buffer size $B$ reduces the dominant component of the I/O cost $\left\lceil \frac{|S|}{B - 2} \right\rceil \cdot |R|$.

---

### 2. Index Nested Loops (INL) Join

In an INL join, the algorithm scans the outer relation and, for each tuple, uses an index on the inner relation to locate matching tuples efficiently. Since the index is built on $R$, $R$ must be the inner relation, and $S$ should serve as the outer relation. By scanning $S$ once and probing the index on $R.sid$ for each tuple in $S$, the algorithm utilizes $(B - 2)$ pages as the input buffer for reading $S$ in large chunks. The remaining two pages are used for traversing the index on $R$ and fetching data pages from $R$. The estimated I/O cost is $\text{Cost} \approx |R| + |R| \cdot p_R \cdot (\text{probing cost})$. A larger buffer $B$ improves performance by increasing the input buffer size for $S$, thereby reducing I/O during the scan. Additionally, the extra memory can be used to cache the non-leaf pages of the B+ tree index on $R$. If $B$ is large enough to hold all non-leaf pages of the index, the I/O cost per probe decreases, reducing the overall second term of the cost expression.

---

### 3. Sort-Merge Join (SMJ)

The Sort-Merge Join algorithm first sorts both relations $R$ and $S$ on their join attributes using an external sort, and then performs a merge phase to combine matching tuples. In the sort phase, each relation is divided into sorted runs during Pass 0, with each run up to $B$ pages in length. The total sort cost for each relation $N \in {|R|, |S|}$ is $2N\left(\left\lceil \log_{B-1}\left(\frac{N}{B}\right) + 1 \right\rceil\right)$, accounting for both reading and writing during each pass. After sorting both relations, the merge phase scans the sorted outputs once and produces joined tuples whenever $r_i = s_j$. As SMJ treats both relations symmetrically, there is no distinction between inner and outer relations. The merge phase has a cost of $|R| + |S|$, since each relation is read once sequentially. Thus, the overall cost of SMJ can be expressed as $\text{Cost}_{\text{SMJ}} = 2|R|\left\lceil \log_{B-1}\left(\frac{|R|}{B}\right) + 1 \right\rceil + 2|S|\left\lceil \log_{B-1}\left(\frac{|S|}{B}\right) + 1 \right\rceil + (|R| + |S|)$. When replacement selection is used, each run generated in Pass 0 can be up to $2B$ pages long, effectively halving the number of runs. After Pass 0, each relation has approximately $\lceil N / (2B) \rceil$ runs. If the available buffer size $B$ is sufficiently large such that $B \ge \sqrt{L}$, where $L = \max(|R|, |S|)$, all runs can be merged in a single pass. In that case, the total cost simplifies to $2(|R| + |S|) + (|R| + |S|) = 3(|R| + |S|)$, representing the cost of reading and writing during Pass 0 plus one final read for the merge. As the buffer size $B$ increases, the number of merge passes $\lceil \log_{B-1}(N/B) \rceil$ decreases, reducing the total I/O cost. Larger $B$ also improves merge efficiency by increasing both the number of runs merged per pass and the size of each run. When $B \ge \sqrt{L}$, only two passes (Pass 0 and a single merge) are required, yielding a near-optimal cost of approximately $3(|R| + |S|)$. If $B$ is large enough to hold an entire relation in memory ($B \ge N$), the sort can be performed entirely in memory with an almost linear cost relative to the size of the relation.

---

### 4. Hash Join

The Hash Join algorithm consists of two phases: the partition phase and the probe phase. In the partition phase, both $R$ and $S$ are partitioned into $(B - 1)$ buckets using the same hash function on the join attribute (in this question sid). Memory allocation is as follows: one page is used as the input buffer, and $(B - 1)$ pages are used as output buffers for the partitions. In the probe phase, $(B - 2)$ pages are allocated to build an in-memory hash table for the smaller partition ($S_i$), one page is used for input from $R_i$, and one page for output. The smaller partition should always be chosen as the inner relation to ensure that it fits entirely in memory during probing. Each partition pair $(R_i, S_i)$ is then read back. The smaller partition $S_i$ is loaded into memory, and a hash table is built. The larger partition $R_i$ is scanned, and for each tuple, the algorithm probes the hash table to find matches. If $\min{|R_i|, |S_i|} \le B - 2$, the join for that partition can be done in a single scan, as the smaller partition fits entirely in memory. Assuming equal-sized partitions, $|R_i| = |R| / (B - 1)$ and $|S_i| = |S| / (B - 1)$. Thus, the condition $\min{|R_i|, |S_i|} \le B - 2$ simplifies to $B \ge \sqrt{\min(|R|, |S|)}$. If this condition holds, all partitions can be joined in one pass, and the total cost becomes $2(|R| + |S|) + (|R| + |S|) = 3(|R| + |S|)$. If $B < \sqrt{\min(|R|, |S|)}$, some partitions may still be too large to fit into memory. In this case, the algorithm applies recursive partitioning, performing additional hash joins on oversized partition pairs until they become small enough to fit. Increasing $B$ therefore has a significant effect on reducing I/O cost by lowering the number of partitions and recursive passes required.

---
## Q2

1. Best single index choice would be B+tree as hash is bad for ranges. Search key would be (A,B), for Q1, the index can use A>=7 as a matching predicate to find the start of the range. For Q2, the index can use both A=7 and B >=6 as matching predicates to find the exact starting point. An index on (B,A) would be much less efficient for Q2, as it could only match B>=6. 
   It should be unclustered. A clustered index means the data rows are physically sorted by the index key. However, the data is already sorted on C. Since a table can have only one physical sort order, the index on (A,B) should be unclustered.
   Data alternative should be (key,rid)-Alt2. Alternative1 can be used for clustered. 
2. The B+tree index on (A,B) reduces I/O by avoiding the full file scan on R. 
   For Q1, the index has a matching predicate on A>=7. The query optimizer will traverse the B+tree to find the first leaf entry where A>=7. For each of the index entries satisfying the first condition, the B>=6 is then checked. For entries that match the both, it uses the rid to fetch the data record, faster than reading all 1000 pages of R. 
   For Q2, the index has matching predicates on A=7 and B>=6. First it will traverse the tree to find the first leaf entry matching A=7 and B=6. It then scans the leaf sequentially applying B filter. This is a very narrow search. 
3. Entire record size is 4x, while index entry size is 2x (x = 10)   Hence, the entire page number is 10000/20 = 500.
   Since the leaf page is 500, the B+tree height can be 3 (Assume the fanout is somewhere around 100~200).
   For Query 1 ($A \ge 7$ AND $B \ge 6$), the matching predicate for an index on $(A, B)$ is only the condition $A \ge 7$. The condition $B \ge 6$ must be checked for every entry retrieved by the first condition. The I/O cost for scanning the index entries is calculated as the tree height plus the number of leaf pages accessed, giving $3 + (500 \times 0.1) = 53$ I/Os. The I/O cost for fetching the matching data records is the total number of records multiplied by both reduction factors, resulting in $10{,}000 \times 0.1 \times 0.1 = 100$ I/Os. Therefore, the total cost of the query is $53 + 100 = 153$ I/Os.
   For Query 2 ($A = 7$ AND $B \ge 6$), a B+ tree index with the search key $(A, B)$ is used. In this case, both predicates are matching: the equality condition on $A$ fixes the first key value, and the range condition on $B$ specifies the starting point within that $A$ value. Therefore, the index scan only needs to traverse the leaf segment corresponding to $A = 7$ and $B \ge 6$, which is approximately 1% of the index entries. Given that $R$ has $10{,}000$ records and each condition reduces the search space by 10%, the number of qualifying records is $10{,}000 \times 0.1 \times 0.1 = 100$. Assuming there are $500$ leaf pages in the index and the tree height is $3$, the I/O cost for scanning the index is calculated as the tree height plus the number of relevant leaf pages: $3 + (500 \times 0.01) = 8$ I/Os. Because data records in $R$ are ordered by $C$, the $(A, B)$ index is unclustered, which means fetching each matching record likely requires one data-page I/O. Hence, the total I/O cost of the query is $8 + 100 = 108$ I/Os.


---
## Q3
1. The data entries (k, rid) are sorted by the search key k. The actual data records r are also physically sorted by the search key k on disk.
    
2. The data entries (k, rid) are sorted by the search key k. However, the actual data records r are not sorted by the search key k.
    
3. The leaf nodes directly store the actual data records. Therefore, the data records are sorted by the search key k.
    
4. The leaf nodes store data entries of the form (k, rid), and these entries are sorted by the search key k. The data records are not sorted by k.
    
5. Only possible if the data records are sorted by k. The leaf nodes store entries (k, rid) for only a subset of the total data records. These entries are sorted by k.
    
6. All data entries are sorted by k, and each k value is unique. This property does not specify whether the data records r themselves are sorted by k or not.

---
## Q4
