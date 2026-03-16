# Cache Coherence — Shared Memory Multiprocessors

> [!summary]
> 이 슬라이드는 **왜 cache coherence가 필요한지**를 설명하는 배경 슬라이드다.  
> shared memory는 프로그래밍이 쉽지만, 각 프로세서가 자기 캐시를 가지면 같은 데이터의 여러 사본이 생길 수 있다.  
> 따라서 **한 프로세서의 write가 다른 프로세서에게도 올바르게 보이도록** 하려면 hardware cache coherence가 필요하다.

---

## 0. 제목 슬라이드

# Cache Coherence

이 강의 전체 주제가 **캐시 일관성(cache coherence)** 이라는 뜻이다.

왜 이게 중요한가?

멀티프로세서/멀티코어에서 각 코어가 자기 캐시를 가지면 성능은 좋아지지만, 문제가 하나 생긴다.

같은 메모리 주소를 여러 코어가 각자 캐시에 따로 들고 있으면, **어느 값이 진짜 최신 값인지** 맞춰야 한다.

이 문제를 해결하는 것이 **cache coherence**이다.

즉 이 다음 슬라이드부터는  
여러 프로세서가 메모리를 공유하는 시스템에서, 각자의 캐시가 서로 모순되지 않게 만드는 방법을 배우게 된다.

> [!important]
> **cache coherence의 핵심 질문**  
> 여러 캐시에 같은 주소의 복사본이 있을 때,  
> **write 이후 어떤 값이 최신값인지 어떻게 모두가 일관되게 보게 할 것인가?**

---

## 1. Shared Memory Multiprocessors

이 제목은 말 그대로 **메모리를 공유하는 멀티프로세서 시스템**을 뜻한다.

단어를 나누면:

- **Shared Memory**: 메모리가 공유됨
- **Multiprocessors**: 프로세서가 여러 개 있음

즉 여러 프로세서가 **같은 주소 공간(shared address space)** 을 보고 있는 구조라는 뜻이다.

> [!note]
> shared-memory system에서는 프로그래머가  
> "이 메모리는 P0 전용", "저 메모리는 P1 전용"  
> 이런 식으로 생각하지 않고,  
> **하나의 공통 주소 공간을 여러 프로세서가 공유한다**고 생각할 수 있다.

---

## 2. All processors can access all memory

이것은 shared-memory의 가장 핵심 정의이다.

의미:

시스템 안의 어떤 프로세서든, **모든 메모리 위치에 접근할 수 있다.**

예를 들어 메모리에 변수 `X`가 있으면:

- `P0`도 `X`를 읽고 쓸 수 있다.
- `P1`도 `X`를 읽고 쓸 수 있다.
- `P2`도 `X`를 읽고 쓸 수 있다.

### 왜 중요한가

이 말은 프로그래머 입장에서

- "이 변수는 P0 전용 메모리다"
- "이 변수는 P1만 접근 가능하다"

이런 식으로 생각하지 않아도 된다는 뜻이다.

그냥 **하나의 큰 공유 주소 공간**이 있다고 생각하면 된다.

따라서 프로그램은 자연스럽게 다음처럼 작성된다.

- 여러 스레드가 같은 배열에 접근
- 같은 큐(queue)에 접근
- 같은 lock 변수에 접근
- 같은 counter에 접근

그래서 프로그래밍은 편해지지만, 바로 이 때문에 coherence 문제가 생긴다.

> [!example]
> 예를 들어 `counter`라는 변수가 shared memory에 있으면  
> `P0`, `P1`, `P2` 모두 같은 `counter`를 읽고 쓸 수 있다.  
>  
> 즉 "공유"라는 것은 편리하지만, 동시에  
> **누가 언제 어떤 값을 보느냐**가 중요해진다는 뜻이다.

---

## 3. Processors share memory resources, but can operate independently

이 문장은 두 가지를 동시에 말한다.

### (1) share memory resources

프로세서들은 메모리를 공유한다.

즉 다음 같은 자원을 함께 쓴다.

- 같은 DRAM
- 같은 주소 공간
- 경우에 따라 같은 shared cache
- 같은 interconnect

### (2) but can operate independently

그렇다고 한 프로세서가 다른 프로세서의 허락을 받고 움직이는 것은 아니다.

각 프로세서는 독립적으로 다음을 수행할 수 있다.

- instruction fetch
- execute
- load/store
- cache access

즉 `P0`가 뭔가 계산하는 동안, `P1`도 동시에 다른 계산을 할 수 있다.

### 왜 중요한가

바로 여기서 **병렬성(parallelism)** 이 나온다.

- 서로 독립적으로 실행 가능 → 성능 향상 가능
- 하지만 같은 메모리를 공유 → 충돌 / 동기화 / 일관성 문제 발생

즉 이 문장은 shared-memory parallelism의 핵심 trade-off를 말한다.

**독립적으로 빠르게 돌고 싶지만, 메모리는 공유하니까 서로 영향 줄 수 있다.**

