# gem5 Basics and Performance Modeling

- **Name:** Wonchan Kim
    
- **Course:** CMPT450
    
- **Date:** 2026-01-26
    

### **Q1: What metric should you use to compare the performance between different system configurations? Why?**

ANS: IPC

**Why?**

The goal of the experiment is to observe the architectural difference(Simple vs O3) in the performance. IPC is an appropriate metric because it directly measures how many instructions a processor can execute in a single clock cycle. 
Furthermore, IPC effectively capture architectural efficiency, independent of clock frequency. Since the purpose of this experiment is to compare microarchitectures under the same clock conditions, IPC is appropriate indicator.         

---
### **Q2: Which benchmark was sensitive to CPU choice? Which benchmark was sensitive to memory model choice? Why?**

#### **1. CPU Choice에 민감한 벤치마크 (CPU Sensitive)**
![[Pasted image 20260126232618.png]]
- **답변:** **`EI`** (가장 민감함), **`MI`**
    
- **근거 (그래프 분석):**
    
    - **CPU Sensitivity Plot (`image_a933d7.png`)**를 보세요.
        
    - **`EI`**의 경우, 파란색(`Simple`) 막대는 1.0 미만으로 매우 낮지만, 빨간색/보라색(`O3_W256`, `O3_W2K`) 막대는 **5.0 이상**으로 치솟았습니다. 약 **5~6배 이상의 성능 향상**이 발생했습니다.
        
    - **`MI`** 역시 `Simple` 대비 `O3` 모델에서 4배 가까운 성능 향상을 보입니다.
        
    - 반면 **`ED1`**은 오히려 고성능 CPU(`O3`)를 썼을 때 성능이 떨어지는 기이한 현상(Negative Sensitivity)을 보입니다. (파이프라인 오버헤드나 분기 예측 실패 때문으로 추정)
        
- **Why? (코드 특성 추론):**
    
    - 이 벤치마크들은 **연산 집약적(Compute-bound)**인 코드로 구성되어 있을 것입니다.
        
    - 반복문 내에서 데이터 의존성(Dependency)이 적고, 순수 계산(사칙연산) 비중이 높습니다.
        
    - 따라서 **Out-of-Order (O3) CPU**가 명령어 병렬성(ILP, Instruction Level Parallelism)을 활용하여 여러 명령어를 동시에 처리함으로써 성능이 급격히 향상된 것입니다.
        

### **Q2: Which benchmark was sensitive to CPU choice? Which benchmark was sensitive to memory model choice? Why?**

#### **1. CPU Choice에 민감한 벤치마크 (Sensitive to CPU)**

- **가장 민감한 벤치마크:** **`EI`** (가장 큰 변화), **`MI`**
    
- **관찰 (Observation):**
    
    - **CPU Sensitivity 그래프**를 보면, `EI`와 `MI`는 `SimpleCPU`에서 `O3_W2K`(보라색)로 변경했을 때 IPC가 **약 5~6배 이상** 폭발적으로 증가했습니다.
        
    - `CCa`와 `CCl`도 3배 정도 증가하여 CPU 성능에 긍정적인 영향을 받습니다.
        
- **이유 (Why):**
    
    - 이 벤치마크들은 **연산 집약적(Compute-bound)**인 성격을 띱니다.
        
    - 프로그램 내에 서로 의존성이 없는 명령어들이 많아, 고성능 CPU(O3)의 **비순차 실행(Out-of-Order Execution)**과 **다중 명령어 발행(Multiple Issue)** 기능을 활용하면 동시에 여러 계산을 처리할 수 있기 때문입니다.
        

#### **2. Memory Model Choice에 민감한 벤치마크 (Sensitive to Memory)**

- **가장 민감한 벤치마크:** **`DP1f`**
    
- **관찰 (Observation):**
    
    - **Memory Sensitivity 그래프**를 보면, 대부분의 벤치마크는 파란색(`SingleCycle`)과 주황색(`Slow`)의 차이가 크지 않습니다.
        
    - 하지만 **`DP1f`**는 메모리가 느려지자(`Slow`) 성능(IPC)이 **약 30% 이상** 눈에 띄게 하락했습니다.
        
- **이유 (Why):**
    
    - 이 벤치마크는 **메모리 집약적(Memory-bound)**입니다.
        
    - CPU가 아무리 빨라도 데이터를 메모리에서 가져오는 데 시간이 오래 걸리면(Memory Latency), CPU는 할 일 없이 대기(Stall)해야 합니다. 따라서 메모리 속도가 전체 성능의 병목(Bottleneck)이 됩니다.
- ### **Q2 Deep Analysis: Code-Level Causality**

#### **1. CPU Sensitive Benchmarks (CPU 선택에 민감한 그룹)**

