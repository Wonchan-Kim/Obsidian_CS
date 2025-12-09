![[Pasted image 20251208215143.png]]
Assuming all initial value 0, each update on a page increments the value by 1.

Q1. State of the disk pages (P1=3, pageLSN = 10), (P2=3, pageLSN=70), (P3=0, -). T3 commits, and should be persist while others should have no effect. The correct state is therefore (P1=0, ), (P2=0, -), P3(1, 50).
Q2. DPT at checkpoint have one entry, (P2, recLSN=20)
Q3. DPT at crashtime (P3, recLSN=50) (P1, recLSN=60) P2 gets flushed
Q4. DPT after analysis (P2, recLSN=20), (P3, recLSN=50), (P1, recLSN=60)
Q5. LSN70, P2 steal. During analysis however, P2 is not removed because the steal does not tell if steal happened or not during the normal execution. Log record should be flushed to the log before the steal happens (WAL), no guarantee if the steal will happen successfully when writing the log record to the log. 
Q6. state of pages after the redo phase: (Redo starts from smallest num in DPT)
TT: (T1, 100, R), (T2, 90, R), (T3, 80, C)
DPT: (p2, recLSN=20), (P3,50), (P1,60)
disk state:  (P1=3, pageLSN = 10), (P2=3, pageLSN=70), (P3=0, -)
```
for log_record in Logs_from_RedoLSN_to_End:
    page_id = log_record.page_id
    lsn = log_record.LSN

    # 1. DPT에 없으면 이미 Clean이다.
    if page_id NOT in DPT:
        continue (SKIP)

    # 2. DPT에 있어도, 해당 변경사항은 이미 저장되고 난 뒤에 다시 Dirty가 된 것이다.
    rec_lsn = DPT[page_id].RecLSN
    if lsn < rec_lsn:
        continue (SKIP)

    # 3. (비용이 듬) 디스크를 직접 확인했더니 이미 반영되어 있다.
    disk_page = ReadPageFromDisk(page_id)
    if disk_page.PageLSN >= lsn:
        continue (SKIP)

    # 위 3가지를 다 통과해야만 실행
    EXECUTE REDO (Apply Log)
```
At 20: page in DPT, rec_lsn= lsn=20, P2.pageLSN=70 > lsn(20) skip P2 is still (3,70)
At 40: page in DPT, rec_lsn=20, lsn = 40, pageLSN > lsn skip
At 50: P3 in dpt, rec_lsn=50, lsn = 50, pageLSN < lsn -> Redo P3 is now (1, 50)
At 60: P1 in DPT, rec_lsn=10, lsn=60, pageLSN < lsn -> Redo P1 is now (2, 60)
At 70: P2 in DPT, rec_lsn = 20, lsn = 70, pageLSN > lsn, skip
At 90: P3 in DPT, rec_lsn = 0, lsn = 90, pageLsn < lsn, RedoP3 (3, 90)
At 100: P1 in DPT, rec_lsn = 60, lsn = 100, pageLSN < lsn Redo P1(3,100)
Q7. Undo Phase:
TT table (T1, R), (T2, R), (T3, C), T1 and T2 are losers (still running). ToUndo={100,90}
120: Undo100, record "CLR T1 undoLSN=100, undoNextLSN=60", (P1=2, pageLSN=120), toUn={60,90}
130: Undo90,T2 undo LSN = 90 and undoNextLSN = 70”, (P3=1,130), ToUndo ={60,70}
Undo 70 and add record “140: CLR T2 undo LSN = 70 and undoNextLSN = 40”, (P2=2,140), ToUndo ={60,40}.
Undo 60 and add record “150: CLR T1 undo LSN =60, undoNextLSN = 10”, (P1=1,150), ToUndo ={10,40}.
Undo 40 and add “160: CLR T2 undo LSN = 40, undoNextLSN = 20”, (P2=1, 160), ToUndo = {10,20}.
Undo 20 and add “170: CLR T2 undo LSN = 20, undoNextLSN = nil”, (P2=0, 170), ToUndo = {10}.
Add record “180: T2 End”
Undo 10 and add record “190: CLR T1 undo LSN = 10, undoNextLSN = nil”, (P1=0,190), ToUndo=empty
Add record “200: T1 End” (End should have separate log record with updated LSN)
After Undo Phase: (P1=0,190), (P2=0, 170), (P3=1, 130). 
These values are correct because only T3 committed. 
Q8. if log records 80-100 are in the log tail but not in the log at the crash time, what happens to T3 after restart and why? T3 will be undone after restart as commit record is not found in the log, therefore is uncommitted at the crash time
재시작해서 **디스크 로그**만 읽어 보면 T3 관련 로그는:
- 50: T3 updates P3
- (80: T3 commits) ❌ **없음** ← log tail 이라서 사라짐
그러면 Analysis Phase에서 TT를 만들 때:
- T3는 **commit 레코드가 없으므로** “아직 끝나지 않은 트랜잭션”으로 보임  
    → loser 트랜잭션.
- Undo Phase에서 T3의 lastLSN = 50 으로 보고 **LSN 50 의 update 를 undo** 하려고 한다.
즉, _원래는_ commit 했던 T3지만,  
commit 로그가 날아갔기 때문에 시스템 관점에서는
> “T3는 crash 시점에 uncommitted 상태였다 → 다른 loser들과 똑같이 undo 대상”

이 되는 거고, 그래서 답에 **“T3 will be undone”** 이라고 되어 있는 거야.
Q9. if the log records 80-100 are in the log tail but not in the log at the crash time, how are the updates on P3 and P1 at LSN 90 and 100 being undone? No undo is needed for the updates because their log records were not written to the log, which means these updates have not been written to disk pages due to WAL. 디스크는 **LSN 80, 90, 100 이 반영된 페이지를 한 번도 flush 할 수 없다.** 즉, 90 100의 변경은 버퍼 안에만 있었고 사라짐, crash 자체가undo. 
Q4. suppose after adding three CLR records to the log during UNDO phase, system crashes again.
1) TT and DPT after Analysis: TT: TT: (T2,140,R), (T1, 120, R), (T3, 80, C), DPt is empty all redo and undo are applied to disk pages. 
2) Nothing is done during the redo Phase DPT is empty
3) first UNDO step, TT losers are T1 and T2, toUndo={140,120}, Undo 140: CLR record, add 40 to ToUndo {40,120}

Q5. T1 R,W increase the salary by 10%, T2 R,W decrease the salary by 10%. 
Concurrent execution fail to produce correct X value: R R W W (W can be mixed and the effects are different, given X is 1000 in the initial value, can be either 1100 or 900 as one W is ignored)
Q6. 
1. T1:W(X), T2:R(X), T1:W(Y), T1:W(X), T2:W(X), T1:Commit, T2:Commit
   Serializable? No, T2 reads the first write, so T2 should be run after T1, but if T1T2, second Wx will change the first read X of T2. 
   Conflict Serializable? T1->T2(WR, T2 depends on T1), T2->T1(RW), cycle.