> [!important]
> shared-memory multiprocessor의 핵심 trade-off:
>
> - **병렬성**을 얻고 싶다
> - 하지만 **공유 데이터** 때문에 서로 영향을 준다
> - 그래서 coherence와 synchronization이 필요해진다

---

## 4. One processor’s memory changes are seen by all other processors

이 bullet은 shared memory에서 반드시 만족되어야 하는 기대를 말한다.

의미:

한 프로세서가 메모리 값을 바꾸면, 다른 프로세서들도 **결국 그 변화를 볼 수 있어야 한다.**

예를 들어 `P0`가

    flag = 1;

를 실행했으면, `P1`도 나중에 `flag`를 읽을 때 `1`을 볼 수 있어야 한다.

또는 `P0`가

    buffer[i] = 99;

를 저장했으면, `P1`이 같은 위치를 읽을 때 결국 `99`를 봐야 한다.

### 왜 "결국"이라는 표현이 중요한가

실제 하드웨어에서는 각자 캐시가 있어서 값이 **즉시 전체 시스템에 동시에 보이는 것처럼** 동작하지는 않는다.

예를 들어:

- `P0`의 cache에는 새 값이 있음
- `P1`의 cache에는 옛날 값이 남아 있음

이럴 수 있다.

그래서 하드웨어가 coherence protocol을 사용해서  
**최종적으로 다른 프로세서들도 올바른 최신 값을 보게 만들어야 한다.**

즉 이 bullet은:

- **프로그래머 관점의 기대**
- **하드웨어가 만족시켜야 하는 조건**

을 말한다.

> [!warning]
> shared memory라고 해서 모든 코어가 항상 같은 순간에 완전히 동일한 값을 본다고 단순하게 생각하면 안 된다.  
> 실제로는 캐시 때문에 **일시적으로 서로 다른 복사본**을 가질 수 있다.  
> coherence는 이 상황을 제어해서 **올바른 최신 값이 eventually 보이도록** 만든다.

---

## 5. Easier to program

이것은 shared-memory 모델의 큰 장점이다.

의미:

분산 메모리 시스템보다 보통 **프로그래밍하기가 더 쉽다.**

왜 쉬운지는 아래 두 개의 서브 bullet이 설명한다.

---

## 5.1 Communication through shared memory

프로세서끼리 통신하려면 shared-memory 모델에서는 그냥 같은 메모리 변수를 사용하면 된다.

예:

- 한 스레드가 queue에 데이터를 넣음
- 다른 스레드가 같은 queue에서 데이터를 꺼냄

또는

- `P0`가 `result[i]`를 계산해서 저장
- `P1`이 그 값을 읽어서 다음 연산 수행

즉 통신 자체가 "메시지 보내기"가 아니라  
**같은 메모리를 읽고 쓰는 것**으로 표현된다.

### 분산 메모리와 비교

분산 메모리에서는 보통 다음 같은 것을 써야 한다.

- `send`
- `receive`
- explicit message passing

하지만 shared memory에서는:

- 그냥 변수 쓰기
- 그냥 배열 읽기

처럼 보인다.

그래서 프로그래머 입장에서는 훨씬 자연스럽고 쉽다.

> [!note]
> shared memory에서는 **communication = shared variable access** 처럼 보인다.  
> 즉 프로세서 사이 통신이 "특별한 네트워크 메시지"가 아니라  
> 그냥 **메모리 읽기/쓰기**로 표현된다.

---

## 5.2 Synchronization through locks stored in shared memory

통신만 중요한 것이 아니라, 여러 프로세서가 동시에 같은 데이터를 건드리면 충돌하므로 **동기화(synchronization)** 가 필요하다.

shared memory에서는 **lock 자체도 메모리에 저장된 변수**로 둘 수 있다.

예를 들어:

    lock(L);
    counter++;
    unlock(L);

여기서 `L`도 사실 shared memory 안에 있는 어떤 값이다.

즉:

- communication도 shared memory 사용
- synchronization도 shared memory 사용

이것이 shared-memory programming model의 큰 장점이다.

### 왜 중요한가

프로그래머 입장에서는 병렬 프로그램을 쓸 때

- 데이터도 메모리 안에 있고
- lock도 메모리 안에 있고
- condition variable도 메모리 안에 있고
- flags도 메모리 안에 있다

즉 거의 모든 것이 같은 주소 공간 안에서 처리된다.

그래서 모델이 단순하다.

> [!important]
> shared memory 모델의 큰 장점:
>
> - **communication**도 memory로 한다
> - **synchronization**도 memory로 한다
>
> 즉 프로그래밍 모델이 하나로 통일된다.

---

## 6. Need cache coherence in hardware

이 bullet이 이 강의의 핵심 진입점이다.

shared memory가 편한 대신, 그것을 실제 하드웨어에서 성립시키려면  
**cache coherence가 반드시 필요하다.**

### 왜 필요한가

