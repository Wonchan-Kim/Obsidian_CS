<div><center><h1>Assignment 3</h1></center></div> <div><center><h3>Wonchan Kim 301449586</h3></center></div>
Q1.
1. 
   P1: Value = 1, PageLSN = 10 : P1 was updated by T1 at LSN 10 and marked as steal, meaning it was flushed to disk at that moment. Subsequent updates were in the memory but not stolen. 
   
   P2: Value = 3, PageLSN = 70: P2 was updated by T2 at LSN 70 and marked as steal, flushing the value 3 to the disk.
   
   P3: Value = 0, PageLSN = 0: P3 was updated at LSN 50 and 90 but was never stolen, so the disk retains the initial value.
   
   No, the state is incorrect, as the database is in an inconsistent state. T3 committed LSN 80, so P3 should reflect T3's update but the disk has value 0. T1 and T2 are uncommitted but their partial updates are on the disk due to the steal. 
   
2. DPT = {(P2, recLSN = 20)}, at P1 had been stolen at LSN 10 so it is clean, and P2 was updated at LSN 20.
3.  DPT at crash time : {(P3, recLSN = 50), (P1, recLSN = 60)} represents the dirty pages in the buffer pool right before the crash. P3 was dirtied at 50, P1 was dirtied again at 60. P2 was flushed at 70 and not updated subsequently, so it was clean at the crash time. 
4. DPT at the end of Analysis phase: {(P2, recLSN = 20), (P3, recLSN = 50), (P1, recLSN = 60)}:
   Analysis starts with the checkpoint DPT P2. It scans the log forward. It adds P3 at LSN50 and P1 at LSN 60. 
5. The reason why DPT is different: The analysis phase reconstructs the table based on the log records. It can't guarantee that the DPT is identical because the log does not explicitly record when a clean page is written to disk. The analysis phase conservatively assumes P2 is still dirty, whereas the actual memory knew it was clean. 
6. After redo: 
   P1: value = 3, pageLSN = 100
   P2: value =3 , pageLSN = 70
   P3: value = 2, pageLSN = 90
7. Undo:
   P1: value = 0, P2: value = 0, P3: value = 1. 
   This result is correct as T3 committed while T1 and T2 aborted.
8. T3 will be considered a loser transaction and will be undone. Since the commit record (LSN80) is lost, the system does not know T3 committed.
9. They are the updates that never happened from the perspective of the recovered log. Since the records are missing, the Redo phase will not reapply. The undo phase will only undo changes recorded in the available log up to LSN 70. Essentially, these future updates are lost and naturally undone by not being redone. 