**해당 벤치마크:** `EI` (최상), `CCi`, `CCa`, `MI` (의외의 결과)

- **`EI` (Efficient Integer)**
    
    - **Code Pattern:**
        
        C
        
        ```
        int t1...t16; // 16개의 독립 변수
        for(...) {
           t1+=i; t2+=i; ... t16+=i; // 상호 의존성 없음
        }
        ```
        
    - **Analysis:** 이 코드는 **ILP(Instruction Level Parallelism, 명령어 수준 병렬성)**를 극대화하도록 설계되었습니다. `t1` 계산은 `t2` 계산 결과를 기다릴 필요가 없습니다.
        
    - **Why O3 Wins:** `SimpleCPU`는 한 번에 하나씩 덧셈을 하지만, `O3CPU`는 여러 개의 정수 연산 유닛(ALU)을 사용하여 한 사이클에 `t1`~`t4` 등을 동시에 처리(Multiple Issue)합니다. 따라서 IPC가 CPU 스펙(Issue Width)에 비례하여 폭발적으로 증가합니다.
        
- **`CCi` (Compute Control intensive)**
    
    - **Code Pattern:**
        
        C
        
        ```
        if(randArr[i]) { t+=3+3*t; ... } // 랜덤 분기 + 많은 연산
        ```
        
    - **Analysis:** `randArr`로 인한 예측 불가능한 분기(Branch)와 루프 내부의 빽빽한 정수 연산이 특징입니다.
        
    - **Why O3 Wins:** `O3CPU`의 **분기 예측기(Branch Predictor)**가 랜덤 패턴을 최대한 예측하려 시도하고, 틀리더라도 **재정렬 버퍼(ROB)**를 통해 투기적 실행(Speculative Execution)을 수행하여 파이프라인 버블을 최소화합니다.
        
- **`MI` (Memory Intensive - 실제로는 L1 Cache Bound)**
    
    - **Code Pattern:**
        
        C
        
        ```
        #define ASIZE 2048 // int(4B) * 2048 = 8KB
        t += arr[i+0]; ... t += arr[i+7]; // Loop Unrolling
        ```
        
    - **Analysis:** 배열 크기가 **8KB**에 불과합니다. 이는 현대 CPU의 L1 Data Cache(보통 32KB~64KB)에 **완벽하게 들어가는 크기(Fit)**입니다.
        
    - **Why CPU Sensitive:** 데이터가 이미 L1 캐시에 들어와 있으므로, 사실상 메모리 접근 속도(DRAM Latency)보다는 **"캐시에서 데이터를 레지스터로 가져와서 더하는 속도"**가 성능을 좌우합니다. `O3CPU`는 Load 명령어를 미리 발행(Prefetching effect by OOO execution)하여 L1 접근 지연시간마저 숨길(Hide) 수 있기 때문에 성능이 크게 향상됩니다.
        

---

#### **2. Memory Sensitive Benchmarks (메모리 모델에 민감한 그룹)**

**해당 벤치마크:** `DP1f`

- **`DP1f` (Double Precision / Data Processing float)**
    
    - **Code Pattern:**
        
        C
        
        ```
        #define ASIZE 8192 // float(4B) * 8192 = 32KB
        float arrA[ASIZE], arrB[ASIZE]; // 총 64KB
        arrA[i]=arrA[i]*3.2f + arrB[i];
        ```
        
    - **Analysis:** 두 배열의 합계 크기는 **64KB**입니다. gem5의 기본 L1 데이터 캐시가 32KB 또는 64KB라면, 이 데이터는 캐시 용량을 초과하거나(Capacity Miss), 다른 데이터와 충돌(Conflict Miss)을 일으킬 가능성이 매우 높습니다.
        
    - **Why Memory Sensitive:** 작업 세트(Working Set)가 L1 캐시보다 크기 때문에 **L2 캐시 또는 DRAM 접근이 필수적**입니다. 따라서 메모리 모델이 `Slow`(DRAM 지연 포함)일 때와 `SingleCycle`일 때의 데이터 공급 속도 차이가 그대로 실행 시간(IPC)에 반영됩니다. 이것이 그래프에서 `DP1f`만 성능이 뚝 떨어진 이유입니다.
        

---

#### **3. The Outlier (CPU 성능 향상이 없거나 적은 그룹)**

**해당 벤치마크:** `ED1`