각 프로세서가 캐시를 하나도 안 쓰고 매번 메모리만 직접 보면 일관성 문제는 비교적 단순하다.

하지만 그러면 성능이 너무 느리다.

그래서 실제로는:

- `P0` has cache
- `P1` has cache
- `P2` has cache

각자 로컬 캐시를 사용한다.

이제 같은 변수 `X`를 `P0` cache와 `P1` cache가 동시에 들고 있을 수 있다.

그 상태에서 `P0`가 `X = 5`로 바꾸면, `P1` cache 안에 있는 옛날 `X`는 어떻게 할 것인가?

바로 이 문제를 해결하는 하드웨어가 **cache coherence**이다.

### 더 깊은 의미

프로그래머는 그냥 shared memory라고 생각하고 코드를 짜지만,  
하드웨어는 내부적으로 다음 같은 일을 해야 한다.

- invalidate
- write-back
- read miss 처리
- ownership 관리
- sharer 추적

즉 shared memory의 "쉬운 프로그래밍 모델"은 사실  
**하드웨어가 복잡한 coherence를 해주기 때문에 가능한 것이다.**

> [!danger]
> 성능을 위해 캐시를 쓰면,  
> 같은 주소의 **복사본이 여러 군데 존재**하게 된다.  
>  
> 이때 coherence가 없으면:
>
> - 어떤 코어는 옛날 값
> - 어떤 코어는 새 값
>
> 을 보는 모순이 생길 수 있다.

---

## 7. Need interconnection network between all processors and all memory

이 bullet은 물리적인 연결 구조를 말한다.

의미:

모든 프로세서와 모든 메모리를 연결해주는  
**인터커넥션 네트워크(interconnection network)** 가 필요하다.

왜냐하면 shared memory라고 말하려면  
모든 프로세서가 모든 메모리에 접근할 수 있어야 하기 때문이다.

### interconnection network란

프로세서와 메모리, 캐시 사이를 이어주는 하드웨어 통신 구조이다.

예:

- bus
- crossbar
- ring
- mesh
- NoC (network-on-chip)

### 왜 그냥 선 몇 개로 끝나지 않는가

프로세서 수가 많아질수록 문제가 생긴다.

- 여러 프로세서가 동시에 메모리 접근
- coherence 메시지도 오감
- invalidate도 오감
- data reply도 오감

즉 단순히 "연결되어 있다"만으로 끝나는 것이 아니라,  
**충분한 대역폭과 적절한 구조**가 필요하다.

그래서 뒤에서 snooping이 왜 scalability에 약한지,  
directory가 왜 필요한지와 연결된다.

> [!note]
> shared memory는 단순히 "모두 같은 메모리를 본다"는 추상적 개념이 아니라,  
> 실제 하드웨어에서는  
> **processor-cache-memory를 연결하는 통신 구조**가 꼭 필요하다.

---

## 8. Two Types:

이제 shared-memory multiprocessor를 크게 두 종류로 나누는 것이다.

- UMA
- NUMA

이 구분은  
**모든 메모리 접근이 같은 비용이냐, 아니냐**를 기준으로 한다.

---

## 9. Uniform Memory Architectures (UMA): e.g., Symmetric Multiprocessors

UMA는 **Uniform Memory Access**의 뜻이다.

즉:

어떤 프로세서가 어떤 메모리 위치를 접근하든,  
메모리 접근 시간/비용이 거의 동일하다.

### 왜 uniform인가

메모리가 시스템 관점에서 거의 "한 덩어리"처럼 보이고,  
모든 CPU가 비슷한 거리/비용으로 접근하기 때문이다.

예전의 **SMP (Symmetric Multiprocessor)** 가 대표 예시이다.

### Symmetric의 의미

각 프로세서가 메모리에 대해 대체로 대칭적인 위치를 가진다는 뜻이다.

즉 한 프로세서가 특별히 더 가까운 메모리를 가지지 않는다.

### 장점

- 프로그래밍 모델이 단순하다.
- 메모리 배치 고민이 적다.
- 시스템 구조를 이해하기 쉽다.

### 단점

프로세서 수가 많아질수록 모두가 같은 메모리 시스템과 인터커넥트를 공유하므로 병목이 생긴다.

즉 small/medium scale에는 좋지만,  
크게 키우면 대역폭과 coherence broadcast 때문에 힘들어진다.

> [!example]
> UMA에서는 보통
>
> - `CPU0`가 `X` 읽는 시간
> - `CPU1`가 `X` 읽는 시간
> - `CPU2`가 `X` 읽는 시간
>
> 이 거의 비슷하다고 생각할 수 있다.

---

## 10. Non-Uniform Memory Architectures (NUMA): Access & latency to memory is different

NUMA는 **Non-Uniform Memory Access**이다.

즉:

메모리 접근 시간이 **위치에 따라 다르다.**

어떤 메모리는 "가까운 메모리", 어떤 메모리는 "먼 메모리"가 된다.

### 왜 non-uniform인가

