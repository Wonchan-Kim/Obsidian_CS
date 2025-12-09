![[Pasted image 20251208215143.png]]
Assuming all initial value 0, each update on a page increments the value by 1.

Q1. State of the disk pages (P1=3, pageLSN = 10), (P2=3, pageLSN=70), (P3=0, -). T3 commits, and should be persist while others should have no effect. The correct state is therefore (P1=0, ), (P2=0, -), P3(1, 50).
Q2. DPT at checkpoint have one entry, (P2, recLSN=20)
Q3. DPT at crashtime (P1, recLSN=60), (P1, recLSN=60)