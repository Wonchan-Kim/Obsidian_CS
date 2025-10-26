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
   SMJ first sorts both R and S on the join key sid using external sorting. Then follows the single merge pass, scanning both sorted relations and joining match tuples. 
   
   Cost = sort R + sort S + one merge pass.
   
   
   