보통 시스템이 여러 노드로 나뉘고, 각 노드가 자기 **local memory**를 가지기 때문이다.

예를 들어:

- `P0`가 자기 노드 메모리 접근 → 빠름
- `P0`가 다른 노드 메모리 접근 → 느림

이렇게 된다.

### shared memory와 모순인가?

아니다. NUMA도 여전히 shared memory이다.

즉 모든 프로세서가 전체 주소 공간에 접근 가능하다.  
다만 **접근 비용이 다를 뿐**이다.

그래서 shared-memory multiprocessor 안에서도 규모가 커지면 NUMA가 많이 등장한다.

### 왜 NUMA를 쓰는가

확장성 때문이다.

- 메모리를 분산시켜 대역폭을 늘리고
- local traffic / remote traffic을 분리하고
- 더 큰 시스템을 만들기 쉽다

하지만 단점은:

- 원격 메모리 접근이 느리다
- 성능이 데이터 배치에 민감하다
- 프로그래밍과 성능 최적화가 어려워질 수 있다

> [!important]
> **NUMA도 shared memory다.**  
>  
> 차이는:
>
> - UMA: 어디를 읽어도 거의 비슷한 비용
> - NUMA: 어떤 메모리는 가깝고, 어떤 메모리는 멀다

---

## 11. 슬라이드 전체가 전달하는 큰 흐름

이 슬라이드는 사실 다음 논리를 만들고 있다.

### 1단계

우리는 shared-memory multiprocessor를 원한다.  
왜냐하면 프로그래밍이 쉽기 때문이다.

### 2단계

그런데 각 프로세서가 독립적으로 실행하면서도 같은 메모리를 공유하면,  
한 프로세서의 변경이 다른 프로세서에게 보여야 한다.

### 3단계

성능 때문에 각자 캐시를 써야 한다.

### 4단계

그러면 shared data가 여러 캐시에 복사될 수 있으므로  
hardware cache coherence가 필요하다.

### 5단계

또 모든 프로세서와 메모리를 연결하는 interconnect도 필요하다.

### 6단계

이런 시스템은 크게 UMA와 NUMA로 나뉜다.

즉 이 슬라이드는 앞으로 배울 coherence protocol들이  
어떤 문제를 해결하려고 등장했는지 배경을 깔아주는 슬라이드다.

> [!summary]
> 이 슬라이드의 전체 스토리:
>
> 1. shared memory는 프로그래밍이 쉽다  
> 2. 하지만 여러 프로세서가 동시에 접근한다  
> 3. 성능 때문에 각자 캐시도 쓴다  
> 4. 그러면 같은 데이터의 여러 사본이 생긴다  
> 5. 그래서 cache coherence가 필요하다

---

## 12. 예시로 다시 보기

### 예시 1: shared memory communication

`P0`가 이렇게 쓴다.

    data = 42;
    flag = 1;

`P1`은:

    while (flag == 0) {}
    print(data);

이 코드는 shared memory라서 가능하다.

- `flag`도 shared
- `data`도 shared

하지만 이 코드가 제대로 동작하려면  
`P0`의 write가 `P1`에게 보여야 하므로 coherence가 필요하다.

> [!example]
> `flag`가 먼저 보이고 `data`가 안 보이면 문제가 생길 수 있다.  
> 즉 shared memory에서는 단순히 "같은 변수 접근"이 가능하다는 것뿐 아니라,  
> **그 변화가 다른 코어에게 올바르게 전달되는 것**이 중요하다.

---

### 예시 2: synchronization through locks

    lock(L);
    sum += x;
    unlock(L);

여기서 `L`도 메모리 안의 값이다.

`P0`, `P1`, `P2`가 모두 같은 `L`을 보고  
누가 critical section에 들어갈지 결정한다.

이 역시 shared memory 모델의 장점이지만,  
동시에 lock 변수도 여러 캐시에 복사될 수 있으므로 coherence가 매우 중요하다.

> [!important]
> lock 변수는 보통 아주 자주 읽히고/쓰이는 shared data이므로  
> coherence가 제대로 안 되면 synchronization 자체가 깨진다.

---

### 예시 3: UMA vs NUMA

#### UMA

- `CPU0`가 `X` 읽음: `100ns`
- `CPU1`가 `X` 읽음: `100ns`
- `CPU2`가 `X` 읽음: `100ns`

거의 비슷하다.

#### NUMA

- `CPU0`가 자기 노드 메모리 `X` 읽음: `80ns`
- `CPU0`가 다른 노드 메모리 `Y` 읽음: `180ns`

이렇게 차이가 난다.

> [!note]
> NUMA에서는 **어디에 데이터를 배치하느냐**가 성능에 직접 영향을 준다.  
> local memory를 많이 쓰면 빠르고, remote memory를 많이 쓰면 느려진다.

---

## 13. 이 슬라이드에서 꼭 잡아야 하는 포인트

### 포인트 1

shared memory는  
**"모두가 같은 주소 공간을 볼 수 있다"** 는 뜻이지,  
**"캐시 없이 그냥 한 메모리만 쓴다"** 는 뜻이 아니다.

