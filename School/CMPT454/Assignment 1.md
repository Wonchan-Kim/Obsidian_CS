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
3. 
   