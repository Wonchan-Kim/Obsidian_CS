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
   
   $\text{Cost} \approx |S| (\text{to read } S) + |S| \cdot (\text{index probe} + \text{matching-record fetches})$.
   INL doesn't use much working memory, but larger B improves caching; the B+tree root/upper levels and hot data pages tend to stay resident, cutting random I/Os.
   
   If R's index is clustered, matching R tuples lie close together. If unclustered, it may incur many random page fetches. 
   
3. 