### 포인트 2

프로그래밍이 쉬운 이유는  
**communication**과 **synchronization**을 둘 다 shared memory 위에서 표현할 수 있기 때문이다.

### 포인트 3

하지만 실제 하드웨어는 캐시 때문에  
**coherence가 반드시 필요하다.**

### 포인트 4

시스템 규모가 커지면 interconnect와 memory bandwidth가 중요해지고,  
그래서 UMA/NUMA 구분이 생긴다.

> [!tip]
> 시험에서 이 슬라이드를 설명하라고 하면,  
> 아래 흐름만 잡으면 된다:
>
> **shared memory의 장점 → 캐시 사용 → coherence 필요 → interconnect 필요 → UMA/NUMA 구분**

---

## 14. 시험용 한 줄 요약

- **All processors can access all memory**  
  모든 프로세서가 전체 주소 공간을 접근할 수 있다.

- **Processors share memory resources, but can operate independently**  
  메모리는 공유하지만 실행은 병렬로 독립적으로 진행된다.

- **One processor’s memory changes are seen by all other processors**  
  한 프로세서의 write는 다른 프로세서에게도 결국 보여야 한다.

- **Easier to program**  
  communication과 synchronization을 shared memory 위에서 표현할 수 있어 프로그래밍이 쉽다.

- **Need cache coherence in hardware**  
  각 프로세서의 캐시 사본들을 일관되게 유지하기 위해 coherence 하드웨어가 필요하다.

- **Need interconnection network**  
  모든 프로세서와 메모리를 연결하는 충분한 대역폭의 연결망이 필요하다.

- **UMA**  
  모든 메모리 접근 비용이 거의 동일하다.

- **NUMA**  
  메모리 접근 비용이 위치에 따라 다르다.

---

## 15. 다음 내용과의 연결

바로 다음 단계에서 보게 될 논리는 대체로 다음과 같다.

1. shared memory는 좋다.
2. 캐시도 필요하다.
3. 그런데 shared data가 여러 캐시에 복사되면 문제가 생긴다.
4. 그래서 coherence protocol이 필요하다.
5. snooping / directory 같은 방식이 나온다.
6. 규모가 커지면 UMA보다 NUMA와 directory가 중요해진다.

즉 이 슬라이드는 사실상

**왜 cache coherence를 배워야 하는가**

를 설명하는 배경 슬라이드다.

> [!summary]
> 한 줄로 끝내면:
>
> **Shared memory는 프로그래밍을 쉽게 만들지만, 각 프로세서가 자기 캐시를 가지는 순간 여러 사본이 생기므로 hardware cache coherence가 필수다.**!

# Interconnection Networks

> [!summary]
> 이 슬라이드는 shared-memory multiprocessor에서 **왜 interconnection network가 필요한지**, 그리고 **어떤 연결 방식들이 있는지**를 소개하는 슬라이드다.  
> 핵심은 단순히 "연결만 되면 된다"가 아니라, **어떤 topology를 쓰느냐에 따라 비용, 지연, 대역폭, 확장성, coherence 방식까지 달라진다**는 점이다.

---

## 1. 이 슬라이드의 역할

이 슬라이드는 shared-memory multiprocessor에서 왜 **interconnection network**가 필요한지, 그리고 어떤 연결 방식들이 있는지를 소개하는 슬라이드다.

앞 슬라이드에서 이미

- 모든 프로세서가 모든 메모리에 접근해야 하고
- 그러려면 interconnect가 필요하다

는 점을 배웠다.

이번 슬라이드는 그 말을 조금 더 구체적으로 바꾼 것이다.

즉 질문이 이렇게 바뀐다.

**"좋아, 연결은 해야 한다. 그런데 어떤 구조로 연결할 것인가?"**

> [!important]
> 이 슬라이드의 핵심은  
> **interconnect의 존재 자체**보다도,  
> **어떤 topology를 선택하느냐에 따라 시스템 성격이 달라진다**는 점이다.

---

## 2. In a shared memory MP, we need to connect different processors and memory modules

여기서 **MP**는 **multiprocessor**를 뜻한다.

즉 shared-memory multiprocessor에서는  
여러 프로세서와 여러 메모리 모듈을 서로 연결해야 한다는 뜻이다.

### 왜 processor끼리만 연결하면 안 되는가

shared memory 시스템에서는 각 프로세서가 계산만 하는 것이 아니라 다음 같은 일을 해야 한다.

- 메모리 읽기
- 메모리 쓰기
- 캐시 miss 처리
- coherence message 전달

그러므로 연결해야 하는 대상은 단순히 processor들끼리가 아니라,

- processor ↔ memory
- processor ↔ shared cache
- processor ↔ processor (간접적으로 coherence)
- cache ↔ memory

를 모두 포함하는 **전체 통신 구조**여야 한다.

### memory modules라고 한 이유

