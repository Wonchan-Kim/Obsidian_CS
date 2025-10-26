<div><center><h1>Assignment 2</h1></center></div>
<div><center><h3>Wonchan Kim 301449586</h2></center></div>


## Q1
1. BNL(block nested loops) Join
   BNL join reads a block of outer relation, then scans the entire inner relation for matches, repeating for every block of outer relation. 
   
   Allocating as much as memory possible to the block of outer relation to minimize the number of blocks. Using B - 2 pages for outer relation's block, 1 page as an input buffer for the inner relation and one page as output buffer. 
   
   With B buffers, we can hold (B-2) pages of the outer at the once and scan the inner per chunk. 
   Cost if S is outer(need to choose S as outer since |S| <|R|), 
   $I/O \approx |S| + \left\lceil \frac{|S|}{B - 2} \right\rceil \cdot |R|$
   If R is outer, the multiplier becomes large, making I/O larger. 
   As B increases, the size of outer block(B-2) increases, which decreases the number of outer blocks (multiplier). Since this indicates how much times we must scan the entire inner relation R, a larger B reduces the dominant term of I/O costs. 
   $\left\lceil \frac{|S|}{B - 2} \right\rceil \cdot |R|$
1.  INL(index nested loops) join
   INL scans the outer relation. For each tuple in outer relation, uses the index on the inner relation to find matching tuples.
   Since the index is on R, R must be inner relation and S should be outer. 
   Make S the outer to scan S once, probing the index on R.sid per S tuple.
   Now we can use B-2 pages as an input buffer for S to read it in large chunks. 
   Remaining two pages are used for traversing index on R and fetching data pages from R.    
   $\text{Cost} \approx |R| + |R| \cdot p_R \cdot (\text{probing cost})$.
   Larger B allows for a larger input buffer for S, making the scan efficient. Also the extra memory is used to cache pages of the B+ trees index on R. If B is large enough to hold all non-leaf pages of the index, the I/O cost for each probe will drop. This reduces the second term of the cost. 

   
2. Sort-Merge join 
The Sort-Merge Join algorithm first sorts both relations $R$ and $S$ on their join attributes using an external sort, and then performs a merge phase to join matching tuples. In the sort phase, each relation is divided into sorted runs during Pass 0, where each run is up to $B$ pages in length. 
The total sort cost for each relation $N \in {|R|, |S|}$ is $2N(\lceil \log_{B-1}(N/B) + 1 \rceil)$, accounting for both reading and writing in each pass.
After sorting both relations, the merge phase scans the sorted outputs once and produces the joined tuples whenever $r_i = s_j$. Hence, in SMJ, there is no concept of inner or outer relation; it is treated symmetrically. The cost of this merge phase is $|R| + |S|$, since each relation is read once sequentially. Thus, the overall cost of the Sort-Merge Join can be expressed as  
$\text{Cost}_{\text{SMJ}} = 2|R|\lceil \log_{B-1}(|R|/B) + 1 \rceil + 2|S|\lceil \log_{B-1}(|S|/B) + 1 \rceil + (|R| + |S|)$.
When replacement selection is used, each run generated in Pass 0 can be up to $2B$ pages long, which halves the number of runs. After Pass 0, each relation has approximately $\lceil N / (2B) \rceil$ runs. If the available buffer size $B$ is large enough such that $B \ge \sqrt{L}$, where $L = \max(|R|, |S|)$, all runs can be merged in a single pass. In that case, the total cost simplifies to $2(|R| + |S|) + (|R| + |S|) = 3(|R| + |S|)$, representing the cost of reading and writing during Pass 0 plus one final read for the merge.
As the buffer size $B$ increases, the number of merge passes $\lceil \log_{B-1}(N/B) \rceil$ decreases, reducing the total I/O cost. Larger $B$ also allows more efficient merging because it increases both the number of runs merged per pass and the size of each run. When $B \ge \sqrt{L}$, only two passes (Pass 0 and a single merge) are needed, giving the near-optimal cost of approximately $3(|R| + |S|)$. If $B$ is large enough to hold an entire relation in memory ($B \ge N$), then the sort can be performed entirely in memory with almost linear cost relative to the size of the relation.
3. Hash Join
Hash join divided into two phases; partition phase & probe phase. First hash both R and S into K partitions using the same hash function on sid. k is chosen as B - 1 here. For each partition i, build in memory hash table on the smaller partition and stream the larger partition (R) to probe for matches.
Memory allocation is as follows; in partition phase, 1 page for input B -1  pages for output buffer. In probe phase, B - 2 pages to build the hash table on S 1 page for input from R 1 for output. 
Inner outer choice matters in probe phase. Small relation should be the inner to build the in-memory hash table, in order to have partition to fit in memory. 