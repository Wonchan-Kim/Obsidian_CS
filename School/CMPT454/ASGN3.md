<div><center><h1>Assignment 3</h1></center></div> <div><center><h3>Wonchan Kim 301449586</h2></center></div><p><strong>Question 1</strong></p>

<p><strong>1.</strong><br>
P1: Value = 1, PageLSN = 10. P1 was updated by T1 at LSN 10 and marked as steal, meaning it was flushed to disk at that moment. Subsequent updates to P1 were kept in memory but were not stolen.<br><br>

P2: Value = 3, PageLSN = 70. P2 was updated by T2 at LSN 70 and marked as steal, flushing the value 3 to disk.<br><br>

P3: Value = 0, PageLSN = 0. P3 was updated at LSN 50 and 90 but was never stolen, so the disk retains the initial value 0.<br><br>

This crash-time state is not the correct final database state from a transactional point of view. T3 committed at LSN 80, so P3 should reflect T3’s update, but the disk still has value 0. T1 and T2 are uncommitted, but their partial updates are already on disk due to steal. Thus the state contains uncommitted updates from T1 and T2, violating Atomicity, and it misses the committed update from T3, violating Durability.<br><br>

The correct state should only reflect the effects of committed transactions. T1 and T2 must be aborted, reverting their changes to the initial values, and T3’s committed update on P3 should be preserved:<br>
&nbsp;&nbsp;P1: Value = 0 (initial value, T1 aborted)<br>
&nbsp;&nbsp;P2: Value = 0 (initial value, T2 aborted)<br>
&nbsp;&nbsp;P3: Value = 1 (initial value 0 plus T3’s update at LSN 50, T3 committed)</p>

<p><strong>2.</strong><br>
DPT at the checkpoint: { (P2, recLSN = 20) }. P1 had been stolen at LSN 10, so it was clean at the checkpoint. P2 was first updated at LSN 20, so recLSN(P2) = 20.</p>

<p><strong>3.</strong><br>
DPT at crash time: { (P3, recLSN = 50), (P1, recLSN = 60) }. This set represents the dirty pages in the buffer pool right before the crash. P3 became dirty at LSN 50, and P1 became dirty again at LSN 60. P2 was flushed at LSN 70 and not updated afterwards, so it was clean at the crash time.</p>

<p><strong>4.</strong><br>
DPT at the end of the Analysis phase: { (P2, recLSN = 20), (P3, recLSN = 50), (P1, recLSN = 60) }. Analysis starts from the checkpoint DPT, which initially contains P2. It then scans the log forward: it adds P3 when it sees the update at LSN 50 and adds P1 when it sees the update at LSN 60. Because the log does not indicate that P2 was later flushed and became clean, Analysis conservatively keeps P2 in the DPT.</p>

<p><strong>5.</strong><br>
The DPT at crash time and the DPT after Analysis are different because the Analysis phase reconstructs the DPT using only the log records and the checkpoint. The log does not explicitly record when a page becomes clean on disk, so Analysis cannot know that P2 was flushed at LSN 70 and became clean. It therefore conservatively assumes that P2 is still dirty, whereas the actual in-memory DPT at crash time knew that P2 was clean and no longer needed to be in the DPT.</p>

<p><strong>6.</strong><br>
After the Redo phase, the disk page contents are:<br>
&nbsp;&nbsp;P1: Value = 3, PageLSN = 100<br>
&nbsp;&nbsp;P2: Value = 3, PageLSN = 70<br>
&nbsp;&nbsp;P3: Value = 2, PageLSN = 90<br>
Redo re-applies only those updates whose effects may be missing on disk (based on the DPT and PageLSN checks). This causes the latest updates to P1 and P3 to be repeated, while P2 already reflects its latest update at LSN 70 and is not modified further.</p>

<p><strong>7.</strong><br>
After the Undo phase, the final page contents are:<br>
&nbsp;&nbsp;P1: Value = 0<br>
&nbsp;&nbsp;P2: Value = 0<br>
&nbsp;&nbsp;P3: Value = 1<br>
This result is correct because T3 committed, so its update to P3 must remain, while T1 and T2 are losers and must be completely undone. Undo rolls back all updates from T1 and T2, restoring P1 and P2 to their initial values and leaving only T3’s committed update on P3.</p>

<p><strong>8.</strong><br>
If the log records 80–100 are only in the log tail and are lost at crash time, then T3 will be considered a loser transaction and will be undone. Since the commit record at LSN 80 is not present in the persisted log, the recovery process has no evidence that T3 ever committed, so it cannot treat T3 as a winner.</p>

<p><strong>9.</strong><br>
In that scenario, the updates at LSN 90 and 100 are treated as updates that never happened from the perspective of recovery. Because these log records are missing from the stable log, the Redo phase will never see them and therefore will not reapply them. Due to WAL, these updates also cannot have been safely flushed to disk before their log records were written. The Undo phase will only undo changes recorded in the available log up to LSN 70. Thus, the effects of the missing updates at LSN 90 and 100 are effectively lost and “undone” simply by not being redone.</p>


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

Condition for skipping the redo. 
During the Redo phase, an update log record with LSN i on page P is skipped if P is absent from the Dirty Page Table, if i is lower than the page's RecLSN, or if the on-disk PageLSN is greater than or equal to i; otherwise, the system performs the Redo to re-apply the update and set the PageLSN to i.

Content of the disk pages after each step