메모리는 하나의 큰 덩어리일 수도 있지만, 실제 하드웨어에서는 대역폭을 늘리기 위해 보통 여러 **memory module / bank / node**로 나뉜다.

즉 슬라이드는 단순히  
"CPU와 메모리를 선 하나로 연결"  
이 아니라,

**여러 CPU와 여러 memory module을 효율적으로 연결하는 구조 전체**

를 interconnection network라고 부르는 것이다.

> [!note]
> shared memory 시스템이라고 해도 실제 물리 구현에서는  
> 메모리가 여러 모듈로 나뉘는 경우가 많다.  
> 그래서 문제는 "CPU와 memory 연결"이 아니라  
> **많은 CPU와 많은 memory block을 어떻게 효율적으로 연결할 것인가**가 된다.

---

## 3. Types of interconnect

이 부분은 대표적인 interconnect topology를 나열한 것이다.

중요한 건 여기서 각각을 외우는 것보다,

**각 구조가 성능, 비용, 확장성 면에서 trade-off가 다르다**

는 점을 이해하는 것이다.

즉 topology를 볼 때는 항상 이런 질문을 해야 한다.

- 구현이 쉬운가?
- 동시에 많은 통신이 가능한가?
- 코어 수가 늘어나도 버틸 수 있는가?
- coherence 메시지를 잘 처리할 수 있는가?

---

## 4. Shared bus

가장 단순한 방식이다.

모든 프로세서와 메모리가 하나의 **공통 버스(shared bus)** 를 함께 쓴다.

그림으로 생각하면,

하나의 긴 통로가 있고 모든 CPU와 memory가 거기에 매달려 있는 느낌이다.

### 장점

- 구조가 단순하다
- 구현이 쉽다
- **snooping coherence와 잘 맞는다**

왜냐하면 버스에 올라온 요청을 모든 캐시가 동시에 볼 수 있기 때문이다.

### 단점

- 한 번에 사실상 제한된 수의 트랜잭션만 가능하다
- 프로세서 수가 늘어나면 버스가 병목이 된다
- 확장성이 낮다

### 언제 적합한가

- 작은 규모 SMP
- 코어 수가 적은 시스템
- 단순한 설계

즉 shared bus는 이해하기 쉬운 기본형이지만, 큰 시스템에는 잘 안 맞는다.

> [!example]
> bus에서는 `P0`가 메모리 요청을 보내면  
> 다른 모든 노드도 같은 bus를 본다.  
>  
> 그래서 snooping은 쉽지만,  
> 동시에 여러 요청이 몰리면 **모두가 같은 자원을 두고 경쟁**하게 된다.

> [!important]
> **shared bus = 단순 + snooping에 유리 + scalability 낮음**

---

## 5. Crossbar: Fully connected

crossbar는 각 입력이 각 출력으로 갈 수 있는 **전용 경로를 많이 제공하는 완전 연결형 구조**다.

### 직관

`N`개의 processor와 `M`개의 memory module이 있을 때,  
processor 하나가 memory 하나에 접근하는 경로가 비교적 독립적으로 마련되어 있다.

즉 shared bus처럼 모두가 한 줄을 두고 경쟁하는 게 아니라,  
서로 다른 쌍은 동시에 통신할 수 있다.

### 장점

- 병렬성이 높다
- 여러 요청이 동시에 다른 memory module로 갈 수 있다
- 지연이 비교적 작다

### 단점

- 하드웨어 비용이 매우 크다
- 연결 수가 많이 필요하다
- 시스템이 커질수록 복잡도가 급격히 증가한다

### 핵심 포인트

crossbar는 **성능은 좋지만 비싸다.**

즉

- shared bus = 싸고 단순하지만 병목
- crossbar = 빠르지만 비싸고 확장 어려움

이렇게 대비하면 된다.

> [!note]
> crossbar는 거의 "직접 연결에 가까운 풍부한 경로"를 제공하므로  
> contention을 줄이고 성능을 높일 수 있다.  
> 대신 그만큼 **배선과 스위치 비용**이 커진다.

> [!important]
> **crossbar = 낮은 hop/높은 병렬성 + 높은 비용**

---

## 6. Ring

ring은 노드들이 **원형으로 연결된 구조**다.

### 직관

각 노드는 양옆 노드와만 직접 연결되고, 데이터는 링을 따라 돈다.

### 장점

- bus보다 더 나은 확장성
- crossbar보다 구현이 단순
- 배선 비용이 상대적으로 낮음

### 단점

- 목적지까지 여러 hop을 거쳐야 할 수 있음
- 노드 수가 많아지면 평균 지연이 증가
- 트래픽이 몰리면 특정 구간이 병목이 될 수 있음

### 언제 자주 쓰이나

칩 내부(on-chip) interconnect에서 ring이 자주 등장한다.  
왜냐하면 배선이 단순하고 레이아웃이 비교적 쉽기 때문이다.