- **`ED1` (Euclidean Distance? No, Expensive Division)**
    
    - **Code Pattern:**
        
        C
        
        ```
        t1=t1/(zero+1); // 반복문 내에서 자기 자신을 나눔
        ```
        
    - **Analysis:** 핵심 연산이 **나눗셈(`/`)**입니다. 나눗셈은 CPU 연산 중 가장 느리며(Latency가 수십 사이클), 대부분 파이프라인화(Pipelined) 되어 있지 않습니다.
        
    - **Why Low Sensitivity:** 게다가 `t1`의 다음 값은 현재 `t1`의 나눗셈이 끝나야만 계산할 수 있습니다(**Data Dependency Chain**). 아무리 좋은 `O3CPU`라도 앞선 나눗셈이 끝날 때까지 다음 명령어를 실행할 수 없습니다. 병렬 처리가 불가능한 코드이므로 좋은 CPU를 써도 성능 향상이 거의 없습니다.
## 1. Project Overview

This project investigates the impact of **CPU frequency scaling** and **memory technology variations** on system performance using the **gem5 simulator**. By utilizing a diverse set of microbenchmarks, we aim to decouple the effects of processor speed from memory subsystem limitations. The experiments are designed to highlight the differences between **compute-bound** and **memory-bound** workloads and to validate the accuracy of detailed architectural modeling (MinorCPU + DRAMCtrl) versus simplified modeling (SimpleCPU + SimpleMemory).

---

## 2. Experimental Setup (Experiment 3)

To ensure realistic simulation results, **Experiment 3** utilizes a detailed In-order pipeline CPU model and a cycle-accurate DRAM controller.

### 2.1 System Configuration

- **ISA:** x86
    
- **CPU Model:** `Minor4CPU`
    
    - **Type:** 4-issue In-order Pipeline
        
    - **Functional Units:** Defined in `run_micro_exp3.py` (IntALU, IntMultDiv, FP_ALU, etc.) to simulate realistic execution latencies and pipeline stalls.
        
- **Cache Hierarchy:**
    
    - **L1 Instruction Cache:** 32kB, 2-way associative
        
    - **L1 Data Cache:** 32kB, 2-way associative
        
    - **L2 Cache:** 256kB, 8-way associative
        
- **Memory Models (Variable):**
    
    - `DDR3_1600_8x8` (Baseline)
        
    - `DDR3_2133_8x8` (High Frequency)
        
    - `LPDDR2_S4_1066_1x32` (Low Power / Low Bandwidth)
        
    - `HBM_1000_4H_1x64` (High Bandwidth)
        

### 2.2 Microbenchmarks Characteristics

- **`ED1`, `EI`:** Arithmetic heavy, independent instructions. **(Compute-bound)**
    
- **`CCa`:** Arbitrary control flow, prone to branch mispredictions.
    
- **`MI`:** Integer memory access, small array size (8KB). Fits in L1 Cache.
    
- **`DP1f`:** Double-precision floating point, large array size (64KB). Exceeds L1 Cache size (32KB). **(Memory-bound / Streaming)**
    

---

## 3. Analysis & Discussion

### Q7: Frequency Sensitivity Analysis

**Objective:** Analyze how performance scales when CPU frequency increases from 1GHz to 4GHz.

**Observation:** The scaling behavior divides the benchmarks into two distinct groups:

1. **Linear Scaling Group (`ED1`, `EI`, `CCa`, `CCl`, `MI`):** These benchmarks exhibit a speedup factor close to the ideal **4.0x** when frequency is quadrupled.
    
2. **Sub-linear Scaling Group (`DP1f`):** This benchmark shows diminishing returns, achieving significantly less than 4.0x speedup at 4GHz.
    

**Theoretical Explanation:**

- **Compute-Bound Efficiency:** For `ED1` and `EI`, the **Working Set Size** is small enough to reside entirely within the L1 Cache (32kB). Consequently, the CPU rarely stalls for main memory access. The execution time is dominated by ALU and FPU latencies, which scale linearly with clock frequency.
    
- **The Memory Wall (`DP1f`):** The `DP1f` benchmark operates on two 8192-element float arrays (Total ~64KB), which exceeds the L1 Data Cache size. This causes frequent **Capacity Misses**, forcing the CPU to fetch data from DRAM. Since DRAM latency (in nanoseconds) is physical and constant regardless of the CPU's clock speed, the CPU spends more cycles waiting for memory as its frequency increases. This bottleneck confirms **Amdahl’s Law** regarding the non-scalable portion of execution (memory access).
    

---

### Q8: Memory Technology Sensitivity Analysis

**Objective:** Evaluate the impact of different DRAM technologies (DDR3, LPDDR2, HBM) on performance at a fixed 4GHz frequency.

**Observation:**

- **Insensitive Benchmarks:** `ED1`, `EI`, `CCa`, and `MI` show negligible performance difference (<1%) across all memory types.
    
