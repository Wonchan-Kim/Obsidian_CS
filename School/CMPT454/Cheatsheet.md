![[Pasted image 20251208215143.png]]
Assuming all initial value 0, each update on a page increments the value by 1.

Q1. State of the disk pages (P1=3, pageLSN = 10), (P2=3, pageLSN=70), (P3=0, -). T3 commits, and should be persist while others should have no effect. The correct state is therefore (P1=0, ), (P2=0, -), P3(1, 50).
Q2. DPT at checkpoint have one entry, (P2, recLSN=20)
Q3. DPT at crashtime (P3, recLSN=50) (P1, recLSN=60) P2 gets flushed
Q4. DPT after analysis (P2, recLSN=20), (P3, recLSN=50), (P1, recLSN=60)
Q5. LSN70, P2 steal. During analysis however, P2 is not removed because the steal does not tell if steal happened or not during the normal execution. Log record should be flushed to the log before the steal happens (WAL), no guarantee if the steal will happen successfully when writing the log record to the log. 
Q6. 