> [!example]
> ring에서 현재 노드의 반대편에 있는 목적지까지 가려면  
> 메시지가 중간 노드들을 여러 번 거쳐야 한다.  
>  
> 즉 **구조는 단순하지만 hop 수가 늘어날 수 있다.**

> [!important]
> **ring = 단순하고 실용적이지만, 노드 수가 커질수록 hop latency가 부담될 수 있음**

---

## 7. Mesh

mesh는 보통 **2차원 격자 형태**로 노드들을 연결하는 구조다.

### 직관

노드가 바둑판처럼 놓이고,  
각 노드는 위/아래/왼쪽/오른쪽 이웃과 연결된다.

### 장점

- 확장성이 좋다
- 큰 시스템 / 많은 코어에 적합하다
- 링크가 지역적으로 배치되어 배선이 현실적이다

### 단점

- 멀리 있는 노드까지 가려면 여러 hop 필요
- routing이 필요
- 트래픽 패턴에 따라 혼잡(congestion)이 생길 수 있음

### 왜 중요하냐

현대 many-core나 NoC(network-on-chip)에서 mesh는 아주 대표적인 구조다.

즉 규모가 커질수록  
shared bus나 crossbar보다 mesh류가 더 현실적이 된다.

> [!note]
> mesh는 "모든 노드를 다 직접 연결하지는 않지만",  
> **지역적 연결을 규칙적으로 반복**해서 큰 시스템을 만들기 좋다.

> [!important]
> **mesh = 현실적인 배선 + 좋은 확장성 + 여러 hop/routing 필요**

---

## 8. 2-D Torus

2-D torus는 mesh와 비슷하지만, **가장자리들이 서로 다시 연결되어 있는 구조**다.

### mesh와 차이

mesh에서는 가장자리 노드는 연결이 덜 되어 있는데,  
torus에서는 반대편 가장자리와 연결해서 **wrap-around 경로**를 만든다.

예를 들어:

- 맨 왼쪽 노드와 맨 오른쪽 노드가 연결
- 맨 위 노드와 맨 아래 노드가 연결

### 장점

- 평균 hop 수를 줄일 수 있음
- 링크 활용도가 좋아질 수 있음
- mesh보다 경로 선택이 더 유연함

### 단점

- 물리적인 배선이 더 복잡함
- 칩 위에서는 wrap-around 연결이 길어져 구현이 부담될 수 있음

> [!example]
> 일반 mesh에서는 왼쪽 끝에서 오른쪽 끝으로 가려면  
> 칸칸이 이동해야 한다.  
>  
> torus에서는 가장자리끼리 직접 이어져 있어서  
> **더 짧은 우회 경로**가 생길 수 있다.

> [!important]
> **2-D torus = mesh의 hop 문제를 완화하지만, 물리 배선은 더 어려워짐**

---

## 9. Hypercube

hypercube는 각 노드를 **비트 패턴**으로 보고,  
**한 비트만 다른 노드끼리 연결하는 구조**다.

### 직관

노드 수가 `2^k`일 때, 각 노드는 `k`개의 이웃과 연결된다.

예:

- 8노드면 3차원 cube
- 16노드면 4차원 hypercube

### 장점

- 노드 수가 많아져도 평균 hop 수가 비교적 작다
- 이론적으로 통신 효율이 좋다

### 단점

- 구조가 직관적이지 않고 구현이 복잡하다
- 물리 배선이 어렵다
- 현대 상용 칩의 물리적 레이아웃과는 덜 잘 맞는 경우가 많다

### 정리

hypercube는 고전적인 병렬 컴퓨터 topology로 중요하지만,  
실제 현대 칩에서는 mesh/ring 쪽이 더 자주 보인다.

> [!note]
> hypercube는 이론적으로 매우 예쁜 구조지만,  
> 실제 칩 설계에서는 **배선과 물리 배치의 현실성** 때문에 덜 선호될 수 있다.

---

## 10. Number of hops vs. number of links: Compare N processors and M memory modules

이 bullet이 이 슬라이드의 핵심 개념이다.

여기서 교수님이 말하려는 건:

interconnect를 비교할 때는 단순히 "연결되냐 안 되냐"가 아니라  
**몇 개의 링크가 필요한지**와  
**평균적으로 몇 hop을 가야 하는지**를 같이 봐야 한다는 것이다.

---

## 10.1 Number of links

링크 수는 **하드웨어 비용, 배선 복잡도, 면적, 전력**과 연결된다.

- 링크 많음 → 비용 큼
- 링크 적음 → 저렴함

예:

- shared bus: 링크 구조는 단순
- crossbar: 링크 엄청 많음
- mesh: 중간 정도
- hypercube: 규칙적이지만 차원 따라 증가

즉 링크 수는 **구현 비용 쪽 지표**다.

> [!important]
> **number of links = 비용 / 배선 / 면적 / 복잡도**

---

## 10.2 Number of hops

hop은 메시지가 목적지까지 가면서 거치는 **중간 단계 수**다.

예를 들어 ring에서 반대편 노드로 가면 여러 hop이 필요하다.