- **Sensitive Benchmark:** `DP1f` shows significant variation:
    
    - **LPDDR2:** Performance degrades by ~25% compared to the baseline.
        
    - **HBM:** Performance is similar to or slightly lower than DDR3-1600.
        

**Theoretical Explanation:**

- **Cache Filtering Effect:** For the insensitive group, the high **Cache Hit Rate** effectively isolates the CPU from main memory. The DRAM technology is irrelevant because memory requests rarely reach the memory controller.
    
- **Bandwidth vs. Latency (`DP1f`):**
    
    - **LPDDR2 Bottleneck:** LPDDR2 is optimized for power, not performance. Its lower clock frequency (1066MHz) and narrower channel width result in insufficient bandwidth for the streaming nature of `DP1f`, causing the pipeline to stall frequently.
        
    - **HBM Underutilization:** While HBM offers massive bandwidth, it requires high **Memory Level Parallelism (MLP)**—multiple concurrent memory requests—to saturate its channels. Since the `Minor4CPU` is a single-core, in-order model, it cannot generate enough concurrent misses to utilize HBM's bandwidth. Instead, the workload suffers from the potentially higher **access latency** of the complex HBM stack compared to standard DDR3.
        

---

### Q9: Benchmark Influence on Architectural Conclusions

**Analysis:** The choice of benchmark is the single most critical factor in architectural evaluation.

1. **Bias towards Compute:** If we only tested `ED1` or `MI`, we would erroneously conclude that **"Memory technology is irrelevant to system performance."** This would lead to over-provisioning CPU frequency while neglecting memory sub-systems.
    
2. **Bias towards Memory:** If we only tested `DP1f`, we would conclude that **"CPU frequency scaling provides diminishing returns."**
    
3. **Correct Methodology:** Real-world applications contain phases of both compute-intensive and memory-intensive operations. Therefore, a balanced suite containing both **Cache-Resident** (e.g., `EI`) and **Cache-Thrashing** (e.g., `DP1f`) workloads is essential to properly characterize the "Memory Wall" and design a balanced system.
    

---

### Q10: Comparison with Baseline Configuration

**Comparison:** Exp 3.2 configurations vs. Baseline (DDR3-1600 @ 4GHz).

- **DDR3-2133:** Showed a slight speedup in `DP1f`. The higher memory clock frequency reduces transfer latency and increases peak bandwidth, directly benefiting the cache-miss-heavy workload.
    
- **LPDDR2:** Showed a drastic slowdown. This highlights the trade-off between **Power and Performance**. While LPDDR2 is suitable for mobile devices, it is a significant bottleneck for high-frequency (4GHz) performance processors running data-intensive tasks.
    
- **HBM:** Did not provide a performance boost over DDR3 for this specific single-core configuration. This indicates that **High Bandwidth Memory requires a matching high-throughput compute engine** (e.g., GPU or Multi-core CPU) to be effective.
    

---

### Q11: Methodology Validation (Simulation Accuracy)

**Question:** Which simulation methodology (Exp 1/2 vs. Exp 3) yields more "correct" and reliable results?

**Answer:** **Experiment 3 (Minor4 CPU + Detailed DRAM Model)** provides the most accurate and reliable representation of real hardware.

**Justification:**

1. **Pipeline Realism (Minor4 vs. Simple):**
    
    - `SimpleCPU` (used in Exp 1/2) uses an **Atomic** execution model. It executes instructions instantaneously (latency = 1 cycle) and ignores data dependencies.
        
    - `Minor4CPU` (used in Exp 3) models a **4-stage In-order Pipeline**. It correctly simulates **Pipeline Stalls** caused by:
        
        - **Data Dependencies:** Waiting for long-latency operations (e.g., Division in `ED1`).
            
        - **Branch Mispredictions:** Flushing the pipeline in `CCa`.
            
        - **Resource Contention:** Limited functional units.
            
    - This allows us to observe that raw frequency scaling does not always translate to linear performance gains due to pipeline hazards.
        
2. **Memory Subsystem Realism (DRAMCtrl vs. Simple):**
    
    - `SimpleMemory` assumes a fixed, constant latency (e.g., 30ns) for all accesses.
        
    - The `DRAMCtrl` in Exp 3 simulates the physical constraints of DRAM, including **Row Buffer Hits/Misses**, **Bank Conflicts**, and **Refresh Cycles**.
        
    - For streaming workloads like `DP1f`, detailed modeling reveals **Queueing Delays** and **Bus Contention** that simplified models completely miss.
        

**Conclusion:** For architectural decision-making, ignoring pipeline dynamics and memory controller state (as in Exp 1/2) leads to optimistic and inaccurate performance projections. The detailed modeling in Exp 3 is required to observe phenomena like the Memory Wall and Bandwidth saturation.