Initial state: P1 = 1 (LSN 10) P2 = 3 (LSN70), P3=0 (LSN0)
After LSN 50: P3 becomes 1 (LSN50)
After LSN 60: P1 becomes 2 (LSN 60)
After LSN 90: P3 becomes 2(LSN 90)
After LSN 100: P1 becomes 3(LSN100)

<div class="page-break" style="page-break-before: always;"></div>

Question 3
- Transactions that are undone
    
    - Losers: T1 and T2.  
        At crash time, both T1 and T2 are still active (no commit/abort log record), so they are loser transactions.

    - T3 is not undone.  
        T3 has a commit record at LSN 80, so it is a winner transaction and is excluded from the undo phase.
        
- Undo steps (using ToUndo, starting from log tail LSN 100)
    
    After the redo phase, the disk pages are:
    
    - P1 = 3 (PageLSN = 100)
        
    - P2 = 3 (PageLSN = 70)
        
    - P3 = 2 (PageLSN = 90)
        
    
    Initial ToUndo set (from the TT at the end of analysis):
    
    - ToUndo = { 100 (T1), 90 (T2) }
        
    
    ARIES undo always takes the largest LSN from ToUndo, undoes that record, writes a CLR, and then follows the prevLSN chain of the same transaction.
    
    Step 1 – LSN 100 (T1, P1)
    
    - Action: undo update of P1 at LSN 100  
        P1: 3 → 2
        
    - ToUndo: remove 100, add T1’s prevLSN = 60  
        ToUndo = { 90 (T2), 60 (T1) }
        
    - Log: write a CLR describing the undo of LSN 100
        
    
    Step 2 – LSN 90 (T2, P3)
    
    - Action: undo update of P3 at LSN 90  
        P3: 2 → 1
        
    - ToUndo: remove 90, add T2’s prevLSN = 70  
        ToUndo = { 70 (T2), 60 (T1) }
        
    - Log: write a CLR describing the undo of LSN 90
        
    
    Step 3 – LSN 70 (T2, P2)
    
    - Action: undo update of P2 at LSN 70  
        P2: 3 → 2
        
    - ToUndo: remove 70, add T2’s prevLSN = 40  
        ToUndo = { 60 (T1), 40 (T2) }
        
    - Log: write a CLR describing the undo of LSN 70
        
    
    Step 4 – LSN 60 (T1, P1)
    
    - Action: undo update of P1 at LSN 60  
        P1: 2 → 1
        
    - ToUndo: remove 60, add T1’s prevLSN = 10  
        ToUndo = { 40 (T2), 10 (T1) }
        
    - Log: write a CLR describing the undo of LSN 60
        
    
    Step 5 – LSN 40 (T2, P2)
    
    - Action: undo update of P2 at LSN 40  
        P2: 2 → 1
        
    - ToUndo: remove 40, add T2’s prevLSN = 20  
        ToUndo = { 20 (T2), 10 (T1) }
        
    - Log: write a CLR describing the undo of LSN 40
        
    
    Step 6 – LSN 20 (T2, P2)
    
    - Action: undo update of P2 at LSN 20  
        P2: 1 → 0
        
    - ToUndo: remove 20; T2 has no earlier LSN, so T2 is fully undone  
        ToUndo = { 10 (T1) }
        
    - Log: write a CLR describing the undo of LSN 20
        
    
    Step 7 – LSN 10 (T1, P1)
    
    - Action: undo update of P1 at LSN 10  
        P1: 1 → 0
        
    - ToUndo: remove 10; T1 has no earlier LSN, so T1 is fully undone  
        ToUndo = ∅, so the undo phase terminates
        
    - Log: write a CLR describing the undo of LSN 10
        
    
    Note: log records of T3 (LSN 50 and the commit at LSN 80) never enter ToUndo, because T3 is a committed (winner) transaction. Therefore they are not processed during the undo phase.
    
- Values after the Undo Phase
    - P1: 0
    - P2: 0
    - P3: 1
        
    All effects of loser transactions T1 and T2 have been undone, while the committed update of T3 on P3 is preserved. Thus, the final database state reflects exactly the committed transactions only, as required by ARIES.
    
<div class="page-break" style="page-break-before: always;"></div>

<p><strong>Question 5</strong></p>

<p>1. Concurrent execution<br>
Schedule: R1(X), R2(X), W1(X), W2(X), C1, C2</p>
<ul>
  <li>T1 reads X = 1000.</li>
  <li>T2 reads X = 1000.</li>
  <li>T1 computes 10% increase and writes X = 1100.</li>
  <li>T2 computes 10% decrease based on the old value 1000 and writes X = 900, overwriting T1’s update.</li>
</ul>
<ul>
  <li>This schedule exhibits a Lost Update anomaly. T2 overwrites the update made by T1 because it read X before T1 wrote the new value. T2 is unaware of T1’s change, so T1’s +10% increase is lost in the final result.</li>
</ul>
<p>2. Correct values</p>
<ul>
  <li>Starting from X = 1000, applying a 10% increase and then a 10% decrease (in either order) gives:<br>
      1000 × 1.10 × 0.90 = 990.</li>
  <li>Therefore, the correct final value of X is <strong>990</strong>.</li>
</ul>
<p>3. Value produced by the schedule in (1):</p>
<ul>
  <li>The final value is 900 because T2 writes 900 last, ignoring T1’s +10% update.</li>
</ul>
<div class="page-break" style="page-break-before: always;"></div>

Question 6
![[Pasted image 20251122223258.png]]