hop 수가 많아지면 보통:

- latency 증가
- congestion 가능성 증가
- coherence/message round-trip 시간 증가

즉 hop 수는 **성능 / 지연 쪽 지표**다.

> [!important]
> **number of hops = latency / 성능 / 메시지 전달 시간**

---

## 10.3 Trade-off

대부분 topology는 이 두 가지 사이에서 trade-off를 가진다.

### Shared bus

- 링크 수 적음
- hop 개념은 단순
- 하지만 contention 심함

### Crossbar

- hop 적음, 거의 직접 연결
- 하지만 링크 수 많고 비용 큼

### Ring

- 링크 수 적당
- hop 수는 노드 수에 따라 증가

### Mesh

- 링크 수는 crossbar보다 훨씬 적고 현실적
- hop 수는 crossbar보다 큼
- 대신 scalability 좋음

즉 결국 질문은 이거다:

**더 비싼 하드웨어를 써서 짧은 경로를 만들 것인가,  
아니면 싼 하드웨어를 써서 hop을 조금 더 감수할 것인가?**

> [!summary]
> interconnect 설계의 핵심 trade-off:
>
> - **많은 링크** → 빠를 수 있지만 비쌈
> - **적은 링크** → 싸지만 hop이나 contention이 늘 수 있음

---

## 11. 왜 coherence와 연결되냐

이 슬라이드가 단순 네트워크 슬라이드가 아니라  
**cache coherence 강의 안에 들어간 이유**가 중요하다.

coherence protocol은 단지 상태만 바꾸는 논리가 아니고,  
실제로는 **메시지를 네트워크 위로 보내야 한다.**

예:

- invalidate
- read miss request
- write miss request
- fetch
- write-back
- data reply

이 모든 것이 interconnect를 통해 이동한다.

따라서 interconnect가 다르면:

- coherence latency도 달라지고
- scalability도 달라지고
- snooping이 가능한지도 달라진다

예를 들어:

- shared bus는 snooping과 잘 맞음
- distributed network는 directory가 더 적합함

즉 interconnect topology는 **coherence protocol 선택과 직접 연결된다.**

> [!danger]
> coherence는 추상적인 상태 머신만의 문제가 아니다.  
> 실제 하드웨어에서는  
> **그 상태 변화를 실어 나를 interconnect가 반드시 필요하다.**  
>  
> interconnect가 달라지면 coherence 구현 방식 자체도 달라질 수 있다.

---

## 12. 앞 슬라이드와 겹치는 부분 / 새로 추가된 부분

### 이미 앞 슬라이드에서 본 내용

- shared-memory MP에는 interconnect가 필요하다
- UMA / NUMA와 연결 구조는 중요하다

### 이 슬라이드에서 새로 추가된 것

- interconnect의 구체적인 종류들
- topology를 비교하는 기준
- **links vs hops**라는 설계 trade-off

즉 이 슬라이드는 완전 새 주제라기보다는  
앞 슬라이드의 **Need interconnection network**를 더 세부적으로 푼 슬라이드다.

---

## 13. 시험/이해용 핵심 요약

### 한 줄 요약

shared-memory multiprocessor에서는 processor와 memory module을 연결하는 interconnect가 필요하며, interconnect topology는 **비용, 지연, 대역폭, 확장성**을 결정한다.

### 구조별 아주 짧은 요약

- **Shared bus**: 가장 단순, snooping에 유리, 확장성 낮음
- **Crossbar**: 성능 좋음, 완전 연결, 비용 큼
- **Ring**: 단순하고 실용적, hop 증가 가능
- **Mesh**: 확장성 좋음, many-core에 적합
- **2-D Torus**: mesh 개선형, 평균 hop 감소
- **Hypercube**: 이론적으로 효율적, 구현은 복잡

### 비교 기준

- **Number of links** → 비용 / 복잡도
- **Number of hops** → 지연 / 성능

> [!tip]
> 시험에서 topology 비교를 물으면  
> 그냥 이름만 나열하지 말고 반드시
>
> - **구현 비용**
> - **평균 hop**
> - **확장성**
> - **coherence와의 궁합**
>
> 을 같이 말하면 좋다.

---

## 14. 다음 내용과의 연결

이 슬라이드 다음에는 보통 이런 방향으로 연결된다.

어떤 interconnect를 쓰느냐에 따라

- snooping이 가능한지
- broadcast가 감당 가능한지
- directory가 필요한지
- NUMA가 왜 필요한지

같은 문제가 달라진다.

즉 이 슬라이드는 단순히 네트워크 종류를 외우는 슬라이드가 아니라,

**cache coherence가 실제 하드웨어 위에서 어떻게 구현될 수 있는지 이해하기 위한 기반 슬라이드**

라고 보면 된다.

> [!summary]
> 한 줄로 끝내면:
>
> **Interconnect는 processor와 memory를 연결하는 통신 구조이며, topology에 따라 coherence의 성능, 비용, 확장성이 달라진다.**