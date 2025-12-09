![[Pasted image 20251208215143.png]]
Assuming all initial value 0, each update on a page increments the value by 1.

Q1. State of the disk pages (P1=3, pageLSN = 10), (P2=3, pageLSN=70), (P3=0, -). T3 commits, and should be persist while others should have no effect. The correct state is therefore (P1=0, ), (P2=0, -), P3(1, 50).
Q2. DPT at checkpoint have one entry, (P2, recLSN=20)
Q3. DPT at crashtime (P3, recLSN=50) (P1, recLSN=60) P2 gets flushed
Q4. DPT after analysis (P2, recLSN=20), (P3, recLSN=50), (P1, recLSN=60)
Q5. LSN70, P2 steal. During analysis however, P2 is not removed because the steal does not tell if steal happened or not during the normal execution. Log record should be flushed to the log before the steal happens (WAL), no guarantee if the steal will happen successfully when writing the log record to the log. 
Q6. state of pages after the redo phase: 
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
At 20: page in DPT, rec_lsn= lsn=20, 