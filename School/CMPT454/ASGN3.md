<div><center><h1>Assignment 3</h1></center></div> <div><center><h3>Wonchan Kim 301449586</h3></center></div>
Q1 
1. 
   P1: Value = 1, PageLSN = 10 : P1 was updated by T1 at LSN 10 and marked as steal, meaning it was flushed to disk at that moment. Subsequent updates were in the memory but not stolen. 
   
   P2: Value = 3, PageLSN = 70: P2 was updated by T2 at LSN 70 and marked as steal, flushing the value 3 to the disk.
   
   P3: Value = 0, PageLSN = 0: P3 was updated at LSN 50 and 90 but was never stolen, so the disk retains the initial value.
   
   No, the state is incorrect, as the database is in an inconsistent state. T3 committed LSN 80, so P3 should reflect T3's update but the disk has value 0. T1 and T2 are uncommitted but their partial updates are on the disk due to the steal. It contains uncommitted updates from T1 and T2, violating Atomicity. It also misses the committed update form T3, violating durability. 
   
   The correct state should only reflect the effects of the committed transactions. T1 and T2 must be aborted, reverting their changes to the initial value. 
   P1: Value = 0 (initial value, T1 aborted)
   P2: Value = 0 (Initial value, T2 aborted)
   P3: Value = 1(Initial Value 0 + T3's update at LSN 50, T3 was committed)
   
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

<div class="page-break" style="page-break-before: always;"></div>

Question 2 
- Transaction Table (TT):{ (T1, LastLSN=100, Status=Active), (T2, LastLSN=90, Status=Active), (T3, LastLSN=80, Status=Committed) }    
- DPT: { (P2, RecLSN=20), (P3, RecLSN=50), (P1, RecLSN=60) }

Steps performed by Redo phase
(Starting at smallest recLSN = 20)
LSN20 : Skip. Page P2(DiskLSN=70) >= 20
LSN40: Skip, page P2(DiskLSN=70) >= 40
LSN50: Redo. page P3(Disk LSN = 0) < 50. Result : P3 becomes 1, LSN set to 50
LSN60: Redo. page P1(Disk LSN = 10) < 60. Result: P1 becomes 2, LSN set to 60
LSN70: Skip. page P2(Disk LSN=70) >= 70
LSN 80: No action on data pages
LSN 90: Redo. page P3(current LSN=50) < 90. Result: P3 becomes 2, LSN set to 90.
LSN 100: Redo, page P1(current lsn = 60) < 100 Result : P1 becomes 3, LSN set to 100

Redo is skipped if pageLSN saved on disk >= Log Record LSN. Indicating the update is already present on the page. 

Content of the disk pages after each step

Initial state: P1 = 1 (LSN 10) P2 = 3 (LSN70), P3=0 (LSN0)
After LSN 50: P3 becomes 1 (LSN50)
After LSN 60: P1 becomes 2 (LSN 60)
After LSN 90: P3 becomes 2(LSN 90)
After LSN 100: P1 becomes 3(LSN100)
