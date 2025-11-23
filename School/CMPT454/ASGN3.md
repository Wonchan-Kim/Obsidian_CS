<div><center><h1>Assignment 3</h1></center></div> <div><center><h3>Wonchan Kim 301449586</h3></center></div>
Q1.
1. 
   P1: Value = 1, PageLSN = 10 : P1 was updated by T1 at LSN 10 and marked as steal, meaning it was flushed to disk at that moment. Subsequent updates were in the memory but not stolen. 
   
   P2: Value = 3, PageLSN = 70: P2 was updated by T2 at LSN 70 and marked as steal, flushing the value 3 to the disk.
   
   P3: Value = 0, PageLSN = 0: P3 was updated at LSN 50 and 90 but was never stolen, so the disk retains the initial value.
   
   No, the state is incorrect, as the database is in an inconsistent state. T3 committed LSN 80, so P3 should reflect T3's update but the disk has value 0. T1 and T2 are uncommitted but their partial updates are on the disk due to the steal. 
   
2. DPT = {(P2, recLSN = 20)}, at P1 had been stolen at LSN 10 so it is clean, and P2 was updated at LSN 20.
3.  DPT at crash time : {(P3, recLSN = 50), (P1, recLSN = 60)}